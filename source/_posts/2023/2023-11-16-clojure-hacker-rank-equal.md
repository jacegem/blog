---
title: "[Clojure] HackerRank - equal"
date: 2023-11-16
tags: [clojure, algorithm, hackerrank, equal]
categories:
  - Clojure
  - Algorithm 
---


## equal 

- https://www.hackerrank.com/challenges/equal/problem

> 크리스티는 해커랭크에서 인턴으로 일하고 있습니다. 
> 어느 날 그녀는 동료들에게 초콜릿을 나눠줘야 합니다. 
> 그녀는 친구들에게 편향되어 있어 다른 친구들보다 더 많이 줄 계획입니다. 
> 프로그램 관리자 중 한 명이 이 사실을 듣고 모든 사람이 같은 숫자를 받도록 하라고 말합니다.
>
> 일을 어렵게 만들기 위해 그녀는 일련의 연산에서 초콜릿의 수를 동일하게 만들어야 합니다. 
> 각 작업마다 그녀는 한 명의 동료를 제외한 모든 동료에게 1, 2, 또는 5개의 초콜릿 조각을 줄 수 있습니다. 
> 한 라운드에서 한 조각을 받은 모든 사람은 같은 수의 조각을 받습니다.
>
> 시작 분포가 주어졌을 때, 모든 동료가 같은 수의 조각을 갖기 위해 필요한 최소한의 연산 횟수를 계산합니다.

## 예시

[2 2 3 7] 입력이 주어지면, 결과값은 `2` 이다. 

[2 2 3 7] 로 시작하고
세번째를 제외하고 1을 더한다 ➡️ [3 3 3 8]
네번째를 제외하고 5를 더한다 ➡️ [8 8 8 8]

---

[10 7 12] 입력이 주어지면, 결과값은 `3` 이다. 

[10 7 12] 로 시작하고

세번째를 제외하고 5를 더한다 ➡️ [15 12 12]
첫번째를 제외하고 2를 더한다 ➡️ [15 14 14]
첫번째를 제외하고 1을 더한다 ➡️ [15 15 15]

## 해결

한명을 제외한 동료들의 조각 수를 더하는 것을, 반대로 생각해서 한명의 동료의 조각 수를 빼는 것으로 한다. 
그러면 각각의 동료들에게세 조각 수를 빼는 횟수를 더하면 된다. 

[2 2 3 7] 에서 가장 작은 수를 뺀 차이값으로 변경한다. 
가장작은 수 ➡️ 2
차이값 ➡️ [0 0 1 5]
1, 2, or 5 개를 더할 수 있으므로, 각각의 횟수는 ➡️ [0 0 1 1] 
따라서 각 횟수를 모두 더하면 `2`회가 된다.

---

[10 7 12] 에서 가장 작은 수를 뺀 차이값으로 변경한다. 
가장 작은 수 ➡️ 7
차이값 ➡️ [3 0 5]
1, 2, or 5 개를 더할 수 있으므로, 각각의 횟수는 ➡️ [2 0 1] 
따라서 각 횟수를 모두 더하면 `3`회가 된다.

## Solution 

- https://github.com/jacegem/coding-challenge-clj/blob/main/src/hacker_rank/equal.clj

```clojure
(def count-map {0 0
                1 1
                2 1
                3 2
                4 2
                5 1})

(defn op-count [n]
  (loop [cnt n
         acc 0]
    (if (<= cnt 5)
      (+ acc (count-map cnt))
      (recur (- cnt 5) (inc acc)))))

(defn equal [arr]
  (->> (map #(- % (apply min arr)) arr)
       (map op-count)
       (reduce +)))
```


## 실행

`(equal [2 2 3 7])` 를 실행한 화면

![](https://i.imgur.com/fa8Fhiw.png)
![](https://i.imgur.com/j71RKGq.png)
![](https://i.imgur.com/wsNZuNC.png)

## link

- https://www.hackerrank.com/challenges/equal/problem
- https://github.com/jacegem/coding-challenge-clj/blob/main/src/hacker_rank/equal.clj
