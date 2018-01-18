---
title: '책. Two Scoops of Django (모범 사례로 배우는 Django(장고) 테크닉)'
date: 2017-04-03 19:13:45
tags: [python, django, book]
categories:
- Programming
- Python
---


# 책. Two Scoops of Django (모범 사례로 배우는 Django(장고) 테크닉)

[![](http://image.yes24.com/goods/30355215/200x293)](http://blog.yes24.com/lib/adon/View.aspx?blogid=9654534&goodsno=30355215&idx=24272&ADON_TYPE=B&regs=b)

[모범 사례로 배우는 Django(장고) 테크닉](http://blog.yes24.com/lib/adon/View.aspx?blogid=9654534&goodsno=30355215&idx=24272&ADON_TYPE=B&regs=b)

(대니얼 로이 그린펠드 · 오드리 로이 그릴펠드 지음 / 김승진 옮김 / 프로그래밍인사이트)

## settings와 requirements 파일

우리가 생각하는 최선의 장고 설정 방법
- 버전 컨트롤 시스템으로 모든 설정 파일을 관리해야 한다.
- 반복되는 설정들을 없애야 한다.
- 암호나 비밀 키 등은 안전하게 보관해야 한다.

http://2scoops.co/the-best-and-worst-of-django

settings/local.py 세팅 파일을 이용하여 장고/파이썬 셸 시작

```shell
python manage.py shell --settings=twoscoops.settings.local
python manage.py runserver --settings=twoscoops.settings.local
```

# 장고에서 모델 이용하기

장고는 세 가지 모델 상속 방법을 제공한다.
- 추상화 기초 클래스(abstract base class)
- 멀티테이블 상속(multitable inheritance)
- 프락시 모델(proxy model)

> 멀티테이블 상속은 피하자
> 접합 상속(concrete inheritance)이라고도 부르는 멀티테이블 상속은 가능한 이용하지 않기를 권한다.

모델에서 `null=True` 와 `blank=True` 옵션을 이용할 때는 애매한 부분을 주의하기 바란다.

# 쿼리와 데이터베이스 레이어

## 쿼리 표현식

```python
from django.db.models import F
from models.customers import Customer
customers = Customer.objects.filter(scoops_ordered__gt=F('store_visits'))
```

내부적으로 장고는 다음과 같은 코드를 실행한다.
```sql
SELECT * from customers_customer where scoops_ordered > store_visits
```

쿼리 표현식이 프로젝트의 안전성과 성능을 대폭 향상시켜 줄 것이다.

https://docs.djangoproject.com/en/1.8/ref/models/expressions

## 데이터베이스 함수들

https://docs.djangoproject.com/en/1.8/ref/models/database-functions

## 필요에 따라 인덱스를 이용하자.

- 인덱스가 빈번하게(모든 쿼리의 10~25% 사이에서서 이용될 때
- 실제 데이터 또는 실제와 비슷한 데이터가 존재해서 인덱싱 결과에 대한 분석이 가능할 때
- 인덱싱을 통해 성능이 향상되는지 테스트할 수 있을 때

PostgreSQL을 이용할 때 `pg_stat_activity`는 실제로 어떤 인덱스들이 이용되는지 알려준다.

## 명시적인 트랜잭션 선언

- 데이터베이스에 변경이 생기지 않는 데이터베이스 작업은 트랜잭션으로 처리하지 않는다.
- 데이터베이스에 변경이 생기는 데이터베이스 작업은 반드시 트랜잭션으로 처리한다.
- 데이터베이스 읽기 작업을 수반하는 데이터베이스 변경 작업 또는 데이터베이스 성능에 관련된 특별한 경우에는 앞의 두 가이드라인을 모두 고려한다.


목적 | ORM 매서드 | 트랜잭션을 이용할 것인가
---------|----------|---------
 데이터 생성 | .create(), .bulk_create(), .get_or_create() | ✔️
 데이터 가져오기 | .get(), .filter(), .count(), .iterate(), .exists(), .exclude() |
 데이터 수정하기 | .update() | ✔️
 데이터 지우기 | .delete() | ✔️

## 장고 ORM 트랜잭션 관련 자료

- https://docs.djangoproject.com/en/1.8/topics/db/transactions
- https://realpython.com/blog/python/transaction-management-with-django-1-6

# 함수 기반 뷰와 클래스 기반 뷰

장고 1.8은

- 함수 기반 뷰(function-based view, FBV)
- 클래스 기반 뷰(class-based view, CBV)

둘 다 지원한다.

## 뷰의 기본 형태들

```python
# simplest_views.py
from django.http import HttpResponse
from django.views.generic import View

# 함수 기반 뷰의 기본 형태
def simplest_view(request):
    # 비즈니스 로직이 여기에 위치한다.
    return HttpResponse("FBV")

# 클래스 기반 뷰의 기본 형태
class SimpleView(View):
    def get(self, request, *args, **kargs):
        # 비즈니스 로직이 여기에 위치한다.
    return HttpResponse("CBV")
```
왜 이 기본 형태가 중요한가?

- 종종 우리에게 한 기능만  따로 떼어 놓은 관점이 필요할 때가 있다.
- 가장 단순한 형태로 된 기본 장고의 뷰를 이해했다는 것은 장고 뷰의 역할을 명확히 이해했다는 것이다.
- 장고의 함수 기반 뷰는 HTTP 메서드에 중립적이지만, 클래스 기반 뷰의 경우 HTTP 메서드의 선언이 필요하다는 것을 설명해 준다.

# 함수 기반 뷰의 모범적인 이용

## 함수 기반 뷰의 장점

- 뷰 코드는 작을수록 좋다.
- 뷰에서 절대 코드를 반복해서 사용하지 말자
- 뷰는 프레젠테이션(presentation) 로직을 처리해야 한다. 비즈니스 로직은 가능한 한 모델 로직에 적용시키고 만약 해야 한다면 폼 안에 내재시켜야 한다.
- 뷰를 가능한 단순하게 유지하자.
- 403, 404, 500을 처리하는 커스텀 코드를 쓰는 데 이용하라.
- 복잡하게 중첩된 if 블록 구문을 피하자

## 데코레이터에 대한 더 많은 자료들

- http://2scoops.co/decorators-explained
- http://2scoops.co/decorators-functional-python
- http://2scoops.co/decorator-cheatsheet

# 클래스 기반 뷰의 모범적인 이용

## 클래스 기반 뷰를 이용할 때의 가이드라인

- 뷰 코드의 양은 적으면 적을수록 좋다.
- 뷰 안에서 같은 코드를 반복적으로 이용하지 말자.
- 뷰는 프레젠테이션 로직에서 관리하도록 하자. 비즈니스 로직은 모델에서 처리하자. 매우 특별한 경우에는 폼에서 처리하자.
- 뷰는 간단 명료해야 한다.
- `403, 404, 500 에러 핸들링`에 클래스 기반 뷰는 이용하지 않는다. 대신 `함수 기반 뷰를 이용`하자.
- `믹스인`은 간단 명료해야 한다.

## 어떤 장고 제네릭 클래스 기반 뷰를 어떤 태스크에 이용할 것인가?

# 장고 폼의 기초

# 폼 패턴들

유용한 폼 관련 패키지들

- django-floppyforms: 장고 폼을 HTML5로 렌더링해 준다.
- django-crispy-forms: 폼 레이아웃의 고급 기능들. 폼을 트위터의 부트스트랩 폼 엘리먼트와 스타일로 보여준다. django-floppyforms와 함께 쓰기 좋아서 빈번하게 함께 쓰인다.

# REST API 구현하기

개발자들은 AJAX와 모바일 앱을 지원해야 하며 JSON, YAML, XML을 포함한 여러 형식을 지원해야 하는 일이 큰 과제가 되었다. REST(representational state transfer) API는 다양한 환경과 용도에 맞는 데이터를 제공하는 디자인을 정의하고 있다.

## 패키지 API를 제작하기 위한 패키지들

- `django-rest-framework` 는 장고의 기본 클래스 기반 뷰를 바탕으로 브라우징이 가능한 편리한 API 기능 등을 제공한다.
- `django-tastypie`는 자체적으로 구현된 클래스 기반 뷰 시스템을 제공하는 안정된 도구다.
- 매우 단순하고 빠르게 REST API를 제작하고 싶다면 django-braces(클래스 기반 뷰)와 django-jsonview(함수 기반 뷰)가 좋은 대안이 된다. 다만 이 도구들은 API를 제작하는데만 중점을 두고 있지는 않기 때문에 HTTP 메서드의 모든 기능을 다 이용한다거나 복잡한 디자인을 구현해야 할 때 문제가 생길 것이다.

## 비즈니스 계획으로서의 접속 제한

- 개발자(Developer) 단계는 무료로 서비스되지만 시간당 10 API 요청만 가능
- 한 숟갈(One Scoop) 24달러, 분당 25요청
- 두 숟갈(Two Scoops) 79달러, 분당 50요청
- 기업(Corporate) 5000달러, 분당 200요청

## 장고의 어드민 문서 생성기

```shell
pip install docutils
```
```python
INSTALLED_APPS = [
    ...
    'django.contrib.admindocs',
    ...
]
```
```python
urlpatterns = [
    url(r'^admin/doc/', include('django.contrib.admindocs.urls')),
    ...
]
```

이후 `/admin/doc`으로 가서 문서들을 살펴볼 수 있다.

# 장고의 비법 소스: 서드 파트 패키지들

장고의 진정한 강력함은 바로 빠르게 성장하고 있는 오픈 소스 커뮤니티에서 제공하는 파이썬 패키지와 서드 파티 장고 패키지들이다.

# 패키지들

## 코어

- Django (htts://djangoproject.com)
데드라인이 있는 완벽주의자를 위한 웹 프레임워크

- django-debug-toolbar (http://django-debug-toolbar.readthedocs.org)
장고 디버깅을 위한 디스플레이 패널

- django-model-utils (https://pypi.python.org/pypi/django-model-utils)
시계열 모델(time stamped model)을 포함한 유용한 모델 유틸리티

- ipdb (https://pypi.python.org/pypi/ipdb)
IPython을 이용할 수 있는 pdb

- Pillow (https://pypi.python.org/pypi/Pillow)
파이썬 이미지 라이브러리를 위한 편리한 설치 프로그램

- pip (https://www.pip-installer.org)
파이썬 패키지 설치 프로그램. 파이썬 3.4 이상부터 파이썬에 내장되어 있다.

- Sphinx (http://sphinx-doc.org)
파이썬 프로젝트 문서화 도구

- virtualenv (http://virtualenv.org)
파이썬 가상 환경 세팅 도구

- virtualenvwrapper (http://www.doughellmann.com/projects/virtualenvwrapper)
맥 OS X과 리눅스에서 virtualenv를 좀 더 편리하게 쓰기 위한 도구

- virtualenvwrapper-win (https://pypi.python.org/pypi/virtualenvwrapper-win)
윈도우에서 virtualenv를 좀 더 편리하게 쓰기 위한 도구

## 비동기

- celery (http://www.celeryproject.org)
분산 태스크 큐

- flower (https://pypi.python.org/pypi/flower)
셀러리 테스크 관리·모니터링 도구

- rq (https://pypi.python.org/pypi/rq)
가벼우면서 간단한 백그라운드 작업 생성과 프로세싱 라이브러리

- django-rq (https://pypi.python.org/pypi/django-rq)
RQ(Redis Queue) 와 장고의 통합을 제공하는 간단한 앱

- django-background-tasks (https://pypi.python.org/pypi/django-background-tasks)
데이터베이스 기반의 비동기 테스크 큐
