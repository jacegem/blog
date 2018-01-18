---
title: it21-machine-learning
date: 2017-02-12
tags: [machine, learning]
categories:
- Conference
- ETC
---

# it21-machine-learning

Hwanjo Yu / POSTECH
http://hwanjoyu.org (웹페이지 안열림)

feature, generaliztion
80분/ 슬라이드 교체함
딥러닝, 컴퓨터 비전, 사물 인식,
조명, 각도가 바뀌면 못알아본다. → 문제점
사람이 인식하는 하이레벨 피처를 알아본다. 인간처럼
이미지 인식은 인간보다 더 잘한다.
what current systems recoginze in below images

자연어 처리/ Sentiment Analysis
문맥을 본다.
Machine Translations

1997: Deep Blue (chess)
2011: IBM Watson (Jeopardy!)
2016: AlphaGo
머신러닝 모델링의 싸움

본능.
사람은 비합리적인 판단을 한다. 본능이 있기 때문에

2가지 공통점
인공지능 과제

AlphaGo
NVIDIA Auto Driving
Google Translation

Big Data + Hardware + Machine Learning Algorithm

real-world task  → `modeling` → formal task → `algoritms` → programs

`가중치`를 구해주는것이 머신러닝
가중치를 자동으로 학습.

모델링/
피처를 잘 고르고, 잘 연결
틀을 만든다.
weight parameter 는 머신러닝을 통해 얻는다.

what do we need to learn?
- Type of models...
- Art of Modeling...
- Developing Algorithms...

어떤 모델들이 있는가

가장 단순한 모델
Reflex
Sentiment Analysis
`cliches`
단어가 들어 있는지 확인 하여 분류. 단어 하나로 결정
단어 여러개를 본다. 부정적이면 -10, 긍정적이면 +5, 스코어를 결정한다.

가장 기본이 되는 단위, `linear classifier`
learning parameter, weight

Training examples → `Learning algorithm` → Simple Program with

`Generalization`

Reflex 모델
State-based model

`state` :
디자인 후, 옵티멀한 경로를 찾아가는 것

응용되는 곳
Search Problem

스스로 학습

`Variable-based Models`
CSP Constrain
Event Scheduling
Topic Modeling

확률그래프 모델링, 그래픽컬 모델링, 베이지안 네트워크

`Logic`
High-level Intelligence

모두 Optimization Problems 으로 바뀐다.

Optimization
- Models are optimization problems
- Discrete optimization (dynamic programming)
- Continuous optimization (gradient descent)

## Roadmap
- loss minimization
- Features & Neural Network
- Generalization

`GAN`
데이터를 genrate
https://tensorflow.blog/2016/11/24/gan-pixelcnn/

피처가 왜 중요하나
Linear Prediction

redidual 을 최소화 한다.
Feature extraction

피처를 어떻게 정의해야 하는가
데이터를 넣어서 얼마나 건강한가 판단

제곱을 디자인할 수도 있다.
새로운 피처 스페이스에서는 넌리니어가 된다.

`joint learning`

Neural Network 는 피처를 learning 하는 것이다.
구조화 되지 않은 데이터에 머신러닝이 좋다.

데이터가 많이 필요한 것이 머신러닝의 단점이다.

### Generalization
어떻게 Generalization을 할 수 있는가
오버피팅을 피하려면
데이터를 많이 모은다.

`Autoencoder`

Loss minimization
- Design lossw ell to reflect what you really want

Features & neural Network
- Design features well to reduce data..
- Desing NN well to learn high-level features..

Generalization
- Regularize well to generalize
- Think about generalization when you do all above
