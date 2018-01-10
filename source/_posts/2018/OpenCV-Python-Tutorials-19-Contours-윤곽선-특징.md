---
title: '[OpenCV-Python Tutorials] 19. Contours(윤곽선) 특징'
date: 2018-01-19 19:17:53
tags: [opencv, python, tutorial]
category:
- Programming
- Python
---

# [OpenCV-Python Tutorials] 19. Contours(윤곽선) 특징

## 목표

- 면적, 둘레, 중심, 경계 상자 등 윤곽선의 다른 특징
- 윤곽선과 관련된 다양한 기능

## 1. Moments

Image moments은 물체의 중심, 물체의 면적 등과 같은 일부 기능을 계산하는 데 도움이 됩니다.

함수 `cv2.moments()`는 모든 moment 값의 사전을 계산합니다. 아래를 참조하십시오 :

```python
import cv2
import numpy as np

img = cv2.imread('star.jpg',0)
ret,thresh = cv2.threshold(img,127,255,0)
contours,hierarchy = cv2.findContours(thresh, 1, 2)

cnt = contours[0]
M = cv2.moments(cnt)
print M
```

이 moments부터 면적, 중심 등 유용한 데이터를 추출 할 수 있습니다. Centroid는 $$C_x = \frac{M_{10}}{M_{00}}$$ 및 $$C_y = \frac{M_{01}}{M_{00}}$$ 로 부터 얻을 수 있습니다. 이 작업은 다음과 같이 수행 할 수 있습니다.

```python
cx = int(M['m10']/M['m00'])
cy = int(M['m01']/M['m00'])
```

## 2. Contour Area(영역)

Contour 영역은 함수 `cv2.contourArea()` 또는 moments `M['m00']`에 의해 주어집니다.

```python
area = cv2.contourArea(cnt)
```

## 3. Contour Perimeter(경계선)

`arc length` 라고도 합니다. `cv2.arcLength()` 함수를 사용하여 찾을 수 있습니다. 두 번째 인수는 `True`로 전달 된 경우 닫힌 contour가 되며 아닌 경우에는 곡선이 됩니다. 

```python
perimeter = cv2.arcLength(cnt,True)
```

## 4. Contour Approximation(근사)

우리가 지정한 정밀도에 따라 Contour 모양을 정점 수가 적은 다른 모양으로 근사합니다. [Douglas-Peucker 알고리즘](https://en.wikipedia.org/wiki/Ramer-Douglas-Peucker_algorithm)의 구현입니다. 

아래에 표시된 첫 이미지처럼 나쁜 모양인 경우 이미지의 사각형을 찾으려고 노력하지만, 이미지의 몇 가지 문제에 대한 완전한 사각형을 얻을 수 없습니다. 이때 이 함수를 사용하여 형상을 근사 할 수 있습니다. 

이 두 번째 인수는 `epsilon`이라 불리며, 엡실론은 윤곽에서 근사 된 윤곽까지의 최대 거리입니다. 정밀도 매개 변수입니다. 올바른 출력을 얻기 위해서는 현명한 `epsilon`의 선택이 필요합니다.

```python
epsilon = 0.1*cv2.arcLength(cnt,True)
approx = cv2.approxPolyDP(cnt,epsilon,True)
```

아래의 두 번째 이미지는 녹색 선은 `epsilon = 10% of arc length`의 추세선을 보여줍니다. 제 3의 이미지는 `epsilon = 1% of the arc length`에서 같은 것을 나타낸 것입니다. 세번째 인수는 곡선이 닫혀 있는지 여부를 지정합니다.

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/approx.jpg)

## 5. Convex Hull

Convex Hull은 contour approximation와 비슷해 보이지만 그렇지 않습니다. (같은 결과를 얻을지도 모릅니다.) 여기에서는 `cv2.convexHull()` 함수 곡선에 볼록 결함을 확인하고 수정합니다. 일반적으로 볼록한 곡선은 항상 불룩 또는 적어도 평탄한 곡선입니다. 그리고 안쪽으로 부풀어 오른 경우는 볼록 결함이라고 합니다. 예를 들어, 다음 단계의 이미지를 확인하십시오. 빨간색 선은 손의 볼록 선체를 보여줍니다. 양면 화살표 표시는 등고선에서 선체의 국부적인 최대 편차인 볼록 결함을 나타냅니다.

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/convexitydefects.jpg)

구문에 대해 약간 논의 할 사항이 있습니다.

```python
hull = cv2.convexHull(points[, hull[, clockwise[, returnPoints]]
```

인수 상세 정보 :

- point: 포인트는 우리가 전달하는 윤곽입니다.
- hull: 선체는 출력이지만, 일반적으로 피합니다.
- clockwise: 시계 방향 : 방향 플래그. `True`이면 출력 볼록 선체는 시계 방향입니다. 그렇지 않으면 시계 반대 방향입니다.
- returnPoints : 기본적으로 `True`입니다. 다음 선체 점의 좌표를 반환합니다. `False`이면 선체 점에 대응하는 윤곽 점의 인덱스를 돌려줍니다.
 
따라서 위의 이미지처럼 볼록 선체를 얻으려면 다음과 같이하면 됩니다.

```python
hull = cv2.convexHull(cnt)
```

그러나 convexity 결함을 찾으려면 `eturnPoints = False`를 전달해야 합니다. 이를 이해하기 위해 위의 사각형 이미지를 사용합니다. 처음에는 그 윤곽선을 cnt로 찾았습니다. 이제 `returnPoints = True`로 볼록한 선체를 발견했습니다. 

다음 값을 얻었습니다 : [[234 202]], [[51 202]], [[514 79]], [499] 직사각형의 점. 이제 returnPoints = False로 동일하게 수행하면 [[129], [67], [0], [142]] 결과를 얻습니다. 이것들은 등고선의 대응점의 색인입니다. 예를 들어, 첫 번째 값을 확인하십시오 : cnt [129] = [[234, 202]] 이는 첫 번째 결과와 같습니다 (다른 것들도 마찬가지입니다).

그러나 convexity 결함을 찾으려면 `eturnPoints = False`를 전달해야 합니다. 그것을 이해하기 위해서는 위의 사각형 이미지를 다룹니다. 처음에는 그 윤곽선을 cnt로 찾았습니다. `returnPoints = True`로 볼록 선체를 찾아내어 사각형의 점 값을 얻었습니다. `[[[234 202], [51 202], [51 79], [234 79]]]` . 이번에는 `returnPoints = False` 처럼하면 결과는 `[[[129] [67] [0] [142]]]`입니다. 이들은 등고선의 대응점의 인덱스입니다. 예를 들어, 첫 번째 결과와 같은 `cnt[129] = [[234,202]]`의 첫 번째 값을 확인합니다 (다른 경우도 마찬가지).

볼록 결함에 대해 논의 할 때 다시 그것을 볼 것입니다.

## 6. Convexity 확인

`cv2.isContourConvex()`는 커브가 볼록인지 여부를 확인하는 기능이 있습니다. True 또는 False를 반환합니다. Not a big deal.

```python
k = cv2.isContourConvex(cnt)
```

## 7. 경계 사각형

경계 사각형은 2 가지가 있습니다.

### 7.a. 직선 경계 사각형

그것은 직사각형이며 객체의 회전을 고려하지 않습니다. 따라서 경계 사각형의 영역은 최소가 되지 않습니다. 이것은 `cv2.boundingRect()` 함수를 통해 찾을 수 있습니다. 

(x, y)를 직사각형의 왼쪽 위 좌표로 하고 (w, h)를 폭과 높이로 합니다.

```python
x,y,w,h = cv2.boundingRect(cnt)
img = cv2.rectangle(img,(x,y),(x+w,y+h),(0,255,0),2)
```


## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_contours/py_contour_features/py_contour_features.html#moments
- https://acidsound.github.io/transplus/


<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>








