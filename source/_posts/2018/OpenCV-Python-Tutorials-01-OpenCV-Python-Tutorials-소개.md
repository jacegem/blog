---
title: '[OpenCV-Python Tutorials 01] OpenCV-Python Tutorials 소개'
date: 2018-01-01 19:13:45
tags: [opencv, python, tutorial]
category:
- Programming
- Python
---


# [OpenCV-Python Tutorials 01] OpenCV-Python Tutorials 소개

모든 파일은 [Github](https://github.com/jacegem/OpenCV-Python-Tutorials)에서 확인 할 수 있습니다.

## OpenCV

![OpenCV](http://answers.opencv.org/upfiles/logo_2.png)

`OpenCV`는 1999년 Gary Bradsky에 의해 인텔에서 시작되었으며 첫번째 릴리스는 2000년에 나왔습니다. 

Vadim Pisarevsky는 Intel의 러시아 소프트웨어 OpenCV 팀을 관리하기 위해 Gary Bradsky에 합류했습니다.

2005년, OpenCV는 2005 DARPA 그랜드 챌린지에서 우승 한 차량인 Stanley에 사용되었습니다.

> 다르파 그랜드 챌린지(The DARPA Grand Challenge)는 미 국방성 최고위 연구기관인 국방고등기획국(Defense Advanced Research Projects Agency, DARPA, 다르파)이 후원하는 무인 자동차 경주 대회다.
> 

나중에 Gary Bradsky와 Vadim Pisarevsky가 프로젝트를 이끌면서 Willow Garage의 지원하에 적극적으로 발전했습니다.

현재 OpenCV는 컴퓨터 비전 및 기계 학습과 관련된 많은 알고리즘을 지원하며 날마다 확장되고 있습니다.

현재 OpenCV는 C++, Python, Java 등 다양한 프로그래밍 언어를 지원하며 Windows, Linux, OS X, Android, iOS 등 다양한 플랫폼에서 사용할 수 있습니다.

또한 `CUDA` 및 OpenCL을 기반으로 한 인터페이스도 고속 GPU 작동을 위해 활발히 개발 중입니다.

> CUDA ("Compute Unified Device Architecture", 쿠다)는 그래픽 처리 장치(GPU)에서 수행하는 (병렬 처리) 알고리즘을 C 프로그래밍 언어를 비롯한 산업 표준 언어를 사용하여 작성할 수 있도록 하는 GPGPU 기술이다.

![CUDA](https://goo.gl/L4mxES)

OpenCV-Python은 OpenCV의 Python API입니다. OpenCV C++ API와 Python 언어의 최상의 특성을 결합합니다.

## OpenCV-Python

![python](https://www.raspberrypi.org/documentation/usage/python/images/python-logo.png)

`Python`은 `Guido van Rossum`에 의해 시작된 범용 프로그래밍 언어입니다. 이 언어는 단순성과 코드 가독성으로 인해 단기간에 큰 인기를 끌게 되었습니다.

Python을 사용하면 프로그래머는 가독성을 떨어 뜨리지 않으면서 적은 수의 코드 행으로 자신의 아이디어를 표현할 수 있습니다.

C/C++와 같은 다른 언어와 비교할 때 Python은 속도가 느립니다. 그러나 파이썬의 또 다른 중요한 특징은 C/C++로 쉽게 확장 할 수 있다는 것입니다.

이 기능은 C/C++에서 계산 집약적인 코드를 작성하고 파이썬 래퍼를 작성하여 이러한 래퍼를 파이썬 모듈로 사용할 수 있게 도와줍니다.

이것은 두 가지 이점을 제공합니다. 
- 첫번째, 우리의 코드는 원래의 C/C++ 코드만큼 빠르며 (실제 C++ 코드가 백그라운드에서 작동하기 때문에) 
- 두번째, Python으로 코딩하는 것이 매우 쉽습니다.

이것은 OpenCV-Python이 작동하는 방식으로 원래의 C++ 구현을 둘러싼 파이썬 wrapper입니다.

그리고 Numpy의 지원으로 작업이 더 쉬워졌습니다. Numpy는 수치 연산을 위해 최적화된 라이브러리입니다. Numpy는 MATLAB 스타일의 구문을 제공합니다. 모든 OpenCV 배열 구조는 Numpy 배열로 변환됩니다.

Numpy에서 할 수 있는 어떤 작업도 OpenCV와 결합하여 당신의 무기고의 무기 개수를 늘릴 수 있습니다. 그 외에도 Numpy를 지원하는 SciPy, Matplotlib과 같은 여러 라이브러리와 함께 사용할 수 있습니다.

따라서 OpenCV-Python은 컴퓨터 비전 문제의 신속한 프로토타이핑을 위한 적절한 도구입니다.

> 프로토타입은 '정보시스템의 미완성 버전 또는 중요한 기능들이 포함되어 있는 시스템의 초기모델'이다.

## OpenCV-Python Tutorials

OpenCV는 OpenCV-Python에서 사용할 수 있는 다양한 기능을 안내하는 새로운 자습서 세트를 소개합니다. 이 가이드는 주로 OpenCV 3.x 버전에 초점을 맞추고 있습니다. (대부분의 자습서는 OpenCV 2.x에서도 작동합니다.)

Python과 Numpy에 대한 사전 지식은 안내서에서 다루지 않기 때문에 시작하기 전에 필요합니다. 특히, OpenCV-Python에서 최적화된 코드를 작성하기 위해서 Numpy에 대한 좋은 지식이 필요합니다.

이 자습서는 Alexander Mordvintsev의 지도하에 2013년 Google Summer of Code 프로그램의 일환으로 Abid Rahman K가 시작했습니다.

## OpenCV Needs You !!!

OpenCV는 `오픈 소스 이니셔티브`이기 때문에 이 라이브러리에 기여할 수 있습니다. 그리고 이 튜토리얼에서도 마찬가지입니다.

> 오픈 소스 이니셔티브(Open Source Initiative, 줄여서 OSI)는 오픈 소스 소프트웨어 사용을 장려하기 위하여 만들어진 단체이다.

따라서 이 튜토리얼에서 실수를 발견하면 (작은 맞춤법 오류, 코드, 개념의 큰 오류 등 어떤 것이든) 자유롭게 수정하십시오.

오픈 소스 프로젝트에 기여하기 시작하는 사람들에게는 좋은 일이 될 것입니다. 

![Github](https://githubuniverse.com/assets/img/universe/logo-2017.png)

OpenCV를 [Github](https://github.com/opencv/opencv)에서 fork 하고 필요한 부분을 수정하고 OpenCV에 pull request 를 보내면 됩니다.

OpenCV 개발자는 pull request 요청을 확인하고 중요한 피드백을 주며 검토자 승인을 통과하면 OpenCV에 병합됩니다.

그러면 당신은 오픈 소스 기여자가 됩니다. 다른 튜토리얼, 문서 등의 경우에서도 마찬가지 입니다

OpenCV-Python에 새로운 모듈이 추가됨에 따라 이 튜토리얼은 확장되어야 합니다. 그래서 특정 알고리즘을 아는 사람들은 알고리즘의 기본 이론과 알고리즘의 기본 사용법을 보여주는 코드를 포함하는 튜토리얼을 작성하여 OpenCV에 제출할 수 있습니다.

우리는 함께 이 프로젝트를 성공적으로 만들 수 있다는 것을 잊지 마십시오!!!

## Contributors

다음은 OpenCV-Python에 자습서를 제출한 기고자 목록입니다.

- Alexander Mordvintsev (GSoC-2013 mentor)
- Abid Rahman K. (GSoC-2013 intern)

## 설치

![](https://www.continuum.io/sites/all/themes/continuum/assets/images/logos/logo-horizontal-large.svg)

[Anaconda](https://www.continuum.io/downloads)를 설치합니다.
[파이썬 라이브러리 모음 사이트](http://www.lfd.uci.edu/~gohlke/pythonlibs/)에서 opencv를 찾아 설치합니다. 

```python
> D:\8.Download>pip install "opencv_python-3.2.0+contrib-cp36-cp36m-win_amd64.whl"

Processing d:\8.download\opencv_python-3.2.0+contrib-cp36-cp36m-win_amd64.whl
Installing collected packages: opencv-python
Successfully installed opencv-python-3.2.0+contrib
```

## Additional Resources

- A Quick guide to Python - [A Byte of Python](https://python.swaroopch.com/)
- [Basic Numpy Tutorials](http://scipy.github.io/old-wiki/pages/Tentative_NumPy_Tutorial)
- [Numpy Examples List](http://scipy.github.io/old-wiki/pages/Numpy_Example_List)
- [OpenCV Documentation](http://docs.opencv.org/)
- [OpenCV Forum](http://answers.opencv.org/questions/)


## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_setup/py_intro/py_intro.html#intro
- https://blog.naver.com/PostView.nhn?blogId=samsjang&logNo=220498694383&categoryNo=66&parentCategoryNo=0&viewDate=&currentPage=6&postListTopCurrentPage=1&from=postList&userTopListOpen=true&userTopListCount=10&userTopListManageOpen=false&userTopListCurrentPage=6
- https://ko.wikipedia.org

<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>