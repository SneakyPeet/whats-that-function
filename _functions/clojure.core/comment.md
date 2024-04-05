---
title: clojure.core/comment
description: TODO
layout: fn
fn: comment
lib: clojure.core
programming_language: clojure
backgroundcolor: ""
headingcolor: ""
fncolor: "TODO"
youtube: TODO
published: false
date: 2024-04-02 00:00:00

---

comment is a macro that takes any number of expressions as arguments and returns nil.

The expressions are completely ignored, meaning they are not evaluated.

Lets see why this is useful by first comparing the comment macro to other comment types in clojure

## Example 1

To comment out a line, you can prefix it with one or more `;`.

```clojure
;; (defn hello [s]
;;  (str hello " " s))

;; (hello "World")
;; (hello "Samantha?")
```

You can use the ignore next form to comment out the entire next expression. This is chainable, making it easy to comment out multiple forms.

```clojure
#_#_#_
(defn hello [s]
  (println (str hello " " s)))

(hello "World")
(hello "Samantha?")

```

The Clojure reader will completely ignore everything that is commented out and your editor will also indicate that fact by removing all syntax highlighting.

## Example 2

The first big difference when using the comment macro, is that the comment is a valid expression and thus returns a result, which is always nil.
This means it is best used as a top level form.

```clojure
(comment

  (defn hello [s]
    (str "Hello " s))

  (hello "World")
  (hello "Samantha?")

  )

=> nil

```

Because it is an expression, your editor will still highlight body of the comment and your code linter will still pickup any mistakes made.

```clojure
(comment

  (defn hello [s]
    (str "Hello " s))

  (hello "World")
  (hellooooo "Samantha?")  ; <- a good editor/linter will highlight this problem

  )

=> nil

The Clojure reader will still try and read the body, without executing it. That means any invalid Clojure will cause the read to fail.

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

This makes the comment macro an excellent tool for documenting code usage, whilst still allowing the execution of the expressions in the body.
You will often find it at the bottom of a namespace detailing how to use the namespace.

```clojure

(defn hello [s]
  (str "Hello " s))

(comment

  (hello "World") ;=> "Hello World"

  ,)
```

If the comment body is lenghty, you can add a `,` before the closing bracket to indicate end of the comment. Clojure considers the `,` as whitespace.


## Example 4

There are some libraries like [Rich Comment Tests](https://github.com/matthewdowney/rich-comment-tests) that allows you to execute your comment blocks as tests. This is very useful for unit testing namespace internals.

```clojure

^:rct/test
(comment

  (+ 1 1) ;=> 3

  ,)

(com.mjdowney.rich-comment-tests/run-ns-tests! *ns*)

=>>

; Testing com.mjdowney.rich-comment-tests.example
;
; FAIL in () (example.clj:4)
; expected: (= (+ 1 1) 3)
;   actual: (not (= 2 3))
;
; Ran 1 tests containing 2 assertions.
; 1 failures, 0 errors.
;=> {:test 1, :pass 1, :fail 1, :error 0}
```
