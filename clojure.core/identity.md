---
layout: page
fn: identity
lib: clojure.core
backgroundcolor: "#ff7952"
headingcolor: "#ff7952"
fncolor: "#f3ff55"

---

## Overview

**Identity!** A function that takes what you give it, and just gives it back.

* Give it a string, get the same string
* Give it a hash-map, get the same hash-map
* Give it a function, get the same function

How can this be useful? Identity is best used combined with other higher order functions.

## Example 1

Combined with filter, it can be used to filter out nil.

```clojure
(->> [1 2 nil 3 nil 4 5]
     (filter identity))

=> (1 2 3 4 5)
```

## Example 2

Combined with the juxt function, it can be used to index data. This works because identity returns the hash-map as is.

```clojure
(def lookup
  (->>
    [{:id 1 :name "John"  :age 5}
     {:id 2 :name "Gale"  :age 5}
     {:id 3 :name "Zoe"   :age 7}
     {:id 4 :name "Diana" :age 7}
     {:id 5 :name "Aden"  :age 5}
     {:id 6 :name "Alex"  :age 7}]
    (map (juxt :id identity))
    (into {})))

(get lookup 5)

=> {:id 5 :name "Aden" :age 5}
```


## Example 3

It can also act as a no-op. In this case the transform function will always round a double, but not do anything to other value types.

```clojure
(defn transform [value]
  (let [f (if (= java.lang.Double (type value))
            #(Math/round %)
            identity)]
    (f value)))


(transform 5) => 5

(transform 1/4) => 1/4

(transform 1.345345) => 1
```

So even though it seems strange at first glance, the identity function have lots of application in the functional world.
