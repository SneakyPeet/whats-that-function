---
layout: fn
fn: juxt
lib: clojure.core
lang: clojure
backgroundcolor: "#99d1db"
headingcolor: "#ee84e3"
fncolor: "#ffb788"
youtube: https://www.youtube.com/embed/jaI3Hcw-ZaA?si=W8FD78qqWOBGWkOI
published: true
date: 2023-03-28 00:00:00
---


**Juxt!** A higher order function that takes one or more functions as arguments, and returns a function.

When calling the returned function with zero or more arguments, the arguments are passed to each of the functions originally passed to juxt, and the returned result is a vector containing each result from these functions.

## Example 1

In this example we want to return the result of some math functions applied to two values

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

We can replace the function definition with juxt for a more concise version

```clojure
(def do-math (juxt + - * / mod max min))

(do-math 6 3)

=> [9 3 18 2 0 6 3]
```

## Example 2

More practically, juxt can be used to extract certain values from a collection of hash-maps, in this case the id and the name.

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

The previous example can easily be expanded into a value lookup

```clojure
(def persons
  (->> [{:id 1 :name "John"  :age 5}
        {:id 2 :name "Gale"  :age 5}
        {:id 3 :name "Zoe"   :age 7}
        {:id 4 :name "Diana" :age 7}
        {:id 5 :name "Aden"  :age 5}
        {:id 6 :name "Alex"  :age 7}]
       (map (juxt :id :name))
       (into {})))

(get persons 1)

=> "John"

(def persons
  (->> [{:id 1 :name "John"  :age 5}
        {:id 2 :name "Gale"  :age 5}
        {:id 3 :name "Zoe"   :age 7}
        {:id 4 :name "Diana" :age 7}
        {:id 5 :name "Aden"  :age 5}
        {:id 6 :name "Alex"  :age 7}]
       (map (juxt :id identity))
       (into {})))

(get persons 3)

=> {:id 3 :name "Zoe" :age 7}
```

### Example 4

It is also handy if you want to sort by more than one property, in this case by age first and then by name

```clojure
(def persons
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
