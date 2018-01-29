---
title: FastCampus 시스템 트레이딩
date: 2017-02-01
tags: [system, trading]
categories:
- Conference
- ETC
---

# FastCampus 시스템 트레이딩

- http://www.fastcampus.co.kr/fin_camp_algot/?utm_source=google&utm_medium=cpc&utm_campaign=fin_camp_algot_2&gclid=CPXH3PftwNMCFQJ9vQodmtEDnw
- http://www.fastcampus.co.kr/fin_camp_pythonstarter/#!
- https://www.codecademy.com/ko/tracks/python-ko


### 1 주차

알고리즘 트레이딩의 개요와 시스템 개발을 위한 필수 라이브러리와 환경구축에 대해 배웁니다.

[Algorithmic Trading]
- 알고리즘 트레이딩의 개요
- 알고리즘 트레이딩의 장단점 및 시스템 구성

금융데이터 분석을 빠른 시간 내에 처리할 수 있는 Pandas 라이브러리에 대해 배웁니다. 아울러 그래프를 쉽게 그려주는 Matplotlib의 사용법을 익힙니다.

[Pandas & Matplotlib]
- Pandas 소개, Pandas Data 구조, Pandas 핵심 기능
- Matplotlib 그래프 그리기

[파이썬 개발환경구축]
- 알고리즘 트레이딩 시스템 개발을 위한 환경 설명
- 개발환경 구축

### 2 주차	

알고리즘 트레이딩 시스템과 HTS 연동 및 주가 데이터 다운로드
[HTS API 연동 프로그램 개요]
- HTS 연동 환경 구축
- 키움증권 API를 이용한 HTS 연동
- MySQL을 이용한 주가 데이터 저장 및 활용방법 설명

[실습]
- 코스닥, 코스피 종목 코드 다운로드
- 키움증권 API를 이용한 일단위, 분단위 데이터 다운로드 프로그램 개발
- 대신증권 API를 이용한 일단위, 분단위 데이터 다운로드 프로그램 개발
- MySQL을 이용한 주가데이터 액세스 프로그램 개발

### 3 주차	

확률통계를 이용한 종목분석

평균, 분산, 표준편차 등 통계와 확률의 기본 개념을 배운 후, Pandas와 Numpy를 이용해 데이터 분석의 시작인 기초 통계, 상관 관계 등을 파이썬을 이용해 직접 구해봅니다.

[Basic Statistics and Probability]
- 통계 기본 개념
- 기초 통계
- 확률 기본 개념

[실습]
- 통계를 이용한 종목 특성 분석
- Skewness, Kurtosis 등의 통계치를 이용한 포트폴리오 선정

### 4 주차

알고리즘 트레이딩의 중요한 수학적 토대인 시계열 분석을 배웁니다. Random Walk 이론과 Stationarity 등의 시계열 데이터 특성과 ARMA, VAR, GARCH 등을 소개합니다.

[Time Series Analysis]
- Time Series Data 특성
- 랜덤 과정
- 정상 시계열 과정
- ARMA, VAR, GARCH

[실습]
- ARMA을 이용한 투자 모델 개발
- GARCH를 이용한 투자 모델 개발

### 5 주차

앞서 배운 시계열 분석을 이용한 모델의 평가와 알고리즘 트레이딩에 적용 가능한 종목을 선정하는 방법을 배웁니다.

[Backtesting]
- Backtesting 개요
- BackTesting을 위한 개발 환경 구축
- 모델 성능 평가 방법
- ARMA, GARCH 모델 Backtesting 실습

[Portfolio 구성]
- Random Walk
- Stationarity Test
- 종목 선정

### 6 주차

알고리즘 트레이딩에 있어 기저가 되는 Mean Reversion 모델에 대해 알아보고, 과거 데이터를 이용해 거래에 따른 수익을 테스트해봅니다.
[Mean Reversion Model]
- Mean Reversion Model 개요
- Mean Reversion Model 구현
- Mean Reversion Model 유의사항
[실습]
- Mean Reversion Model 개발

### 7 주차

Mean Reversion의 발전된 모델인 Pairs Trading Model에 대해 설명하고, 이를 직접 구현해 Backtesting을 실시합니다.
[Pairs Trading Model]
- Pairs Trading Model 개요
- Cointegration, PCA 그리고 Johansen Test
- Pairs Trading Model 구현
[실습]
- Pairs Trading Model 개발
- ETF와 주식을 연동한 Pairs Trading Model 개발

### 8 주차

개발한 모델의 위험관리를 위한 이론적 배경과 활용가능한 모델들을 설명하고, 직접 구현합니다.
[Risk Management]
- GARCH를 이용한 Volatility Modelling
- VaR (Value at Risk)
- Risk Management를 위한 적절한 확률분포 선택
[실습]
- GARCH를 이용한 Volatility Model 개발
- VaR,CVaR 개발