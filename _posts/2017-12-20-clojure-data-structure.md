---
layout: post
title:  "Clojure常用的function用法"
date:   2017-12-20 20:36:39 +0800
categories: clojure datastructure
math: true
typora-copy-images-to: ../assets/images/2017-12
typora-root-url: ../../alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}
[reference from cheatsheet](https://clojure.org/api/cheatsheet)



## -> 与->>

```clojure
(defmacro ->
  "Threads the expr through the forms. Inserts x as the
  second item in the first form, making a list of it if it is not a
  list already. If there are more forms, inserts the first form as the
  second item in second form, etc."
  {:added "1.0"}
  [x & forms]
  (loop [x x, forms forms]
    (if forms
      (let [form (first forms)
            threaded (if (seq? form)
                       (with-meta `(~(first form) ~x ~@(next form)) (meta form))
                       (list form x))]
        (recur threaded (next forms)))
      x)))
```



```clojure
(defmacro ->>
  "Threads the expr through the forms. Inserts x as the
  last item in the first form, making a list of it if it is not a
  list already. If there are more forms, inserts the first form as the
  last item in second form, etc."
  {:added "1.1"}
  [x & forms]
  (loop [x x, forms forms]
    (if forms
      (let [form (first forms)
            threaded (if (seq? form)
              (with-meta `(~(first form) ~@(next form)  ~x) (meta form))
              (list form x))]
        (recur threaded (next forms)))
      x)))
```





## iterate

> ```
> Returns a lazy sequence of x, (f x), (f (f x)) etc. f must be free of side-effects
> ```



```
user=> (iterate inc 5)
(5 6 7 8 9 10 11 12 13 14 15 ... n

;; limit results
user=> (take 5 (iterate inc 5))
(5 6 7 8 9)

user=> (take 10 (iterate (partial + 2) 0))
(0 2 4 6 8 10 12 14 16 18)

user=> (take 20 (iterate (partial + 2) 0))
(0 2 4 6 8 10 12 14 16 18 20 22 24 26 28 30 32 34 36 38)
```



## seq与sequence

seq:

```
Returns a seq on the collection. If the collection is
  empty, returns nil
```



常用来判断collection是否为空

```clojure
(seq '(1))  ;;=> (1)
(seq [1 2]) ;;=> (1 2)
(seq "abc") ;;=> (\a \b \c)

;; Corner cases
(seq nil)   ;;=> nil
(seq '())   ;;=> nil
(seq "")    ;;=> nil

;; (seq x) is the recommended idiom for testing if a collection is not empty
(every? seq ["1" [1] '(1) {:1 1} #{1}])
;;=> true



```



```clojure
;; Here is the difference between seq and sequence

user> (seq nil)
;;=> nil

user> (seq ())
;;=> nil

user> (sequence ())
;;=> ()

user> (sequence nil)
;;=> ()
```





## for/cycle

for: 

```clojure
;; prepare a seq of the even values 
;; from the first six multiples of three
(for [x [0 1 2 3 4 5]
      :let [y (* x 3)]
      :when (even? y)]
  y)
;;=> (0 6 12)

```



```clojure
(def digits (seq [1 2 3]))
(for [x1 digits x2 digits] (* x1 x2))
;;=> (1 2 3 2 4 6 3 6 9)
```



```clojure
;; produce a seq of all pairs drawn from two vectors
(for [x ['a 'b 'c] 
      y [1 2 3]]
  [x y])
;;=> ([a 1] [a 2] [a 3] [b 1] [b 2] [b 3] [c 1] [c 2] [c 3])

```



cycle:

```clojure
user=> (take 5 (cycle ["a" "b"]))
("a" "b" "a" "b" "a")

user=> (take 10 (cycle (range 0 3)))
(0 1 2 0 1 2 0 1 2 0)
```



## cons与conj

cons:

```
Returns a new seq where x is the first element and seq is
  the rest.
```



```clojure
;; prepend 1 to a list
(cons 1 '(2 3 4 5 6))
;;=> (1 2 3 4 5 6)

;; notice that the first item is not expanded
(cons [1 2] [4 5 6])
;;=> ([1 2] 4 5 6)
```



conj:

```
 Returns a new collection with the xs
  'added'.
  
```

conj: 保持原来的数据结构， list加到头部， vector增加到尾部

```clojure
;; notice that conjoining to a vector is done at the end
(conj [1 2 3] 4)
;;=> [1 2 3 4]

;; notice conjoining to a list is done at the beginning
(conj '(1 2 3) 4)
;;=> (4 1 2 3)

(conj ["a" "b" "c"] "d")
;;=> ["a" "b" "c" "d"]
```



## partition

```clojure
;; partition a list of 20 items into 5 (20/4) lists of 4 items
(partition 4 (range 20))
;;=> ((0 1 2 3) (4 5 6 7) (8 9 10 11) (12 13 14 15) (16 17 18 19))

;; partition a list of 22 items into 5 (20/4) lists of 4 items 
;; the last two items do not make a complete partition and are dropped.
(partition 4 (range 22))
;;=> ((0 1 2 3) (4 5 6 7) (8 9 10 11) (12 13 14 15) (16 17 18 19))

;; uses the step to select the starting point for each partition
(partition 4 6 (range 20))
;;=> ((0 1 2 3) (6 7 8 9) (12 13 14 15))

;; if the step is smaller than the partition size, items will be reused
(partition 4 3 (range 20))
;;=> ((0 1 2 3) (3 4 5 6) (6 7 8 9) (9 10 11 12) (12 13 14 15) (15 16 17 18))

```



## shuffle

```clojure
;; Make five permutations of the [1 2 3] vector.
;; Notice that the type of the shuffled collection is retained.
(repeatedly 5 (partial shuffle [1 2 3]))
;;=> ([2 3 1] [2 1 3] [2 3 1] [3 2 1] [3 1 2])
```



## sort

```clojure
user=> (sort [3 1 2 4])
(1 2 3 4)

user=> (sort > (vals {:foo 5, :bar 2, :baz 10}))
(10 5 2)

;; do not do this, use sort-by instead
user=> (sort #(compare (last %1) (last %2)) {:b 1 :c 3 :a  2})
([:b 1] [:a 2] [:c 3])

;; like this:
user=> (sort-by last {:b 1 :c 3 :a 2})
([:b 1] [:a 2] [:c 3])
```



## map

```clojure
(map inc [1 2 3 4 5])
;;=> (2 3 4 5 6)


;; map can be used with multiple collections. Collections will be consumed
;; and passed to the mapping function in parallel:
(map + [1 2 3] [4 5 6])
;;=> (5 7 9)


;; When map is passed more than one collection, the mapping function will
;; be applied until one of the collections runs out:
(map + [1 2 3] (iterate inc 1))
;;=> (2 4 6)



;; map is often used in conjunction with the # reader macro:
(map #(str "Hello " % "!" ) ["Ford" "Arthur" "Tricia"])
;;=> ("Hello Ford!" "Hello Arthur!" "Hello Tricia!")
```



## zipmap

```clojure
;; initialize with 0 for all values
user=> (zipmap [:a :b :c] (repeat 0))
{:a 0, :b 0, :c 0}


user=> (zipmap [:a :b :c :d :e] [1 2 3 4 5])
{:a 1, :b 2, :c 3, :d 4, :e 5}

;; 4 is not included in the result
user=> (zipmap [:a :b :c] [1 2 3 4])
{:a 1, :b 2, :c 3}

;; :c is not included in the result
user=> (zipmap [:a :b :c] [1 2])
{:a 1, :b 2}
```



## list*

```clojure
(defn list*
  "Creates a new seq containing the items prepended to the rest, the
  last of which will be treated as a sequence."
  {:added "1.0"
   :static true}
  ([args] (seq args))
  ([a args] (cons a args))
  ([a b args] (cons a (cons b args)))
  ([a b c args] (cons a (cons b (cons c args))))
  ([a b c d & more]
     (cons a (cons b (cons c (cons d (spread more)))))))

```



## apply

往往后面是collection

```clojure
;; If you were to try
(max [1 2 3])
;;=> [1 2 3]

(apply max [1 2 3])
;;=> 3

;; which is the same as 
(max 1 2 3)
;;=> 3


(map #(apply max %) [[1 2 3][4 5 6][7 8 9]])
;;=> (3 6 9)
```

