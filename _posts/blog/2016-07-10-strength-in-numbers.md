---
categories: ["python"]
date: "2016-07-10T00:00:00Z"
tags: ["python", "flask", "programming languages"]
title: "Strength in Numbers"
---

I've gone back and forth about my language choices over the years. Some of those
changes have been based on inexperience (Python is really easy to learn), some
on curiosity (Clojure is so different! What are LISPs all about?!). Others have
been based in a kind of misplaced egoism, followed by a quick return to reality:
"Surely *I* am smart enough to use Haskell!" (**reads about monads and is still
mostly confused**).

There was nothing wrong with any of these choices. They all happened at points
in my development that made sense. What I'm learning now, is that there is a lot
of wisdom in returning to your roots. I found it odd that MIT stopped using
SICP, but their rationale was fair: we moved from *programming-by-composition*
to *programming-by-poking*. All the same, I still think there is value there.

SICP gives beginners two differing ways of considering the field of computing.
Using Scheme, you consider computing from the perspective of data and
transformations. These abstractions allow you to move quickly, to build and
compose small pieces, and create large pieces of well-understood, yet robust
software. Using C, you consider the computer as it works in physical terms. How
memory works, how the CPU executes instructions, etc. (IMO, this should be
reinforced more than it is in contemporary computer science).

Both C and Scheme, by design, are simple languages. This was a conscious choice
by the writers of SICP. I'm beginning to favor these choices in my own life as
well. The beauty of simple languages is that they are easy to understand.

At my previous job, I ended up using Clojure and ClojureScript for a large
application. We had used NodeJS and JavaScript in our initial version, but found
ourselves running into a lot of issues with JavaScript itself, that Clojure
seemed to alleviate. We had a great time from there, producing features at a
much faster rate than we did with JS, and it was more performant as far as we
could tell. It wasn't all positive though.

Our employer was a small research lab, and in giving us carte blanche to pick
our stack, they essentially doomed the project at the same time. Funding issues
arose, and I left the project. My former coworker, who I still keep in touch
with, is also looking to leave for similar reasons. When he does, the project
will likely die.

This isn't because the concepts of the project are hard to grasp. Nor is Clojure
a particularly hard language to learn. The problem is a subtle one. Clojure
mostly represents a hurdle because it is a LISP. Many programmers are thrown off
by the parentheses or something. Which is unfortunate because LISP semantics are
really simple.

Python is an interesting case here. Some developers have an issue with
whitespace sensitivity, but aside from that, it is a generally well-favored
language. More interestingly, it has had *significant* uptake by the greater
scientific community. As a result, there are a tremendous number of mature
libraries for doing pretty much anything, from interactive graphing to computer
vision.

It's also incredibly easy to find developers that know Python, even if they
don't have a traditional computer science background. This (in my experience) is
usually not a problem, though developers without a CS background may require
more oversight to adhere to code standards, complexity analysis, etc.

The main point is that by choosing a simple, popular system, you gain strength
in numbers. The problems you run into will hardly be unique, and this alone is
generally worth the tradeoffs in expressiveness or performance.
