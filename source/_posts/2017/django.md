---
title: django 프로젝트 생성
date: 2017-01-17 19:13:45
tags: [python, django]
categories:
- Programming
- Python
---

# django 프로젝트 생성

## 프로젝트 생성
[Google App Engine 에서 Django 사용하기](http://i5on9i.blogspot.kr/2016/09/google-app-engine-django.html)

### 문자열 반환

URL 추가
```python
url(r'^view/', view.test),
```

함수 생성
```python
def test(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

### 템플릿 반환

URL 추가
```python
url(r'^index/', view.index),
```

함수 행성
```python
def index(request):
    msg = 'My Message'
    return render(request, 'index.html', {'message': msg})

```
/templates/index.html 파일 생성
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>{{message}}</h1>
</body>
</html>
```

### 참고
- http://pythonstudy.xyz/python/article/307-Django-%ED%85%9C%ED%94%8C%EB%A6%BF-Template
- http://i5on9i.blogspot.kr/2016/09/google-app-engine-django.html
