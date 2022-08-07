---
categories: ["programming"]
date: "2015-09-01T00:00:00Z"
tags: ["clojure", "compojure", "json"]
title: "Compojure Lessons"
---

I recently had the opportunity to migrate a codebase for a work project from
`NodeJS` and `JavaScript` to `Clojure` and `Clojurescript`. I've been trying to
pick up Clojure for months now, but I found it difficult without a solid project.
Since I started porting our code over, I feel that I'm understanding how to do
things in this system at an impressive rate.

That being said, I've hit a few issues along the way that I'd like to document,
both for myself and for others. Some of these are specific to my tech stack,
while others are likely general web stuff.

### Put your 404 handler last. Always.
I spent the better part of an hour trying to figure out why I could access my
index page, but not anything deeper in the app. Everything kept coming up as a
404. And not a Ring 404 page.

My code looked a bit like this.

```clojure
;; some-api.clj
(ns app.some-api
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [compojure.handler :as handler]))
              
(defroutes some-routes
  (GET "/stuff" [] {:apples 2 :oranges 3}))
    
(def some-api
  (handler/api some-routes))
```

```clojure
;; interface.clj
(ns app.interface
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [compojure.handler :as handler]))
              
(defroutes interface-routes
  (GET "/" [] "Hello World!")
  (route/not-found "Not Found"))
    
(def interface
  (handler/site interface-routes))
```

```clojure
;; handler.clj
(ns app.handler
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [app.some-api :refer [some-api]]
            [app.interface :refer [interface]]))
              
 (def app
  (routes interface some-routes))
```

The issue here is subtle, but obvious to the experienced developer. The 404 route
is being injected into the app after the index route, but before anything else. 
Because routes are checked in sequence, the server checks index, fails, moves on
to the 404 route, which acts as a catch-all. The fix is simple.

```clojure
;; handler.clj FIXED
(ns app.handler
...
(def app
  (routes some-routes interface))
```

### Handling raw POST data with Compojure.
I'm pretty new to Clojure. I've only been using it at work for about two weeks.
It's been a pleasant learning curve - the codebase looks quite a bit better, and
is also much easier to reason about.

I ran into one issue with getting raw POST data from a request using Compojure.
I'm not familiar with the system yet, so I try to get something to work, then
look it up on StackOverflow or something.

Looking at the docs for Compojure, it seemed that the proper way of getting the
data from a request is to get the `:params` map from the request. However, due
to passing raw data (I'm using [Postman](https://www.getpostman.com/) to test),
this map was empty - everything was in the `:body` map. It's trivial to get the
data out of this.

```clojure
(POST "some-route" {body :body}
  (-> (slurp body) (do-stuff)))
```

### Clojure vectors to JDBC Arrays (and know what version of JDBC you're using).
In my data set, I need to store 3D geometry data. Using Postgres, I chose to do
this by serializing mesh data (vertices, faces, etc.) to JSON (which Postgres
lets you store!), and turning transform Vector3s into arrays. I initially
attempted to write Postgres types around arrays to ensure that these transform
components could only be arrays of length 3, but this ended up being more
trouble than it was worth.

Anyway, the issue I ran into was this: going from JSON to Clojure maps was
simple (using `clojure.data.json`) but inserting these Clojure data structures
was an issue. Simple data types, like integers and strings, were automatically
converted. Vectors were... not? I don't actually know how they were being
entered into Postgres, but the server kept spitting an error at me. It was
stating that the number of expressions was greater than the number of columns. I
inferred this to mean that vectors weren't being converted into an acceptable
form.

I figured out what I needed to do: translate these vectors to JDBC arrays. This
process isn't as streamlined as it could be, as you need to have an active JDBC
connection to do the conversion. To be efficient, you need to do this in a
transaction before you actually submit your query. That feels weird. But it
isn't even the problem. My problem is even simpler than that.

My vectors were storing data as doubles. Spatial coordinates neeedd to be as
specific as possible, so highest precision is preferable. To convert vectors (or
any Clojure seq) to a JDBC array, you need to call `.createArrayOf`. ex.
`(.createArrayOf conn "double" (into-array [...]))`. The problem here is a
versioning one. I'm using JDBCv4. All the documentation that came up when I
searched for "JDBC array types" listed `double` as an acceptable type. However,
in JDBCv4, this was deprecated in favor of the `float` type. So, my code needed
to look like this: `(.createArrayOf conn "float" (into-array [...]))`. And this
took me longer to realize than I'm proud admitting.

### Types are great.  PostgreSQL does pretty strong type checking. 
For a currentproject, I'm using [Yesql](https://github.com/krisajenkins/yesql)
to build my queries. The best part of this is that you can have casts in your
queries, which makes passing data much easier. For example, I have one record
type that describes a 3D mesh object. The schema is:

```sql
CREATE TABLE mesh(mesh_id serial primary key, name text, mesh_data json, mesh_type mesh_t);
```

The `mesh_t` type is a simple enum type of strings representing primitive mesh
types. Using casts, I can send my mesh type as a string, but have it typed in
queries for safety.

An example of this would be an insert. Such a query using Yesql would look like
this:

```sql
INSERT INTO mesh (mesh_type, mesh_data, name) VALUES (:mesh_type::mesh_t, :mesh_data, :name);
```

I'll add other bits of knowledge and wisdom as I run into them. But that's all
for now.
