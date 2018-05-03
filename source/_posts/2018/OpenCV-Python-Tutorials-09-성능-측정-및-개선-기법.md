---
title: '[OpenCV-Python Tutorials] 09 성능 측정 및 개선 기법'
date: 2018-01-09 19:14:25
tags: [opencv, python, tutorial]
category:
- Programming
- Python
---

# [OpenCV-Python Tutorials] 09 성능 측정 및 개선 기법

## 목표

이미지 처리에서는 초당 많은 작업을 처리하기 때문에 코드가 올바른 솔루션을 제공 할 뿐만 아니라 `가장 빠른 방식`으로 제공되어야 합니다. 따라서 이 장에서는 다음과 같은 내용을 배울 수 있습니다. 

- 코드의 성능을 측정합니다.
- 코드의 성능을 향상시키기 위한 몇 가지 팁.
- `cv2.getTickCount`, `cv2.getTickFrequency` 등의 함수를 확인합니다.

OpenCV 외에도 Python은 실행 시간을 측정하는 데 유용한 모듈 시간을 제공합니다. 다른 모듈 프로파일은 코드의 각 함수가 얼마나 많은 시간을 사용하는지, 얼마나 많은 시간이 함수가 호출되었는지 등과 같은 코드에 대한 자세한 보고서를 얻는 데 도움이 됩니다. 

IPython을 사용하는 경우 이러한 모든 기능이 사용자 친화적 방법으로 포함되어 있습니다. 우리는 몇 가지 중요한 것들을 보게 될 것이며, 자세한 내용은 Additional Resouces 섹션의 링크를 확인하십시오.

## OpenCV로 성능 측정

`cv2.getTickCount` 함수는 이 함수가 호출되는 순간까지 참조 이벤트 (순간 기계가 켜졌을 때처럼) 이후의 클럭 사이클 수를 반환합니다. 따라서 함수를 실행하기 전후에 함수를 호출하면 함수를 실행하는데 사용되는 클럭 사이클 수를 얻게 됩니다.

`cv2.getTickFrequency` 함수는 클럭 사이클의 빈도 또는 초당 클럭 사이클 수를 반환합니다. 따라서 실행 시간을 초 단위로 확인하려면 다음을 수행 할 수 있습니다.

```python
e1 = cv2.getTickCount()
# your code execution
e2 = cv2.getTickCount()
time = (e2 - e1)/ cv2.getTickFrequency()
```

우리는 다음 예제를 통해 설명 할 것입니다. 다음 예제는 5부터 49까지의 홀수 크기의 커널을 사용하여 중간 필터링을 적용합니다. (결과가 어떻게 보이는지 걱정하지 마십시오. 이것은 우리의 목표가 아닙니다.)

```python
import cv2

img1 = cv2.imread('model.jpg')

e1 = cv2.getTickCount()
for i in range(5, 49, 2):
    img2 = cv2.medianBlur(img1, i)
e2 = cv2.getTickCount()
t = (e2 - e1) / cv2.getTickFrequency()

cv2.imshow('Original', img1)
cv2.imshow('Blur', img2)
cv2.waitKey(0)
cv2.destroyAllWindows()

print(t)


# 결과: 2.5564403101561437
```


![원본](https://goo.gl/HiuJ4e)

![medianBlur](https://goo.gl/D84KTf)



> 당신은 `time` 모듈을 사용할 수도 있습니다. `cv2.getTickCount` 대신 `time.time()` 함수를 사용하십시오. 그런 다음 두 시각의 차이를 사용하면 됩니다.


## OpenCV의 기본 최적화

많은 OpenCV 기능은 SSE2, AVX 등을 사용하여 최적화됩니다. 최적화되지 않은 코드도 포함되어 있습니다. 따라서 우리 시스템이 이러한 기능을 지원한다면, 우리는 이를 이용해야합니다 (거의 모든 현대 프로세서가 이를 지원합니다). 

컴파일하는 동안 기본적으로 활성화됩니다. 따라서 OpenCV는 최적화 된 코드를 실행하면 활성화되고 그렇지 않으면 최적화되지 않은 코드가 실행됩니다. 

`cv2.useOptimized()`를 사용하여 활성화 / 비활성화되어 있는지 확인하고 `cv2.setUseOptimized()`를 사용하여 활성화 / 비활성화 할 수 있습니다. 간단한 예를 봅시다.

```python
# check if optimization is enabled
In [5]: cv2.useOptimized()
Out[5]: True

In [6]: %timeit res = cv2.medianBlur(img,49)
10 loops, best of 3: 34.9 ms per loop

# Disable it
In [7]: cv2.setUseOptimized(False)

In [8]: cv2.useOptimized()
Out[8]: False

In [9]: %timeit res = cv2.medianBlur(img,49)
10 loops, best of 3: 64.1 ms per loop
```

최적화 된 중앙 필터링은 최적화되지 않은 버전보다 ~2 배 빠릅니다. 소스를 확인하면 중간 필터링이 SIMD에 최적화 된 것을 볼 수 있습니다. 따라서 이것을 사용하여 코드 상단에서 최적화를 활성화 할 수 있습니다 (기본적으로 활성화되어 있음을 기억하십시오).

## IPython에서 성능 측정

때로는 두 가지 유사한 작업의 성능을 비교해야 할 수도 있습니다. IPython은 이것을 수행하는 마법 명령 `%timeit` 제공합니다. 더 정확한 결과를 얻기 위해 코드를 여러 번 실행합니다. 다시 한번 말하지만, 단일 라인 코드를 측정하는 데 적합합니다.

예를 들어, 다음 중 더 나은 연산이 어떤것인지 알 수 있습니까? 

- x = 5; y = x ** 2
- x = 5; y = x * x
- x = np.uint8([5]); y = x * x 또는 y = np.square(x)? 

우리는 IPython 셸에서 `%timeit`을 사용하여 찾아보겠습니다.

```python
In [10]: x = 5

In [11]: %timeit y=x**2
10000000 loops, best of 3: 73 ns per loop

In [12]: %timeit y=x*x
10000000 loops, best of 3: 58.3 ns per loop

In [15]: z = np.uint8([5])

In [17]: %timeit y=z*z
1000000 loops, best of 3: 1.25 us per loop

In [19]: %timeit y=np.square(z)
1000000 loops, best of 3: 1.16 us per loop
```

x = 5; y = x * x는 가장 빠르며 Numpy에 비해 20 배 정도 빠른 것을 확인할 수 있습니다. 배열 생성도 고려한다면 최대 100 배까지 빨라질 수 있습니다. 멋지지 않습니까? (Numpy devs는이 문제를 해결하기 위해 노력하고 있습니다)

> 파이썬 스칼라 연산은 Numpy 스칼라 연산보다 빠릅니다. 따라서 하나 또는 두 개의 요소를 포함하는 연산의 경우 Python 스칼라가 Numpy 배열보다 낫습니다. Numpy는 배열의 크기가 조금 더 클 때 이점을 취합니다.

우리는 한 가지 더 많은 예제를 시도 할 것입니다. 이번에는 동일한 이미지에 대해 `cv2.countNonZero()` 및 `np.count_nonzero()`의 성능을 비교합니다.

```python
In [35]: %timeit z = cv2.countNonZero(img)
100000 loops, best of 3: 15.8 us per loop

In [36]: %timeit z = np.count_nonzero(img)
1000 loops, best of 3: 370 us per loop
```

OpenCV 기능이 Numpy 기능보다 `25배` 빠릅니다.

> 일반적으로 OpenCV 기능은 Numpy 기능보다 빠릅니다. 따라서 동일한 작업을 위해 OpenCV 기능이 선호됩니다. 그러나 예외가 있을 수 있습니다. 특히 Numpy가 사본 대신 보기를 사용할 때 그렇습니다.

## 더 많은 IPython 마법 명령

성능, 프로파일링, 라인 프로파일링, 메모리 측정 등을 측정하는 몇 가지 다른 마법 명령이 있습니다. 그것들 모두 잘 문서화되어 있습니다. 따라서 해당 문서에 대한 링크만 제공됩니다. 관심있는 독자는 링크를 확인하시기 바랍니다.

## 성능 최적화 기법

Python과 Numpy의 최대 성능을 활용하기 위한 몇 가지 기술과 코딩 방법이 있습니다. 관련된 것만이 여기에 표시되며 링크는 중요한 출처에 제공됩니다. 여기서 주목해야 할 점은 먼저 알고리즘을 간단한 방식으로 구현하려고 시도한다는 것입니다. 일단 작동되면 프로파일링하고 병목 현상을 찾아 최적화하십시오.

- Python에서 가능한 한 루프를 사용하지 마십시오. 특히 이중 / 삼중 루프 등은 본질적으로 느립니다.
- Numpy와 OpenCV가 벡터 연산에 최적화되어 있기 때문에 알고리즘 / 코드를 가능한 최대로 벡터화하십시오.
- 캐시 일관성을 이용하십시오.
- 필요하지 않으면 배열의 복사본을 만들지 마십시오. 대신 보기를 사용해보십시오. 배열 복사는 비용이 많이 드는 작업입니다.

이러한 모든 작업을 수행 한 후에도 코드가 여전히 느리거나 큰 루프를 사용해야하는 경우에는 Cython과 같은 추가 라이브러리를 사용하여 더 빨리 수행 할 수 있습니다.


## 추가 리소스

- [파이썬 최적화 기법](https://wiki.python.org/moin/PythonSpeed/PerformanceTips)
- Scipy 강의 노트 - [고급 Numpy](http://www.scipy-lectures.org/advanced/advanced_numpy/index.html#advanced-numpy)
- [IPython의 타이밍 및 프로파일링](http://pynash.org/2013/03/06/timing-and-profiling/)



## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_core/py_optimization/py_optimization.html


<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>














