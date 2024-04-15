---
title: clojure.core/into
description: into is a clojure.core function that takes 2 collections, and conjoins the items from the second collection into the first collection, returning a collection of the same type as the first collection.
layout: fn
fn: into
lib: clojure.core
programming_language: clojure
backgroundcolor: "#f5ff74"
headingcolor: "#7de6a1"
fncolor: "#c78bf9"
youtube: TODO
published: false
frontpage: false
date: 2024-04-19 00:00:00

---

into is a clojure.core function that takes 2 collections, and conjoins the items from the second collection into the first collection, returning a collection of the same type as the first collection.

Just like with conj, into's behaviour changes based on the collection types.

## Example 1

A vector as the first collection will add the second collection items to it's end.

```clojure
(into [1 2 3] [4 5 6])
(into [1 2 3] '(4 5 6))
(into [1 2 3] (sorted-set 4 5 6))
(into [1 2 3] (java.util.List/of 4 5 6))
(into [1 2 3] (conj clojure.lang.PersistentQueue/EMPTY 4 5 6))

;=> [1 2 3 4 5 6]
```

The order of added items cannot be guaranteed if the second collection is not a sorted type.

```clojure
(into [1 2 3] #{4 5 6}) ;=> [1 2 3 4 6 5]
```

Conjoining into a vector is especially useful when building up html structures using hiccup syntax

```clojure
(->> ["Home" "Blog" "About"]
     (map (fn [page]
            [:li page]))
     (into [:ul]))

;;=> [:ul [:li "Home"] [:li "Blog"] [:li "About"]]
```

## Example 2

A list as the first collection, will add sorted items in reverse order to the front of the list. That is because a lists always conjoins at the front.

```clojure
(into '(1 2 3) [4 5 6])
(into '(1 2 3) '(4 5 6))

=> '(6 5 4 1 2 3)

```

## Example 3

Having a hash-map as the second collection, will split the hash-map into key value pairs

```clojure
(into [] {:a 1 :b 2}) ;=> [[:a 1] [:b 2]]
(into '() {:a 1 :b 2}) ;=> '([:b 2] [:a 1])
(into #{} {:a 1 :b 2}) ;=> #{[:b 2] [:a 1]}

```

## Example 4

You can use into to conjoin key value pairs into a hash-map

```clojure
(into {} [[:a 1] [:b 2] [:c 3]])

=> {:a 1 :b 2 :c 3}
```

As seen in the [juxt](/clojure.core/juxt) video this can be useful to build up lookup tables.

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

```

# Example 5

If the second collection is a hash-map or collection of hash-maps, the key values are merged with the first hash-map.

```clojure
(into {:a 1 :b 1} {:b 2 :c 3}) ;=> {:a 1 :b 2 :c 3}
(into {:a 1 :b 1} [{:b 2 :c 2} {:c 3}]) ;=> {:a 1 :b 2 :c 3}
```

# Example 6

into can optionally take a transducer as the second argument. The second collection will be passed to the transducer before conjoining with the first collection

```clojure
(into [1 2 3 4] (filter odd?) [5 6 7 8 9 10])

=> [1 2 3 4 5 7 9]
```

# Example 7

Finally given no arguments, into returns an empty vector and given 1 argument, into returns that argument

```clojure
(into) ;=> []
(into :anything) ;=> :anything
```
