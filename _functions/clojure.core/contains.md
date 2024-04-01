---
title: clojure.core/contains?
description: Contains? is a predicate function that returns true if the provided collection contains the provided key.
layout: fn
fn: contains?
lib: clojure.core
programming_language: clojure
backgroundcolor: "#ffba9f"
headingcolor: "#7976fc"
fncolor: "#f79cff"
youtube: https://www.youtube.com/embed/jaI3Hcw-ZaA?si=W8FD78qqWOBGWkOI
published: true
date: 2023-03-28 00:00:00

---

Contains? is a predicate function that returns `true` if the provided collection contains the provided key.

## Example 1

Its simplest use case is checking if a hash set contains a value.

```clojure
(contains?
  #{"Ann" "John" "Matt"}
  "Matt")

=> true
```

## Example 2

Or if a hash map contains a certain key.

```clojure
(contains?
  {:name "Peter" :surname "Johnson"}
  :name)

=> true
```

## Example 3

Contains? can also be used with any keyed sequence, like vectors.

```clojure
(contains?
  [1 3 5]
  1)

=> true
```

However, it is important to remember that contains? cares about keys or indexes, not values. So in this example, even though 5 is a value in the vector, the vector only has 3 indexes namely 0, 1 and 2.

```clojure
(contains?
  [1 3 5]
  5)

=> false
```

## Example 4

The same goes for strings.

```clojure
(contains? "abcde" 2)

=> true
```

```clojure
(contains? "abcde" \a)

=> IllegalArgumentException
```

## Example 5

Contains? can also be used for Java arrays, however not for Clojure lists or queues as these are not keyed sequences.

```clojure
(contains?
  (list "a" "b" "c")
  2)

=> IllegalArgumentException
```
