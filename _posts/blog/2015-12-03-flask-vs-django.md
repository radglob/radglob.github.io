---
categories: ["python", "frameworks"]
date: "2015-12-03T00:00:00Z"
tags: ["python", "flask", "django"]
title: "Flask vs. Django"
---

I really like Clojure. A lot. Since I've started using it seriously, I find
myself going to it to solve most of my problems. My previous go-to language was
Python. It has its share of issues (GIL, for example), but overall, it's a joy
to work in. Given its ease of use, it's quite a popular language as well. As
such, it's pretty easy to find developers who know it, or are at least
comfortable with it.

As much as I like Clojure, developer onboarding is a concern when starting a new
project.  Some friends approached me to help them with an idea they had. They're
both non-technical, so any decisions in this realm were left up to me. I would
have loved to use Clojure, but finding people to help develop their app would be
a little harder. So I found myself looking at Python again. Based off my
previous experience, I found myself looking at two web frameworks to build the
backend: [Flask](http://flask.pocoo.org) and
[Django](https://www.djangoproject.com).

I'm fairly familiar with Flask. I've used it to build big and small projects. I
wasn't particularly familiar with Django, so along with doing some
tutorials/reading the documentation, I attended a few local Django meetups. The
last one got cancelled, but I was supposed to give a talk comparing the two
frameworks from my point of view. Since that meetup was cancelled, I've decided
to write up a blog post instead. Note that this is all based off of my somewhat
limited experience.

## Flask
Flask is great. It's a microframework written by [Armin
Ronacher](https://twitter.com/@mitsuhiko). I personally love it. It's incredibly
flexible and easy to use. A basic app, as seen on the Flask homepage is as
simple as this:

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run()
```

This is a pretty straightforward example. You import the library, generate an
app object, designate route handlers, etc. The API is very clean and easy to
understand. For these reasons, I think Flask excels for small projects, but also
for large ones (more on this shortly).

The double-edged sword of Flask is its unopinionated nature. For beginners, this
presents a relatively small surface area that they need to learn. Flask handles
routing, has a templating library, and not much else. When it comes to security,
database integration, and other aspects that larger apps require, it's up to the
developer to determine what to use. Flask really doesn't care. This is good if
you know what you're doing, but can be a bit intimidating if you're new to
development. It's easy to make bad decisions due to inexperience.

## Django
Django takes a very different approach. It's similar to Ruby on Rails -
"batteries-included". Pretty much everything is decided for you: the database
ORM, routing, templating, etc. While this is great for getting an app up
quickly, it's not very flexible. Swapping components, or using Django in a way
besides a monolithic app can present some challenges. That said, it does a great
job of presenting a working development pattern.

The biggest downside is that, like Ruby on Rails, it requires the developers to
learn a rather large system. You have to learn how to do things the "Django
way". Which isn't necessarily a bad thing. It just means that you'll generally
have to focus on learning the system first, then building your app. No
shortcuts. To make a point, the Flask example above tells you most of what you
need to know. There is a short tutorial that goes through templating and some
other features. Django is **much** more complicated, requiring a lengthy
tutorial and likely some digging through documentation to understand all of what
is going on.

## Issues and Strengths
There are a few things surrounding the use of these frameworks worth mentioning.
The Python2/Python3 divide is mostly resolved (you should probably use Python3
unless you have a specific reason not to). Django is very up-to-date with this.
Since most of their components are specific to the Django ecosystem, they are
all up-to-date and use Python3, assuming the version of Django you are using
supports it.

In contrast, while Flask *does* support Python 3, it's noted [on the
website](http://flask.pocoo.org/docs/0.10/python3/) that not all extensions do.

Django also provides long-term support (LTS) releases. If you're using Django
for a large application or simply in a large organization, these may be worth
exploring for their stability.

## Which should I use?
Both Flask and Django are great frameworks. Given their strengths and
weaknesses, I'd recommend the following path for someone wishing to learn web
development in Python.

First, learn Flask. It's pretty small, and most people should be able to wrap
their brain around it fairly quickly. Follow the tutorial. Then, build a small
app. It could be a list making app, a Twitter bot, whatever.

Once you've done that, go and learn Django. Build something larger with it:
maybe a CMS.

Finally, go back and learn how to build a similarly large application in Flask.
This will require you to pick a number of libraries, understand how they fit
together, their tradeoffs, etc.

It's worth noting that these aren't the only web frameworks available for
Python.  [Far from it,
actually](https://github.com/vinta/awesome-python#web-frameworks). Explore all
of the options.
