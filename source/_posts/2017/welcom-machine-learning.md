---
title: '책. 어서와~ 머신러닝은 처음이지'
date: 2017-05-01 19:13:45
tags: [R, machine, learning]
categories:
- Programming
- R
---

# 책. 어서와~ 머신러닝은 처음이지

- 데이터 마이닝, 기계학습 → 데이터사이언스
- 유사한 분류를 찾는다.
- 유사성이란 무엇일까.

$$
distance = \sqrt{(x_1-x_2)^2 + (y_1-y_2)^2}
$$

- 다차원 척도법(Multi-Dimensinal Scaling, MDS)
- 민코우스키(Minkowski) 거리 (r=1이 되면 맨타한 거리, r=2이 되면 유클리드 거리)
$$
d(x,y) = (\sum^{m}_{n=1}|x_n-y_n|^r)^\frac{1}{r}
$$

$$
d(x,y) = \cos(x,y) = \frac{x\cdot y}{|x||y|}
$$

## 변화의 시작

```r
academy <- read.csv('academy.csv', stringsAsFactors=F, header=T)
academy <- academy[-1]
head(academy)

tow_coord <- cmdscale(dist_academy)
plot(two_coord, type='n')
text(two_coord, as.caracter(1:52))
```

> Entropy(x)는 x에 대한 정보를 나타내기 위하여 평균적으로 알아야할 bit의 개수이다. 이것은 정보의 기대값이다.

> Pruning(가지치기)은 트리를 생성한 후에 overfitting을 방지하기 위해 필요없는 node를 줄이는 것을 의미한다.

### Clustering Analysis(군집분석)

최단연결법(Single Linkage) : 데이터 A와 군집 C와의 거리, 데이터 B와 군지 C와의 거리 중 최소값을 선택한다.
$$
d_{(A B)C}=min(d_{A C}, d_{B C})
$$

### K-means(K-평균군집)

1. 임의의 K개의 학습데이터를 평균벡터로 설정한다. (군집의 중심 초기설정)
2. 데이터들을 K개의 평균벡터에 대하여 분류한다. (가장 가까운 점으로)
3. 분류된 데이터들에 대하여 평균벡터(군집 중심)를 구한다. 그래서 새로운 K개의 평균벡터를 구한다.
4. 2번을 반복한다.
5. 3번을 반복한다.
6. K평균벡터가 거의 변동이 없을 때까지 2번, 3번을 반복한다.

> 각각의 K개의 평균 vector로부터 해당군집의 분산이 최소가 되는 군집을 찾는 기법
