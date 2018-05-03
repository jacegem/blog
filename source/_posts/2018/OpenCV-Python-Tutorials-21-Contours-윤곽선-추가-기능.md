---
title: '[OpenCV-Python Tutorials] 21. Contours(윤곽선) 추가 기능'
date: 2018-01-21 19:18:37
tags: [opencv, python, tutorial]
category:
- Programming
- Python
---

# [OpenCV-Python Tutorials] 21. Contours(윤곽선) 추가 기능

## 목표

- 볼록성 결함 및 그 결함을 찾는 방법.
- 점에서 다각형까지 최단 거리 찾기
- 다른 모양 맞추기

# 이론 및 코드

## 1. 볼록성 결함

윤곽선에 대한 두 번째 장에서 볼록한 선체가 무엇인지 보았습니다. 이 선체에서 물체가 이탈하면 볼록 결함으로 간주 될 수 있습니다.

OpenCV에는 이것을 찾을 수있는 기성 함수가 제공됩니다 `cv2.convexityDefects()`. 기본 함수 호출은 다음과 같습니다. 

```python
hull = cv2.convexHull(cnt,returnPoints = False)
defects = cv2.convexityDefects(cnt,hull)
```

> convexity 결함을 찾기 위해 convex hull을 찾는 동안 `returnPoints = False`를 전달해야한다는 것을 기억하십시오.

각 행에 [시작점, 끝점, 가장 먼 지점, 가장 가까운 지점까지의 대략적인 거리] 값이 들어있는 배열을 반환합니다. 이미지를 사용하여 시각화 할 수 있습니다. 우리는 시작점과 끝점을 연결하는 선을 그린 다음 먼 지점에 원을 그립니다. 처음 세 개의 값은 `cnt`의 인덱스임을 기억하십시오. 그래서 우리는 그 값들을 `cnt`에서 가져와야 합니다.

```python
import cv2
import numpy as np

img = cv2.imread('star.jpg')
img_gray = cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
ret, thresh = cv2.threshold(img_gray, 127, 255,0)
contours,hierarchy = cv2.findContours(thresh,2,1)
cnt = contours[0]

hull = cv2.convexHull(cnt,returnPoints = False)
defects = cv2.convexityDefects(cnt,hull)

for i in range(defects.shape[0]):
    s,e,f,d = defects[i,0]
    start = tuple(cnt[s][0])
    end = tuple(cnt[e][0])
    far = tuple(cnt[f][0])
    cv2.line(img,start,end,[0,255,0],2)
    cv2.circle(img,far,5,[0,0,255],-1)

cv2.imshow('img',img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

결과

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/defects.jpg)

## 2. 점 다각형 테스트

이 함수는 이미지의 점과 윤곽 사이의 최단 거리를 찾습니다. 거리가 윤곽선 외부에 있을 때 `음수`이고, 점이 내부에 있을 때 `양수`이고, 점이 윤곽선 위에 있으면 `0`입니다.

예를 들어 다음과 같이 점 (50,50)을 확인할 수 있습니다.

```python
dist = cv2.pointPolygonTest(cnt,(50,50),True)
```

함수에서 세 번째 인수는 `measureDist`입니다. `Ture`이면 부호가 있는 거리를 찾습니다. `False`이면 점이 내부 또는 외부인지 윤곽선위에 있는지 찾습니다 (각각 +1, -1, 0을 반환).

> 거리를 찾고 싶지 않다면 세 번째 인수가 `False`임을 확인하십시오. 왜냐하면 시간이 많이 소요되는 프로세스이기 때문입니다. 따라서 `False`로 설정하면 약 2-3 배의 속도 향상을 얻을 수 있습니다.


## 3. 도형 맞추기

OpenCV에는 두 개의 도형 또는 두 개의 윤곽선을 비교하고 유사성을 나타내는 메트릭을 반환하는 `cv2.matchShapes()` 함수가 있습니다. 결과가 낮을수록 더 일치합니다. hu-moment 값을 기반으로 계산됩니다. 다른 측정 방법은 문서에 설명되어 있습니다.

```python
import cv2
import numpy as np

img1 = cv2.imread('star.jpg',0)
img2 = cv2.imread('star2.jpg',0)

ret, thresh = cv2.threshold(img1, 127, 255,0)
ret, thresh2 = cv2.threshold(img2, 127, 255,0)
contours,hierarchy = cv2.findContours(thresh,2,1)
cnt1 = contours[0]
contours,hierarchy = cv2.findContours(thresh2,2,1)
cnt2 = contours[0]

ret = cv2.matchShapes(cnt1,cnt2,1,0.0)
print ret
```

아래에 주어진 다른 모양으로 일치하는 모양을 시도하였습니다.

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/matchshapes.jpg)

결과는 다음과 같습니다.

- 이미지 A와 이미지 A(일치) = 0.0
- 이미지 A와 이미지 B = 0.001946
- 이미지 A와 이미지 C = 0.326911

이미지의 회전이 비교에 많은 영향을 미치지 않습니다.

> Hu-Moments은 translation, 회전 및 크기에 영향을 받지 않는 seven moments입니다. 일곱 번째 것은 왜곡 불변입니다. 이러한 값은 `cv2.HuMoments()` 함수를 사용하여 찾을 수 있습니다.

## 연습 문제

- `cv2.pointPolygonTest()`에 대한 설명서를 확인하십시오. 빨간색과 파란색으로 멋진 이미지를 찾을 수 있습니다. 모든 픽셀에서 흰색 곡선까지의 거리를 나타냅니다. 곡선 안에있는 모든 픽셀은 거리에 따라 파란색입니다. 마찬가지로 외부 포인트는 빨간색입니다. 윤곽선 모서리는 흰색으로 표시됩니다. 문제는 간단합니다. 그런 거리 표현을 만드는 코드를 작성하십시오.
- `cv2.matchShapes()`를 사용하여 숫자 나 문자의 이미지를 비교하십시오. (OCR을 향한 간단한 단계 일 것입니다)







## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_contours/py_contours_more_functions/py_contours_more_functions.html


<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>


