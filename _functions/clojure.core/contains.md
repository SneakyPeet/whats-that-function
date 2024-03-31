---
title: clojure.core/contains?
description: TODO
layout: fn
fn: contains?
lib: clojure.core
lang: clojure
backgroundcolor: "#ffba9f"
headingcolor: "#7976fc"
fncolor: "#f79cff"
youtube: https://www.youtube.com/embed/jaI3Hcw-ZaA?si=W8FD78qqWOBGWkOI
published: true
date: 2023-03-28 00:00:00

---

Something should go here

## Example 1


```clojure
(contains? {:name "Pieter"} :name) => true
```

## Example 2


```clojure
(contains? #{"Ann" "John" "Matt"} "Matt") => true
```


## Example 3


```clojure
(contains? ["a" "b" "c"] 2)  => true
```

```clojure
(contains? ["a" "b" "c"] 7)  => false
```

```clojure
(contains? ["a" "b" "c"] 4294967296)  => true
```

REMINDER MEME

## Example 4

```clojure
(contains? (list "a" "b" "c") 2)  => IllegalArgumentException
```

REMINDER MEME


REMINDER Check queue behaviour
