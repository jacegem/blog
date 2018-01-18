---
title: 장고의 설계 원칙
date: 2017-01-14 19:13:45
tags: [python]
categories:
- Programming
- Python
---

# 장고의 설계 원칙

- https://docs.djangoproject.com/en/1.7/misc/design-philosophies/
- https://docs.djangoproject.com/en/1.11/misc/design-philosophies/

## 일반 사항

- 약한 결합(Loose coupling)
- 경량 코드(Less code)
- 신속 개발(Quick development)
- 반복 방지(DRY, Don't Repeat Yourself)
- 암시보다는 명시적으로 표현(Explicit is better than implicit)
- 일관성(Consistency)

## 모델

- 암시보다는 명시적으로 표현(Explicit is better than implicit)
- 관련 도메인 로직을 모두 포함(Include all relevant domain logic)

## 데이터베이스 API

- SQL 효율성(SQL efficiency)
- 간결하고 강력한 문법(Terse, powerful syntax)
- 필요 시 쉽게 작성할 수 있는 SQL(Option to drop into raw SQL easily, when needed)

## URL 설계

- 약한 결합(Loose coupling)
- 제약없는 유연설(Infinite flexibility)
- 베스트 프랙티스 권장(Encourage best practices)
- 결정적인 URL(Definitive URLs)

## 템플릿 시스템

- 표현과 로직의 분리(Separate logic from presentation)
- 중복 배제(Discourage redundancy)
- HTML과 분리(Be decoupled from HTML)
- 템플릿 언어로 XML 금지(XML should not be used for template languages)
- 디자이너의 능력 가정(Assume designer competence)
- 여백을 확실하게 처리(Treat whitespace obviously)
- 프로그래밍 언어를 만들지 말자(Don't invent a programming language)
- 안정성과 보안(Safety and security)
- 확장성(Extensibility)

## 뷰

- 간단함(Simplicity)
- 요청 객체의 사용(Use request objects)
- 약한 결합(Loose coupling)
- GET, POST 간 차이(Differentiate between GET and POST)

## 캐시 시스템

- 경량 코드(Less code)
- 일관성(Consistency)
- 확장성(Extensibility)



## 링크

- http://doku.ml/open/%EC%9E%A5%EA%B3%A0%EC%9D%98_%EC%84%A4%EA%B3%84_%EC%9B%90%EC%B9%99
