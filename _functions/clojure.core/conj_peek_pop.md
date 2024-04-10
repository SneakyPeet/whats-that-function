---
title: clojure.core list operations - conj peek pop
description: TODO
layout: fn
fn: conj peek pop
lib: clojure.core
programming_language: clojure
backgroundcolor: ""
headingcolor: ""
fncolor: "TODO"
youtube: TODO
published: false
date: 2023-04-15 00:00:00

---

conj, peek and pop are core functions that operate on various collection types.

Given a collection and zero or more items, conj will return a new collection of the same type with the new items added.

Given a collection, pop will return a new collection of the same type, with one of the items removed.

Finally peek will return the item that will be removed when pop is called.

The resulting behaviour of these functions change based on the type of collection.

## Example 1

Given a vector, conj will add the new items to the end of the vector.

Peek will return the item at the end of the vector. This behaves the same as the last function, but is much more efficient.

Pop then removes the last item.

```clojure
(def a [1 2 3])

(conj a 4) ;=> [1 2 3 4]

(def b (conj a 4))

(peek b) ;=> 4

(last b) ;=> 4

(pop b) ;=> [1 2 3]
```

## Example 2

Given a list, conj will add the new items to the start of the list.

Peek will return the item at the start of the list, similar to the first function.

Pop will remove the first item.

```clojure
(def a '(1 2 3))

(conj a 4) ;=> '(4 1 2 3)

(def b (conj a 4))

(peek b) ;=> 4

(first b) ;=> 4

(pop b) ;=> '(1 2 3)
```

## Example 3

Given a queue, conj will add the new items to the end of the queue.

Peek will return the item a the start of the queue, similar to the first function.

Pop will remove the first item

```clojure
(def a #queue [1 2 3])

(conj a 4) ;=> #queue [1 2 3 4]

(def b (conj a 4))

(peek b) ;=> 1

(first b) ;=> 1

(pop b) ;=> #queue [2 3 4]
```

Note that the #queue [] tagged literal is only available in Clojurescript. A queue can be constructed in Clojure as follows:

```clojure
(def a (conj clojure.lang.PersistentQueue/EMPTY 1 2 3))
```

## Example 4

Here is a side by side comparison of the 3 list types. Queues are first in first out (fifo). Vectors and lists are first in last out (filo) and they can be used to represent a stack. Lists are more performant than vectors in this situation.

|        | Vectors   | Lists      | Queues           |
|:------:|:---------:|:----------:|:----------------:|
|        | [1 2 3]   | '(1 2 3)   | #queue [1 2 3]   |
| conj 4 | [1 2 3 4] | '(4 1 2 3) | #queue [1 2 3 4] |
| peek   | 4         | 4          | 1                |
| pop    | [1 2 3]   | '(1 2 3)   | #queue [2 3 4]   |
|        | FILO      | FILO       | FIFO             |
