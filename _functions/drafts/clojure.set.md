---

---

## union

```clojure
(clojure.set/union #{1 2 3} #{4 5 6})
=> #{1 4 6 3 2 5}

(clojure.set/union #{1 2 3} #{4 5 6} #{3 4 7 8})
=> #{7 1 4 6 3 2 5 8}
```

## difference

```clojure
(clojure.set/difference #{1 2 3} #{4 5 6})
=> #{1 2 3}

(clojure.set/difference #{1 2 3 4} #{3 4 5 6})
=> #{1 2}
```

## intersection

## subset?

## superset?

## select

## map-invert

## rename-keys

## rename

## project

## index

## join
