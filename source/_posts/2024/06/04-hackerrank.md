---
title: 'HackerRank Warmup'
date: 2024-06-03
tags: [Programming, Clojure, HackerRank]
categories:
  - Programming
  - Clojure
---

![](https://i.imgur.com/AOqggTJ.png)

## Solve Me First

https://www.hackerrank.com/challenges/solve-me-first/problem

```clojure
(defn solveMeFirst [x y]
    (+ x y)
)
```

## Simple Array Sum

https://www.hackerrank.com/challenges/simple-array-sum/problem

```clojure
(defn simpleArraySum [ar]
    (apply + ar)
)
```

## Compare the Triplets

https://www.hackerrank.com/challenges/compare-the-triplets/problem

```clojure
(defn compare [ax bx]
  (map (fn [a b]
         {:alice (if (> a b) 1 0)
          :bob   (if (< a b) 1 0)})
       ax bx))

(defn calc-points [res]
  (reduce (fn [acc v]
            (let [a (:alice v)
                  b (:bob v)]
              (-> acc
                  (update :alice #(+ % a))
                  (update :bob #(+ % b)))))
          {:alice 0
           :bob   0}
          res))

(defn print-points [m]
  [(:alice m) (:bob m)])

(defn compareTriplets [ax bx]
   (-> (compare ax bx)
    (calc-points)
    (print-points)))
```

## A Very Big Sum

https://www.hackerrank.com/challenges/a-very-big-sum/problem

```clojure
(defn aVeryBigSum [ar]
 (apply + ar)
)
```

## Birthday Cake Candles

https://www.hackerrank.com/challenges/birthday-cake-candles/problem

```clojure
(defn birthdayCakeCandles [candles]
	(->> (frequencies candles)
			(sort-by first)
			(reverse)
			first
			second)
	)
```
