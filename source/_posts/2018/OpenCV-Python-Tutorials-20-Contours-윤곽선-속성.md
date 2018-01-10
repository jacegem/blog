---
title: '[OpenCV-Python Tutorials] 20. Contours(윤곽선) 속성'
date: 2018-01-20 19:18:19
tags: [opencv, python, tutorial]
category:
- Programming
- Python
---

# [OpenCV-Python Tutorials] 20. Contours(윤곽선) 속성

여기서는 Solidity, Equivalent Diameter, Mask image, Mean Intensity 등과 같은 객체에서 자주 사용되는 속성을 추출하는 방법을 배웁니다. 더 많은 기능은 [Matlab regionprops 문서](http://www.mathworks.in/help/images/ref/regionprops.html)에서 찾을 수 있습니다.

(주의 : Centroid, Area, Perimeter 등은 이전 장에서 보았습니다)

## 1. 종횡비 (Aspect Ratio)

이것은 객체의 경계 사각형의 너비와 높이의 비율입니다.

$$
Aspect \; Ratio = \frac{Width}{Height}
$$

```python
x,y,w,h = cv2.boundingRect(cnt)
aspect_ratio = float(w)/h
```

## 2. 범위 (Extent)

Extent는 윤곽선 영역과 경계 사각형 영역의 비율입니다.

$$
Extent = \frac{Object \; Area}{Bounding \; Rectangle \; Area}
$$

## 3. 견고성 (Solidity)

Solidity는 Contour 영역과 볼록한 선체 영역의 비율입니다.

$$
Solidity = \frac{Contour \; Area}{Convex \; Hull \; Area}
$$

```python
area = cv2.contourArea(cnt)
hull = cv2.convexHull(cnt)
hull_area = cv2.contourArea(hull)
solidity = float(area)/hull_area
```

## 4. 등가 지름 (Equivalent Diameter)

등가 지름은 윤곽 영역과 동일한 영역의 원의 직경입니다.

$$
Equivalent \; Diameter = \sqrt{\frac{4 \times Contour \; Area}{\pi}}
$$

```python
area = cv2.contourArea(cnt)
equi_diameter = np.sqrt(4*area/np.pi)
```

## 5. 오리엔테이션

Orientation은 객체가 향하는 각도입니다. 다음 방법은 주요 축과 보조 축 길이를 제공합니다.

```python
(x,y),(MA,ma),angle = cv2.fitEllipse(cnt)
```

## 6. 마스크와 픽셀 포인트

어떤 경우에는 그 대상을 구성하는 모든 점이 필요할 수도 있습니다. 다음과 같이 수행 할 수 있습니다.

```python
mask = np.zeros(imgray.shape,np.uint8)
cv2.drawContours(mask,[cnt],0,255,-1)
pixelpoints = np.transpose(np.nonzero(mask))
#pixelpoints = cv2.findNonZero(mask)
```

여기에는 Numpy 함수를 사용하는 방법과 OpenCV 함수를 사용하는 두 가지 방법 (마지막 주석 처리 된 줄)이 있습니다. 
결과도 동일하지만 약간의 차이가 있습니다. 
Numpy는 (행, 열) 형식으로 좌표를 제공하고 OpenCV는 (x, y) 형식으로 좌표를 제공합니다. `row = x` 및 `column = y`입니다.

## 7. 최대 값, 최소값 및 위치

이 매개 변수는 마스크 이미지를 사용하여 찾을 수 있습니다.

```python
min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(imgray,mask = mask)
```

## 9. 익스트림 포인트

Extreme Points는 객체의 가장 위쪽, 가장 아래쪽, 가장 오른쪽 및 가장 왼쪽의 점을 의미합니다.

```python
leftmost = tuple(cnt[cnt[:,:,0].argmin()][0])
rightmost = tuple(cnt[cnt[:,:,0].argmax()][0])
topmost = tuple(cnt[cnt[:,:,1].argmin()][0])
bottommost = tuple(cnt[cnt[:,:,1].argmax()][0])
```

예를 들어 인도지도에 적용하면 다음과 같은 결과가 나타납니다.

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/extremepoints.jpg)

## 연습문제

아직 MATLAB 지역 프로젝트 문서에 남아있는 몇 가지 기능이 있습니다. 그것들을 구현해 보세요.

## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_contours/py_contour_properties/py_contour_properties.html


<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>













