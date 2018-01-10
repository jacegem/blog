---
title: '[OpenCV-Python Tutorials] 18. Contours(윤곽선) 시작하기'
date: 2018-01-18 19:17:33
tags: [opencv, python, tutorial]
category:
- Programming
- Python
---

# [OpenCV-Python Tutorials] 18. Contours(윤곽선) 시작하기

## 목표

- Contours(윤곽선)이 무엇인지에 대한 이해
- 등고선 찾기, 등고선 그리기
- 함수: `cv2.findContours()`, `cv2.drawContours()`

## contours(윤곽)이란?

윤곽선은 동일한 색상 또는 강도를 갖는 모든 연속 점 (경계를 따라)을 결합하는 곡선으로 간단히 설명 할 수 있습니다. 윤곽선은 형상 분석 및 객체 감지 및 인식에 유용한 도구입니다.

- 정확성을 높이려면 이진 이미지를 사용하십시오. 따라서 윤곽선을 찾기 전에 임계점 또는 canny edge detection를 적용하십시오.
- `findContours` 함수는 소스 이미지를 수정합니다. 따라서 윤곽선을 찾은 후에도 소스 이미지를 원한다면 이미 다른 변수에 저장하십시오.
- OpenCV에서 윤곽선을 찾는 것은 검정 배경에서 흰색 물체를 찾는 것과 같습니다. 그러므로 찾을 물체는 흰색이어야하고 배경은 검은색이어야 합니다.

이진 이미지의 윤곽선을 찾는 방법을 살펴 보겠습니다.

```python
import numpy as np
import cv2

im = cv2.imread('test.jpg')
imgray = cv2.cvtColor(im,cv2.COLOR_BGR2GRAY)
ret,thresh = cv2.threshold(imgray,127,255,0)
image, contours, hierarchy = cv2.findContours(thresh,cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE)
```

![](https://goo.gl/7dwL43)

![](https://goo.gl/di8bFg)



`cv2.findContours()` 함수에는 세 개의 인수가 있습니다. 

- 첫 번째는 소스 이미지이고, 
- 두 번째는 등고선 검색 모드이고, 
- 세 번째는 등고선 근사 방법입니다. 

그리고 이미지, contours 및 계층을 출력합니다. `contours`은 이미지의 모든 윤곽을 파이썬으로 나타낸 목록입니다. 각각의 개별 윤곽은 객체의 경계 지점의 (x, y) 좌표의 객체 배열입니다.

> 나중에 두 번째 및 세 번째 인수와 계층 구조에 대해 자세히 설명합니다. 그때까지는 코드 예제에서 주어진 값이 모든 이미지에서 잘 작동합니다.

## contours(윤곽)을 그리는 방법?

contours을 그리려면 `cv2.drawContours` 함수가 사용됩니다. 또한 경계 지점이 있는 경우 모든 모양을 그리는 데 사용할 수 있습니다. 

- 첫 번째 인수는 소스 이미지이고, 
- 두 번째 인수는 파이썬 목록으로 전달되어야하는 contours이며, 
- 세 번째 인수는 contours 색인입니다 (개별 윤곽선을 그리는 데 유용합니다. 모든 윤곽선을 그리려면 -1을 전달합니다.)
- 나머지 인수는 색상, 두께 등입니다.

이미지에 모든 윤곽선을 그리려면 :

```python
img = cv2.drawContours(img, contours, -1, (0,255,0), 3)
```

개별 윤곽선을 그리려면 4 번째 윤곽선을 말하십시오.

```python
img = cv2.drawContours(img, contours, 3, (0,255,0), 3)
```

하지만 대부분의 경우 아래의 메소드가 유용합니다.

```python
cnt = contours[4]
img = cv2.drawContours(img, [cnt], 0, (0,255,0), 3)
```

> 마지막 두 가지 방법은 동일하지만 앞으로 나아갈 때 마지막 방법이 더 유용하다는 것을 알 수 있습니다.

![](https://goo.gl/2cYgEu)


## Contour 근사 방법

이것은 `cv2.findContours` 함수의 세 번째 인수입니다. 그것은 실제로 무엇을 나타낼까요?

위에서 우리는 contours 이 동일한 강도를 지닌 모양의 경계라고 이야기했습니다. 도형 경계의 (x, y) 좌표를 저장합니다. 그러나 그것은 모든 좌표를 저장할까요? 윤곽 근사법으로 지정됩니다.

`cv2.CHAIN_APPROX_NONE`를 넘겨 주면 모든 경계 지점이 저장됩니다. 그러나 실제로 모든 포인트가 필요할까요? 예를 들어, 직선의 윤곽을 발견했습니다. 그 라인을 나타 내기 위해 라인의 모든 포인트가 필요할까요? 아닙니다. 우리는 그 선의 두 종점만 있으면 됩니다. 이것이 `cv2.CHAIN_APPROX_SIMPLE`의 기능입니다. 모든 중복 점을 제거하고 윤곽을 압축하여 메모리를 절약합니다.

아래 사각형 이미지는 이 기법을 보여줍니다. 등고선 배열(파란색으로 그려 짐)의 모든 좌표에 원을 그립니다. 첫 번째 이미지는 `cv2.CHAIN_APPROX_NONE` (734개의 점)으로 얻은 점을 표시하고 두 번째 이미지는 `cv2.CHAIN_APPROX_SIMPLE` (4개의 점만)으로 표시합니다. 얼마나 많은 메모리가 절약되는지 확인하시기 바랍니다.

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/none.jpg)





## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_contours/py_contours_begin/py_contours_begin.html#contours-getting-started




<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>






