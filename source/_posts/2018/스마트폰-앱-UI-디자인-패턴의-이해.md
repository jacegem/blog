---
title: "스마트폰 앱 UI 디자인 패턴의 이해"
date: 2018.05.21
tags: [app, ui, ux, design, pattern]
categories:
- Programming
---

# 스마트폰 앱 UI 디자인 패턴의 이해

## 디자인 패턴이란

프로그래밍 시 자주 반복되어 나타나는 문제점을 해결하고자, 과거의 다른 사람이 해결한 결과물을 재사용하기 좋은 형태로 활용한다는 의미로 쓰여졌으며, 특정한 상황에서 구조적인 문제를 해결할 수 있는 방식을 설명

- 어떠한 목적/기능을 위한 레이아웃을 디자인 함에 있어
- 콘텐츠, 버튼, 컴포넌트 등의 배치에 대한 디자인 패턴을 의미

## 스마트폰 앱 UI 디자인 패턴

1. 목적/기능을 위한 레이아웃
2. 요소의 배치 = UI 디자인 패턴

목적/기능에 부합한 용어를 정의하고, 해당 UI 디자인 패턴을 참조
동일 목적의 '타임라인'이더라도, 의도/목적에 따라 달라진다.

## UI 디자인 패턴 참조 사이트

구글에서 `UI DESIGN PATTERN`으로 검색

- http://inspired-ui.com
- http://mobilepatterns.com
- http://mobile-patterns.com
- http://www.pttrns.com
- http://androidpttrns.com

공식 개발자 웹사이트

- developer.android.com
- developer.apple.com

## 모바일 앱 UI 디자인 패턴 실습 설명

바로 PPT 문서로 정리하는 것보다는, 스케치로 구체화하는 것을 권장한다.
정밀하게 표현하는 것보다는, 와이어프레임 방시으로 스케치하면 쉽고 빠르다.
스케치 템플릿을 활용하여 `UI 플로우(flow)`라 불리우는 화면 간의 연결(네비게이션) 표현을 할 수 있다.

필요할 경우, 동일한 목적의 페이지를 여러 개 디자인하고 비교/평가 할 수 있다.

# 스마트폰 앱 UI 스토리보드를 활용한 레이아웃 디자인

## 스마트폰 앱 UI 스토리보드의 이해

모든 앱의 공통 구성 페이지: 앱 아이콘 > 런치 이미지 > 메인 페이지의 전개

UI 스토리보드 핵심 구성 페이지 먼저 기획/디자인 한다.
App ICON, Launch Image, Main Menu, Main Content, Settings...

앱의 전개 방식도 카테고리별 공통적인 디자인 패턴이 존재한다.

## UI 스토리보드 작업 방법

> 어느 페이지가 사용자에게 더 중요한 정보를 담고 있는가?

연필을 사용하여 작업하고, 컬러 펜으로 네비게이션 등을 표시한다. 표현은 와이어프레임으로, 간결하게 하는 것이 좋다.

필요한 경우, 팝업이나 키보드가 올라오는 페이지의 경우 포스트잇을 활용한다.

## UI 스토리보드를 활용한 리서치

UI 스토리보드를 활용하여 스케치로 다른 앱의 구조를 그린다면, 굉장히 짧은 시간안에 앱의 전체적인 구조, 네비게이션, 목적 지향적 UI 구조를 파악할 수 있다. 

기본 구성 페이지 외에도 인 앱 결제의 프로세스, 로그인 페이지의 구성 등 다양한 부분에 대한 전체 구조적 관점에서 분석이 가능하다. 

페이지 구성시 로그인, 회원 가입, 결제 프로세스 등과 같이 트렌드 및 기술에 따라 사용자가 민감하게 반응하는 디자인 패턴 부분은, 해당 문제 부분이 무엇인지를 먼저 정의한 다음, 동일 카테고리의 앱이 아니더라도 해당 문제에 대한 답을 가지고 있는 앱의 특정 부분만 분석 비교함으로써 쉽게 문제에 대한 해답을 찾아낼 수 있다.

- 예시 질문: 커피 주문 앱의 경우, 결제 단계 및 프로세스가 잘 기획이 안될 때는?
- 해답: 결제 프로세스를 전무능로 하고 있는 피자 주문, 배달 주문 앱 등의 해당 프로세스 부분만 UI 스토리보드로 스케치하여 분석/비교 한다.

## UI 스토리보드, 스마트폰 템플릿을 활용한 실습 설명

### 1. UI 스토리보드를 먼저 작업한다.

전체 구조를 파악할 수 있고, 작은 사이즈로 작업하게 되므로 디테일 보다는 주요 콘텐츠/기능에만 관심을 둘 수 있어서 작업 속도를 빠르게 할 수 있는 장점이 있다.

### 2. 반드시 관련 정보를 기입한다.

앱의 이름, 작성 날짜, 작성자, 해당 버전, 대상 디바이스의 종류 등 앱과 관련된 필수 사항들을 기록해 두어야 추구 관리가 쉽다.

### 3. 네비게이션, 그룹핑, 제스처-모션 등의 요소도 반드시 기입하라.

컬러 펜을 사용하여 네비게이션을 통한 페이지 이동, 제스처에 따른 모션의 방향 등의 요소는 반드시 기입한다.

### 4. 그룹핑, 페이지 네이밍도 기록하라.

각 페이지별로 어떠한 역할 혹은 디자인 패턴 명칭으로 불리울 지를 감안하고 네이밍하고, 여러 페이지로 구성될 경우에는 그룹핑+네이밍을 표시한다.

### 5. 스케치에 문제가 생길 경우, 다시 그리지 말고 포스트잇을 활용하라.

실수로 잘못 그리거나, 해당 페이지의 내용이 바뀌어 진다면 포스트있을 붙이면 된다.

## 메인 플로우 UI 디자인 스케치

1. UI 스토리보드를 통해 러프하게 그려진 레이아웃을, 큰 사이즈로 그리면서 보다 구체화
2. 큰 사이즈 작업 시에는 실제 터치 영역 사이즈, 폰트의 크기, 아이콘의 구체적 형태 등에 대해서도 확정
3. 네비게이션-인터랙션 디자인 부분을 커럴 펜으로 표시
4. 구체적인 사용자 유즈 케이스 혹은 시나리오를 작성하는 것도 큰 도움
5. 키보드, 팝업 같은 요소는 1개의 폐이지로 별도 구성하지 않는다. 포스트 있을 활용하여 작업

# UI 스케치를 활용한 디지털 프로토타이핑 테스트

## 디지털 프로토타이핑의 이해

실제 스마트폰에서 앱을 사용하는 것처럼, 스마트폰 앱 혹은 관련 서비스를 통해 스크린에서 터치 제스처, 화면의 이동, 터치에 따른 반응 등의 UI 조작 및 인터랙션을 테스트

디지털 프로토타이핑을 하는 경우, inVision, FluidUI, Axure, Balsamiq, FarmerJS 등 수 많은 서비스가 존재한다.

![](https://goo.gl/mex94a)

## UI 사용자 테스트 진행 방법

UI 사용자 테스트는 사용자 테스트를 통해 실제 기획/디자인 의도와 사용자의 실제 사용 방식을 확인하고 평가하는 과정

### 씽크어라우드 (Think Aloud)

사용자가 구체적인 행동을 하는 이유를 말로 표현하게 하여 행동의 이유/근거에 대해 알 수 있는 방법

### 관찰

실제 사용자의 행동을 관찰하여 인사이트, 아이디어를 얻는 방법이다. 이를 통해 사용자 중심의 구성이 가능하다.

## 출처

- [SK 동반성장아카데미](https://skwinwin.com/classroom/class_main.asp?pEduYY=2018&pEduCD=2968&pEduDeg=3&pPrevPath=)
- [Origami Studio — Design Prototyping](https://origami.design/)
- [STUDIO | The next generation design tool](https://app.studio.design)