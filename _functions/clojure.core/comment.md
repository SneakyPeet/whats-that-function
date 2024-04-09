---
title: clojure.core/comment
description: TODO
layout: fn
fn: comment
lib: clojure.core
programming_language: clojure
backgroundcolor: "#ff5b72"
headingcolor: "#7ad4e0"
fncolor: "#ffad5d"
youtube: https://www.youtube.com/embed/8RpvJeSbgcI?si=Xf7agj1lbUUboo7s
published: true
date: 2024-04-09 00:00:00

---
The comment macro enables rich text comments that fits well into a repl driven development workflow.

It takes any number of expressions as arguments and returns nil.

The expressions in the comment body are not evaluated.

Let's see why this is useful by first comparing the comment macro to other comment types.

## Example 1

To comment out a line, you can prefix it with one or more semicolon.

```clojure
;; (defn hello [s]
;;  (str hello " " s))

;; (hello "World")
;; (hello "Samantha?")
```

Alternatively you can use the ignore next form to comment out the entire next expression. This is chain-able, making it easy to comment out multiple expressions.

```clojure
#_#_#_
(defn hello [s]
  (println (str hello " " s)))

(hello "World")
(hello "Samantha?")

```

The Clojure reader will completely ignore everything that is commented out and your editor will also indicate that fact by removing all syntax highlighting. Your code linter will also ignore any problems in the commented out code.

## Example 2

The first big difference when using the comment macro, is that the comment is a valid expression and thus returns a result, which is always nil.

```clojure
(comment

  (defn hello [s]
    (str "Hello " s))

  (hello "World")
  (hello "Samantha?")

  )

=> nil

```

Because it is a valid expression, it will not be ignored. This means syntax highlighting will still be applied and any linting mistakes will be picked up.

```clojure
(comment

  (defn hello [s]
    (str "Hello " s))

  (hello "World")
  (hellooooo "Samantha?")  ; <- a good editor/linter will highlight this problem

  )

=> nil
```

The Clojure reader will read the comment body, without executing it. That means any invalid Clojure will cause the read to fail.

```clojure
(comment

  (defn hello [s]
    (str "Hello " s))

  (hello "World")
  (hello "Samantha?")

  foo : bar
  )

=> Invalid Token :

```

## Example 3

This makes the comment macro an excellent tool for documenting code usage, whilst ensuring validity of the examples and allowing for easy evaluation at the repl. You will often find it at the bottom of a namespace detailing how to use the namespace.

```clojure
(ns example)

(defn hello [s]
  (str "Hello " s))

(comment

  (hello "World") ;=> "Hello World"
  (hello "Samantha?") ;=> "Hello Samantha?"

  )
```
