---
title: django-celery 장고에 스케쥴 설정
date: 2017-01-13 19:13:45
tags: [django, python, celery, schedule]
categories:
- Programming
- Python
---

# django-celery 장고에 스케쥴 설정

## django-celery 설치

```python
pip install django-celery
```

proj/proj/settings.py 수정

```python
INSTALLED_APPS = [
    ...
    'djcelery',
    'kombu.transport.django',
]
```

## 데이터베이스 테이블 생성

```python
python manage.py makemigrations
python manage.py migrate
```

## proj/proj/settings.py 수정

```python
import djcelery

djcelery.setup_loader()
BROKER_URL = 'django://'
CELERYBEAT_SCHEDULER = 'djcelery.schedulers.DatabaseScheduler'
```

## proj/app/tasks.py 파일 생성

```python
import datetime
import celery

@celery.decorators.periodic_task(run_every=datetime.timedelta(minutes=1))
def myfunc():
    print('periodic_task')
```

```python
from celery.task import periodic_task
from celery.schedules import crontab


@periodic_task(run_every=crontab(hour="*", minute="0", day_of_week="*"), ignore_result=True)
def my_test():
    print('periodic_task', 'my_test')
```

## 실행

```python
python manage.py celery worker --loglevel=DEBUG -E -B -c 1
```

## admin 페이지 접속

```python
http://127.0.0.1:9000/admin/djcelery/
```

## crontab 설정법

- http://docs.celeryproject.org/en/latest/userguide/periodic-tasks.html
