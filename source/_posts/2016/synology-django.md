---
title: Synology 에서 django 실행하기
date: 2017-03-09
tags: [synology, django, python]
categories:
- Application
- NAS
---

# Synology 에서 django 실행하기

ssh 로 synology에 접속합니다.

`root` 로 접속합니다. 다른 계정으로 접속하였다면 `sudo su -`를 실행하여 root로 변경합니다.

python3.5를 사용하기 위해 위치를 확인해 봅니다.

```sh
root@syn:~# which python3.5
/usr/local/bin/python3.5
```

pip 를 설치하기 위한 파일을 다운로드 합니다.

```sh
wget https://bootstrap.pypa.io/get-pip.py
```

pip 를 설치합니다.

```python
python3.5 get-pip.py
```

python이 설치된 위치를 확인합니다.

```python
root@syn:/usr/local/bin# ls -al python3.5
lrwxrwxrwx 1 root root 47 Mar  1 22:32 python3.5 -> /volume2/@appstore/py3k/usr/local/bin/python3.5
```

디렉토리 이동을 합니다.

```sh
> cd /volume2/@appstore/py3k/usr/local/bin
```

django 를 설치합니다.

```sh
> pip install django

Collecting django
  Using cached Django-1.10.6-py2.py3-none-any.whl
Installing collected packages: django
Successfully installed django-1.10.6
```

버전 확인

```sh
> python -m django --version

1.10.6
```

django-admin 를 설치합니다.

```sh
> pip install django-admin
```


프로젝트를 생성합니다.

```sh
> django-admin startproject mysite
```

django를 실행합니다.

```sh
> cd mysite
> python manage.py runserver 0.0.0.0:9000
```

이후, django 사용법은 [이곳](https://docs.djangoproject.com/en/1.10/intro/tutorial01/)에서 확인할 수 있습니다.

### 출처

- https://forum.synology.com/enu/viewtopic.php?t=59837
- https://docs.djangoproject.com/en/1.10/intro/tutorial01/
