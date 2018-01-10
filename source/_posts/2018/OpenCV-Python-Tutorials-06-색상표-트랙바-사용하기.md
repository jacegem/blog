---
title: '[OpenCV-Python Tutorials 06] 색상표 트랙바 사용하기'
date: 2018-01-06 19:12:16
tags:
---

# [OpenCV-Python Tutorials 06] 색상표 트랙바 사용하기



## 목표

- 트랙바를 OpenCV 창에 바인딩하는 방법 배우기
- cv2.getTrackbarPos(), cv2.createTrackbar() 등의 함수를 배웁니다.

## 코드 데모

여기에서는 지정한 색상을 보여주는 간단한 응용 프로그램을 만듭니다. B, G, R 각 색상을 지정하는 색상과 세 개의 트랙바를 보여주는 창이 있습니다. 트랙바를 슬라이드하고 그에 따라 창 색상이 변경됩니다. 기본적으로 초기 색은 검은색으로 설정됩니다.

`cv2.getTrackbarPos()` 함수의 경우 첫 번째 인수는 트랙바 이름이고, 두 번째 인수는 연결될 창 이름이며 세 번째 인수는 기본값이며 네 번째 매개 변수는 최대값이며 다섯 번째 매개 변수는 실행되는 콜백 함수입니다 트랙볼 값이 매번 변경됩니다. 콜백 함수에는 항상 트랙바 위치인 기본 인수가 있습니다. 우리의 경우 함수는 아무 것도하지 않으므로 간단히 패스합니다.

파라미터 목록

1. 트랙바 이름
2. 연결될 창 이름
3. 기본값
4. 최대값
5. 콜백 함수

트랙바의 또 다른 중요한 응용 프로그램은 단추 또는 스위치로 사용하는 것입니다. OpenCV는 기본적으로 버튼 기능이 없습니다. 따라서 트랙바를 사용하여 이러한 기능을 사용할 수 있습니다. 우리의 응용 프로그램에서는 스위치가 `ON` 인 경우에만 응용 프로그램이 작동하는 스위치 하나를 만들었습니다. 그렇지 않으면 화면이 항상 검은색입니다.

```python
import cv2
import numpy as np

def nothing(x):
    pass

# Create a black image, a window
img = np.zeros((300,512,3), np.uint8)
cv2.namedWindow('image')

# create trackbars for color change
cv2.createTrackbar('R','image',0,255,nothing)
cv2.createTrackbar('G','image',0,255,nothing)
cv2.createTrackbar('B','image',0,255,nothing)

# create switch for ON/OFF functionality
switch = '0 : OFF \n1 : ON'
cv2.createTrackbar(switch, 'image',0,1,nothing)

while(1):
    cv2.imshow('image',img)
    k = cv2.waitKey(1) & 0xFF
    if k == 27:
        break

    # get current positions of four trackbars
    r = cv2.getTrackbarPos('R','image')
    g = cv2.getTrackbarPos('G','image')
    b = cv2.getTrackbarPos('B','image')
    s = cv2.getTrackbarPos(switch,'image')

    if s == 0:
        img[:] = 0
    else:
        img[:] = [b,g,r]

cv2.destroyAllWindows()
```

애플리케이션의 스크린 샷은 다음과 같습니다.

![](https://goo.gl/kqrSh4)


> `ESC` 키를 눌러서 윈도우를 종료합니다.


## 연습 문제

트랙 바를 사용하여 색상 및 브러쉬 반경을 조정할 수있는 페인트 응용 프로그램을 만듭니다. 그리기에 대해서는 마우스 조작에 대한 이전 자습서를 참조하십시오.

```python
import cv2
import numpy as np

def nothing(x):
    pass


# 마우스 콜백 함수
def draw_circle(event, x, y, flags, param):
    if event == cv2.EVENT_LBUTTONDBLCLK:
        # get current positions of four trackbars
        r = cv2.getTrackbarPos('R', 'image')
        g = cv2.getTrackbarPos('G', 'image')
        b = cv2.getTrackbarPos('B', 'image')
        d = cv2.getTrackbarPos('Distance', 'image')

        cv2.circle(img, (x, y), d, (b, g, r), -1)

# Create a black image, a window
img = np.zeros((300,512,3), np.uint8)
cv2.namedWindow('image')

# create trackbars for color change
cv2.createTrackbar('R','image',0,255, nothing)
cv2.createTrackbar('G','image',0,255, nothing)
cv2.createTrackbar('B','image',0,255, nothing)
cv2.createTrackbar('Distance','image',0,255, nothing)

cv2.setMouseCallback('image', draw_circle)

while(1):
    cv2.imshow('image',img)
    k = cv2.waitKey(1) & 0xFF
    if k == 27:
        break

cv2.destroyAllWindows()
```

![](https://goo.gl/bFrPVa)



## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_gui/py_trackbar/py_trackbar.html

<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>


