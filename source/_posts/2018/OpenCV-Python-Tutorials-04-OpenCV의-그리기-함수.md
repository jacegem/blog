---
title: '[OpenCV-Python Tutorials 04] OpenCV의 그리기 함수'
date: 2018-01-04 19:11:27
tags: [opencv, python, tutorial]
category:
- Programming
- Python
---

# [OpenCV-Python Tutorials 04] OpenCV의 그리기 함수

모든 파일은 [Github](https://github.com/jacegem/OpenCV-Python-Tutorials)에서 확인 할 수 있습니다.

## 목표

- OpenCV를 사용하여 다양한 기하학적 모양을 그리는 방법을 배웁니다.
- cv2.line(), cv2.circle(), cv2.rectangle(), cv2.ellipse(), cv2.putText() 등의 함수를 배웁니다.

## 코드

위의 모든 기능에서 다음과 같은 몇 가지 일반적인 인수가 표시됩니다.

- img : 도형을 그리려는 이미지
- color : 도형의 색. `BGR`의 경우에는 튜플 (예 : (255,0,0), 파란색)으로 전달합니다. 그레이 스케일의 경우 스칼라 값을 전달하십시오.
- thickness : 선이나 원 등의 두께입니다. 원과 같은 닫힌 그림에 `-1`이 전달되면 모양이 채워집니다. 기본 두께 = `1`
- lineType : 8-connected, 앤티 앨리어싱 된 라인 등의 라인 유형. 기본적으로 8-connected 입니다. `cv2.LINE_AA`는 커브를 위해 멋진 앤티 앨리어싱 된 선을 제공합니다.

> 8-connected은 가장자리 또는 모서리 중 하나에 닿는 모든 픽셀에 이웃합니다.

![An 8-Connected Neighborhood](http://radio.feld.cvut.cz/matlab/toolbox/images/binarya.gif)

![A 4-Connected Neighborhood](http://radio.feld.cvut.cz/matlab/toolbox/images/binary2a.gif)

## 선 그리기

선을 그리려면 선의 `시작`과 `끝` 좌표를 전달해야 합니다. 우리는 검은색 이미지를 만들고 왼쪽 위부터 오른쪽 아래까지 `파란` 선을 그릴 것입니다.

```python
import numpy as np
import cv2

# Create a black image
img = np.zeros((512,512,3), np.uint8)

# Draw a diagonal blue line with thickness of 5 px
img = cv2.line(img,(0,0),(511,511),(255,0,0),5)
```

선은 그렸지만 보이지 않으므로, 보이는 코드를 추가합니다.

```python
import numpy as np
import cv2

# Create a black image
img = np.zeros((512,512,3), np.uint8)

# Draw a diagonal blue line with thickness of 5 px
img = cv2.line(img,(0,0),(511,511),(255,0,0),5)

cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](https://goo.gl/ia6JzX)

```python
img = cv2.line(img,(0,0),(511,511),(255,0,0),5)
```

`cv2.line()` 함수의 파라미터 값은 다음과 같습니다.

- img: 배경
- (0,0): 시작좌표
- (511,511): 마지막좌표
- (255,0,0): 컬러는 `BGR` 이므로, 파란색
- 5: 두께

> `BGR` 은 Blue, Green, Red 를 의미합니다.


## 직사각형 그리기 

직사각형을 그리려면 직사각형의 `왼쪽 위`모서리와 `오른쪽 하단` 모서리가 필요합니다. 이번에는 이미지의 오른쪽 상단에 `녹색` 사각형을 그려 보겠습니다.

```python
import numpy as np
import cv2

# Create a black image
img = np.zeros((512,512,3), np.uint8)

img = cv2.rectangle(img,(384,0),(510,128),(0,255,0),3)

cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```
```python
img = cv2.rectangle(img,(384,0),(510,128),(0,255,0),3)
```
![](https://goo.gl/4xoYfS)

`cv2.rectangle` 함수의 파라미터 값은 다음과 같습니다. 

- img: 배경
- (384,0) : 시작 좌표 (좌측 상단)
- (510,128): 끝 좌표 (우측 하단)
- (0,255,0): `BGR` 이므로 녹색
- 3 : 두께



## 원 그리기 

원을 그리려면 `중심 좌표`와 `반지름`이 필요합니다. 위에서 그린 직사각형 안에 원을 그립니다.

```python
import numpy as np
import cv2

# Create a black image
img = np.zeros((512,512,3), np.uint8)

img = cv2.circle(img,(447,63), 63, (0,0,255), -1)

cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](https://goo.gl/xsNXyT)

```python
img = cv2.circle(img,(447,63), 63, (0,0,255), -1)
```

cv2.circle() 함수의 파라미터는 다음과 같습니다.

- img: 배경
- (447,63): 중심좌표
- 63: 반지름
- (0,0,255): 컬러 `BGR` 이므로 빨간색
- `-1` : 두께가 `-1` 이므로 색상 채우기

## 타원 그리기 

타원을 그리려면 몇 가지 인수를 전달해야합니다. 하나의 인수는 `중심 위치 (x, y)`입니다. 다음 인수는 `축 길이` (장축 길이, 보조 축 길이)입니다. `angle`은 반 시계 방향으로 타원의 회전 각도입니다. `startAngle`과 `endAngle`은 장축에서 시계 방향으로 측정 된 타원 호의 시작과 끝을 나타냅니다. 0과 360의 값을 부여하면 타원이됩니다. 자세한 내용은 cv2.ellipse() 문서를 확인하십시오. 아래 예제에서는 이미지의 중앙에 반 타원을 그립니다.

```python
import numpy as np
import cv2

# Create a black image
img = np.zeros((512,512,3), np.uint8)

img = cv2.ellipse(img,(256,256),(100,50),0,0,180,255,-1)

cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```
![](https://goo.gl/Br73T8)

```python
img = cv2.ellipse(img,(256,256),(100,50),0,0,180,255,-1)
```

cv2.ellipse() 함수의 파라미터는 다음과 같습니다. 

- img: 배경
- (256,256): 중심점
- (100,50): 장축길이, 보조축길이
- 0: 회전 각도
- 0: 시각 각도
- 180: 종료 각도
- 255: 색상, `(255,0,0)`로 입력해도 동일한 결과를 얻을 수 있습니다. 
- -1: 두께, 채우기

![](http://docs.opencv.org/2.4/_images/ellipse.png)


## 다각형 그리기 

다각형을 그리려면 먼저 `정점 좌표`가 필요합니다. 이러한 점을 모양 `ROWSx1x2`의 배열로 만듭니다. 여기서 ROWS는 정점의 수이고 `int32` 유형이어야합니다. 여기서 우리는 4 개의 꼭지점이있는 작은 폴리곤을 노란색으로 그립니다.

```python
import numpy as np
import cv2

# Create a black image
img = np.zeros((512,512,3), np.uint8)

pts = np.array([[10,5],[20,30],[70,20],[50,10]], np.int32)
pts = pts.reshape((-1,1,2))
img = cv2.polylines(img,[pts],True,(0,255,255))

cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```
![](https://goo.gl/uWiVdu)

```python
pts = np.array([[10,5],[20,30],[70,20],[50,10]], np.int32)
pts = pts.reshape((-1,1,2))
img = cv2.polylines(img,[pts],True,(0,255,255))
```

cv2.polylines() 함수의 파라미터는 다음과 같습니다. 

- img: 배경
- [pts]: 좌표 배열
- True: 닫힘 여부 설정
- (0,255,255)): 컬러

> 세 번째 인수가 `False`인 경우 닫힌 모양이 아닌 모든 점에 합류하는 폴리 라인을 가져옵니다.

> 여러 줄을 그리려면 `cv2.polylines()`를 사용할 수 있습니다. 그리기 원하는 모든 선 목록을 만들어 함수에 전달하십시오. 모든 선은 개별적으로 그려집니다. 각 행에 대해 `cv2.line()`을 호출하는 것보다 더 효율적이고 빠른 방법으로 행 그룹을 그립니다.

## 이미지에 텍스트 추가하기

이미지에 텍스트를 넣으려면 다음을 지정해야합니다.

- 입력할 텍스트 데이터
- 놓을 위치의 좌표 (즉, 데이터가 시작되는 왼쪽 하단 모서리).
- 글꼴 유형 (지원되는 글꼴은 `cv2.putText()` 문서를 확인하십시오)
- 글꼴 크기 (글꼴 크기 지정)
- color, thickness, lineType 등과 같은 일반적인 것들. 더 나은 모양을 위해 `lineType = cv2.LINE_AA`가 권장됩니다.

우리는 우리의 이미지에 흰색으로 `Hello OpenCV!!!`를 씁니다.

```python
import numpy as np
import cv2

# Create a black image
img = np.zeros((512,512,3), np.uint8)

font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(img,'Hello OpenCV!!!',(10,500), font, 2, (255,255,255), 2, cv2.LINE_AA)

cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```
![](https://goo.gl/QUKhT8)

```python
font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(img,'OpenCV',(10,500), font, 4,(255,255,255),2,cv2.LINE_AA)
```

cv2.putText()함수의 파라미터는 다음과 같습니다. 

- img: 배경
- 'OpenCV': 문자열
- (10,500): 시작위치
- font: 폰트
- 4: 폰트 크기
- (255,255,255): 색상
- 2: 두께
- cv2.LINE_AA: 선 유형

> LINE_4 = 4
> LINE_8 = 8
> LINE_AA = 16


## 결과

이제 우리가 그린 그림의 최종 결과를 볼 시간입니다. 이전 기사에서 공부하면서 이미지를 표시하여 이미지를 봅니다.

```python
import numpy as np
import cv2

# Create a black image
img = np.zeros((512,512,3), np.uint8)

# Draw a diagonal blue line with thickness of 5 px
img = cv2.line(img,(0,0),(511,511),(255,0,0),5)
img = cv2.rectangle(img,(384,0),(510,128),(0,255,0),3)
img = cv2.circle(img,(447,63), 63, (0,0,255), -1)
img = cv2.ellipse(img,(256,256),(100,50),0,0,180,(255,0,0),-1)

pts = np.array([[10,5],[20,30],[70,20],[50,10]], np.int32)
pts = pts.reshape((-1,1,2))
img = cv2.polylines(img,[pts],True,(0,255,255))

font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(img,'Hello OpenCV!!!',(10,500), font, 2, (255,255,255), 2, cv2.LINE_AA)

cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```


![](https://goo.gl/v4G8mP)

## 추가 리소스

타원 함수에 사용 된 각도는 원형 각도가 아닙니다. 자세한 내용은이 [토론](http://answers.opencv.org/question/14541/angles-in-ellipse-function/)을 방문하십시오.

## 연습 문제

OpenCV에서 사용할 수있는 그리기 기능을 사용하여 OpenCV 로고 만들기



## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_gui/py_drawing_functions/py_drawing_functions.html
- http://radio.feld.cvut.cz/matlab/toolbox/images/binary6.html
- http://docs.opencv.org/2.4/modules/core/doc/drawing_functions.html

<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>


