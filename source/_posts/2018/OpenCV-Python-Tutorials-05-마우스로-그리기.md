---
title: '[OpenCV-Python Tutorials 05] 마우스로 그리기'
date: 2018-01-05 19:11:51
tags:
---


# [OpenCV-Python Tutorials 05] 마우스로 그리기

모든 파일은 [Github](https://github.com/jacegem/OpenCV-Python-Tutorials)에서 확인 할 수 있습니다.

## 목표

- OpenCV에서 마우스 이벤트 처리 방법 배우기
- 다음 함수를 배웁니다. cv2.setMouseCallback()

## 간단한 데모

여기서 우리는 두 번 클릭 할 때마다 이미지에 원을 그리는 간단한 애플리케이션을 만듭니다.

먼저 마우스 이벤트가 발생할 때 실행되는 마우스 `콜백` 함수를 만듭니다. 마우스 이벤트는 왼쪽 버튼 누를 때, 왼쪽 버튼 놓을 때, 왼쪽 버튼 두 번 클릭 등과 같은 마우스 관련 항목 일 수 있습니다. 모든 마우스 이벤트에 대한 좌표 (x, y)를 제공합니다. 이 이벤트와 위치로 우리는 무엇이든 할 수 있습니다. 사용 가능한 이벤트를 모두 나열하려면 Python 터미널에서 다음 코드를 실행하십시오.

```python
>>> import cv2
>>> events = [i for i in dir(cv2) if 'EVENT' in i]
>>> print events
```

> python3 의 경우에는 `print(events)` 로 입력합니다.

결과는 아래와 같습니다. 

```python
>>> import cv2
>>> events = [i for i in dir(cv2) if 'EVENT' in i]
>>> print(events)
['EVENT_FLAG_ALTKEY', 'EVENT_FLAG_CTRLKEY', 'EVENT_FLAG_LBUTTON', 'EVENT_FLAG_MBUTTON', 'EVENT_FLAG_RBUTTON', 'EVENT_FLAG_SHIFTKEY', 'EVENT_LBUTTONDBLCLK', 'EVENT_LBUTTONDOWN', 'EVENT_LBUTTONUP', 'EVENT_MBUTTONDBLCLK', 'EVENT_MBUTTONDOWN', 'EVENT_MBUTTONUP', 'EVENT_MOUSEHWHEEL', 'EVENT_MOUSEMOVE', 'EVENT_MOUSEWHEEL', 'EVENT_RBUTTONDBLCLK', 'EVENT_RBUTTONDOWN', 'EVENT_RBUTTONUP']
>>>
```

마우스 콜백 함수를 생성하는 것은 어디서나 같은 형식을 취합니다. 함수가 하는 것만 다릅니다. 따라서 마우스 콜백 함수는 한 가지 작업을 수행합니다. 두 번 클릭하면 원이 그려집니다. 아래 코드를 참조하십시오. 코드 내용을 보면서 확인할 수 있습니다.

```python
import cv2
import numpy as np


# 마우스 콜백 함수
def draw_circle(event, x, y, flags, param):
    if event == cv2.EVENT_LBUTTONDBLCLK:
        cv2.circle(img, (x, y), 100, (255, 0, 0), -1)


# 검은색 바탕을 생성합니다. 마우스 콜백함수를 바인드 합니다.
img = np.zeros((512, 512, 3), np.uint8)
cv2.namedWindow('image')
cv2.setMouseCallback('image', draw_circle)

while True:
    cv2.imshow('image', img)
    if cv2.waitKey(20) & 0xFF == 27:
        break
cv2.destroyAllWindows()
```

더블클릭을 하면 원이 그려지는 것을 확인할 수 있습니다.

![](https://goo.gl/9yn7mB)

> 종료를 하려면 `ESC` 키를 눌러야 합니다.


## 고급 데모

이제 우리는 훨씬 더 나은 응용 프로그램으로 갑니다. 이 그림에서는 Paint 응용 프로그램에서와 같이 마우스를 끌어서 선택한 사각형에 따라 직사각형 또는 원을 그립니다. 그래서 우리의 마우스 콜백 함수는 두 부분을 가지고 있습니다. 하나는 직사각형을 그리고 다른 하나는 원을 그립니다. 이 특정 예제는 객체 추적, 이미지 분할 등과 같은 일부 대화식 응용 프로그램을 작성하고 이해하는데 실제로 도움이 될 것입니다.

```python
import cv2
import numpy as np

drawing = False  # True 이면 마우스가 눌린 상태입니다.
mode = True  # True이면 사각형을 그립니다. 'm'을 누르면 곡선으로 변경(토글)됩니다 
ix, iy = -1, -1


# 마우스 콜백 함수
def draw_circle(event, x, y, flags, param):
    global ix, iy, drawing, mode

    if event == cv2.EVENT_LBUTTONDOWN:
        drawing = True
        ix, iy = x, y

    elif event == cv2.EVENT_MOUSEMOVE:
        if drawing == True:
            if mode == True:
                cv2.rectangle(img, (ix, iy), (x, y), (0, 255, 0), -1)
            else:
                cv2.circle(img, (x, y), 5, (0, 0, 255), -1)

    elif event == cv2.EVENT_LBUTTONUP:
        drawing = False
        if mode == True:
            cv2.rectangle(img, (ix, iy), (x, y), (0, 255, 0), -1)
        else:
            cv2.circle(img, (x, y), 5, (0, 0, 255), -1)

```

다음으로이 마우스 콜백 함수를 OpenCV 윈도우에 바인딩해야합니다. 메인 루프에서 사각형과 원 사이를 토글(toggle)하기 위해 키 `'m'`에 대한 키보드 바인딩을 설정해야 합니다.

```python
img = np.zeros((512,512,3), np.uint8)
cv2.namedWindow('image')
cv2.setMouseCallback('image',draw_circle)

while(1):
    cv2.imshow('image',img)
    k = cv2.waitKey(1) & 0xFF
    if k == ord('m'):
        mode = not mode
    elif k == 27:
        break

cv2.destroyAllWindows()
```

![](https://goo.gl/wEHYWV)

기본은 사각형을 그리게 되고, `m`키를 누르면 원을 그리게 됩니다.


## 추가 리소스

## 연습 문제

마지막 예제에서는 채워진 직사각형을 그렸습니다. 코드를 수정하여 채워지지 않은 사각형을 그립니다.

```python
cv2.rectangle(img, (ix, iy), (x, y), (0, 255, 0), True)
```

마지막 파라미터 값을 `True`로 변경하여 채워지지 않은 사각형으로 변경합니다. 

```python
import cv2
import numpy as np

drawing = False  # True 이면 마우스가 눌린 상태입니다.
mode = True  # True이면 사각형을 그립니다. 'm'을 누르면 곡선으로 변경(토글)됩니다
ix, iy = -1, -1


# 마우스 콜백 함수
def draw_circle(event, x, y, flags, param):
    global ix, iy, drawing, mode

    if event == cv2.EVENT_LBUTTONDOWN:
        drawing = True
        ix, iy = x, y

    elif event == cv2.EVENT_LBUTTONUP:
        drawing = False
        if mode == True:
            cv2.rectangle(img, (ix, iy), (x, y), (0, 255, 0), True)
        else:
            cv2.circle(img, (x, y), 5, (0, 0, 255), -1)


img = np.zeros((512, 512, 3), np.uint8)
cv2.namedWindow('image')
cv2.setMouseCallback('image', draw_circle)

while (1):
    cv2.imshow('image', img)
    k = cv2.waitKey(1) & 0xFF
    if k == ord('m'):
        mode = not mode
    elif k == 27:
        break

cv2.destroyAllWindows()
```


![](https://goo.gl/BVJVKL)




## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_gui/py_mouse_handling/py_mouse_handling.html


<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>






















