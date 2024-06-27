---
title: '구름/합 계산기'
date: 2024-06-03
tags: [Programming, Python, 구름]
categories:
  - Programming
  - Python
---

https://level.goorm.io/exam/195685/%ED%95%A9-%EA%B3%84%EC%82%B0%EA%B8%B0/quiz/1

![](https://i.imgur.com/BBynTlQ.png)

## 해결 - python

```python
n = int(input())
s = 0
for _ in range(n):
	a, op, b = input().split()
	a = int(a)
	b = int(b)
	if (op == "+"):
		r = a + b
	elif (op == '-'):
		r = a - b
	elif (op == "*"):
		r = a * b
	elif (op == "/"):
		r = int(a / b)
	s += r
print(s)
```

## 해결 - clojure

주어진 수 만큼 반복해서
계산하고 더하고 출력한다.

```clojure
(defn calc [line]
  (let [[a op b] (clojure.string/split line #"\s+")]
    (int ((resolve (symbol op))
          (Integer/parseInt a)
          (Integer/parseInt b)))))

(->> (repeatedly (Integer/parseInt (read-line))
                 #(calc (read-line)))
     (apply +)
     (print))

```
