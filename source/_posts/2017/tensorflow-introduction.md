---
title: '텐서플로 입문 (예제로 배우는 텐서플로)'
date: 2017-04-01 19:13:45
tags: [python, tensorflow, book]
categories:
- Programming
- Python
---

# 책.텐서플로 입문 (예제로 배우는 텐서플로)

머신러닝을 공부하고자 하는 학생은 유투브, 코세라, 에드엑스 등 동영상 강의를 통해 해외 유명 대학의 수업을 쉽게 접할 수 있다.

## 들어가며

텐서플로(Tensorflow)는 머신 러닝과 딥 러닝을 위해 만들어진 오픈소스 소프트웨어 라이브러리이다.

머신러닝 알고리즘
- 지도 학습 : 프로그래머가 컴퓨터에게 해야 할 행위를 가려쳐 줌
- 비지도 학습 : 모든 과정을 컴퓨터가 스스로 하도록 함

텐서플로의 프로그래밍적인 기능을 따루기 위해서는 먼저 파이썬 프로그램이 언어에 대해 알아둬야 한다.

## 이 책의 구성

- 1장, 텐서플로 기초 : 최적화와 디버깅에서 사용하는 텐서보드(`TensorBoard`)
- 2장, 텐서플로 기초 연산 : 자료 구조인 텐서(`Tensor`)
- 3장, 머신 러닝 시작 : 분류(`Classification`), 군집화(`Clustering`)
- 4장, 인공 신경망 소개 : 신경망(Neural Newtork), 단일 계층 퍼셉트론, 다중 계층 퍼셉트론
- 5장, 딥 러닝 : CNN(Convolutional Neural Network), RNN(Recurrent Neural Network)
- 6장, GPU 프로그래밍과 텐서플로 서빙 : GPU 연산 능력 이용

## 다운로드

- 예제 코드 : https://github.com/PacktPublishing/Getting-Started-with-TensorFlow
- 컬러 이미지 : http://www.acornpub.co.kr/book/tensorflow
- 텐서플로 : https://www.tensorflow.org

## 텐서플로 개요

주요 기능
- 다차원 배열(텐서)의 정의, 최적화, 효율적 산술 연산
- 딥 뉴럴 네트워크와 머신 러닝 프로그래밍 지원
- 메모리와 데이터가 자동으로 관리되는 쉬운 GPU 가속 기능 제공
- 별도의 코드를 작성하지 않아도 텐서플로가 자동으로 CPU와 GPU에 자원을 할당해 연산
- 빅데이터 처리를 위한 대규모 병렬 컴퓨팅 지원

## 파이썬 기초

- 강력한 형식 Strong Type
- 동적 언어 Dynamic Language
- 대소문자를 구별
- 객체지향 패러다임으로 작성된 언어

### 데이터 형식

- 리스트 list
- 튜플 tuple
- 딕셔너리 dict

### 클래스

- 파이썬은 다중 상속 클래스를 지원한다.
- 변수와 비공개`private` 메소드의 이름은 일반적으로 `__`(두 개의 언더스코어)를 접투어 및 접미어로 사용한다.

### 텐서플로 동작 확인

```sh
pip install tensorflow
```

```python
>>> import tensorflow as tf
>>> hello = tf.constant('hello Tensorflow!')
>>> sess = tf.Session()
>>> print(sess.run(hello))
b'hello Tensorflow!'
```

```python
>>> x = tf.constant(1, name='x')
>>> y = tf.Variable(x+9, name='y')
>>> print(y)
<tf.Variable 'y:0' shape=() dtype=int32_ref>
>>> model = tf.global_variables_initializer()
>>> with tf.Session() as session:
...     session.run(model)
...     print(session.run(y))
...
10
```

## 데이터 플로우 그래프

대부분의 머신 러닝 알고리즘은 복잡한 산술식의 반복 연산으로 구성돼 있다.
텐서플로의 연산은 데이터 플로우 그래프로 구성되며, 데이터 플로우 그래프의 노드`node`는 산술 연산자(덧셈, 뺄셈, 곱셈, 나눗셈)이고, 에지`edge`는 텐서`tensor`라고 명명된 다중 다차원 데이터 집합을 의미하며 피연산자가 된다.

## jupyter 서버 설치

http://goodtogreate.tistory.com/entry/IPython-Notebook-설치방법

## 변경 소스

- tf.initialize_all_variables() →  `tf.global_variables_initializer()`
- tf.summary.merge_all → `tf.summary.merge_all`
- tf.train.SummaryWriter → `tf.summary.FileWriter`
- tf.complex_abs → tf.abs

# 텐서플로 기초 연산

텐서는 텐서플로의 기본 자료 구조다.
텐서는 데이터 플로우 그래프에서 에지를 연결한다.

텐서는 rank, shape, type, 세 가지의 매개변수로 구분된다.
- rank : 텐서의 차원은 rank로 나타낸다.
- shape : 텐서의 행과 열이 몇 개인지를 나타낸다.
- type : 텐서의 데이터가 어떤 형식인지를 나타낸다.


### 텐서플로 산술연자

- tf.add
- tf.sub
- tf.mul
- tf.div
- tf.mod
- tf.abs
- tf.neg
- tf.sign
- tf.inv
- tf.square
- tf.round
- tf.sqrt
- tf.pow
- tf.exp
- tf.log
- tf.maximum
- tf.minimum
- tf.cos
- tf.sin


# 3장 머신 러닝 시작

## 경사 하강법

경사 하강법(Gradient Descent)을 이용해 cost_function 값을 최소화할 수 있다. 하지만 수학적인 특성으로 인해 경사 하강법은 대부분의 경우 전역 최솟값이 아닌 지역 최솟값에 머무르게 된다. 경사 하강법을 통해 비용 함수를 최소화하는 방법을 알아보자.

- `평가` : 함수의 정의역 내에서 임의의 값을 추출해 입력 값으로 사용한 후 결과 값에 대해 경사 하강법을 적용한다. 경사 하강법은 비용 함수가 최소화될 수 있는 경사로의 방향을 가리킨다.
- `선택` : 어느 방향의 경사를 따라갈 것인지를 선택해야 한다.

---

- 비용 함수 cost function
- 경사 하강 알고리즘 Gradient Descent Algorithm

## MNIST

데이터 다운로드와 스크립트
- https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/mnist/input_data.py

```python
from tensorflow.contrib.learn.python.learn.datasets.mnist import read_data_sets
```

## 분류기

# 5장 딥러닝
