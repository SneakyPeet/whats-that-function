---
title: clojure.core/juxt
description: A higher order function that takes one or more functions as arguments, and returns a function. When calling the returned function with zero or more arguments, the arguments are passed to each of the functions originally passed to juxt, and the returned result is a vector containing each result from these functions.
layout: fn
fn: juxt
lib: clojure.core
programming_language: clojure
backgroundcolor: "#99d1db"
headingcolor: "#ee84e3"
fncolor: "#ffb788"
youtube: https://www.youtube.com/embed/6-SGdhYdMGE?si=drpG12T6pnsBKkFh
published: true
date: 2024-04-04 00:00:00
---


A higher order function that takes one or more functions as arguments, and returns a function.

When calling the returned function with zero or more arguments, the arguments are passed to each of the functions originally passed to juxt, and the returned result is a vector containing each result from these functions.

## Example 1

We'll start with a toy example to show the basic functionality. We want to return the result of various math functions applied to two numbers.

```clojure
(defn do-math [a b]
  [(+ a b)
   (- a b)
   (* a b)
   (/ a b)
   (mod a b)
   (max a b)
   (min a b)])

 (do-math 6 3)

 => [9 3 18 2 0 6 3]
```

We can replace this with juxt for a more concise version.

```clojure
(def do-math (juxt + - * / mod max min))

(do-math 6 3)

=> [9 3 18 2 0 6 3]
```

## Example 2

More practically, juxt can be used to extract certain values from a collection of hash-maps, in this case a persons id and name.

```clojure
(->>
  [{:id 1 :name "John"  :age 5}
   {:id 2 :name "Gale"  :age 5}
   {:id 3 :name "Zoe"   :age 7}
   {:id 4 :name "Diana" :age 7}
   {:id 5 :name "Aden"  :age 5}
   {:id 6 :name "Alex"  :age 7}]
  (map (juxt :id :name)))

=>
([1 "John"]
 [2 "Gale"]
 [3 "Zoe"]
 [4 "Diana"]
 [5 "Aden"]
 [6 "Alex"])
```

## Example 3

We can expand on this idea and create a lookup using the into function.

```clojure
(def people
  (->> [{:id 1 :name "John"  :age 5}
        {:id 2 :name "Gale"  :age 5}
        {:id 3 :name "Zoe"   :age 7}
        {:id 4 :name "Diana" :age 7}
        {:id 5 :name "Aden"  :age 5}
        {:id 6 :name "Alex"  :age 7}]
       (map (juxt :id :name))
       (into {})))

(get people 1)

=> "John"

(def people
  (->> [{:id 1 :name "John"  :age 5}
        {:id 2 :name "Gale"  :age 5}
        {:id 3 :name "Zoe"   :age 7}
        {:id 4 :name "Diana" :age 7}
        {:id 5 :name "Aden"  :age 5}
        {:id 6 :name "Alex"  :age 7}]
       (map (juxt :id identity))
       (into {})))

(get people 3)

=> {:id 3 :name "Zoe" :age 7}
```

### Example 4

Juxt can be handy if you want to sort by more than one field, in this case we sort by age first and then by name.

```clojure
(def people
  (->> [{:id 1 :name "John"  :age 5}
        {:id 2 :name "Gale"  :age 5}
        {:id 3 :name "Zoe"   :age 7}
        {:id 4 :name "Diana" :age 7}
        {:id 5 :name "Aden"  :age 5}
        {:id 6 :name "Alex"  :age 7}]
       (sort-by (juxt :age :name))))

=>
({:id 5 :name "Aden"  :age 5}
 {:id 2 :name "Gale"  :age 5}
 {:id 1 :name "John"  :age 5}
 {:id 6 :name "Alex"  :age 7}
 {:id 4 :name "Diana" :age 7}
 {:id 3 :name "Zoe"   :age 7})
```
