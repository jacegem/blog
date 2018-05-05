---
title: "카카오톡 플러스친구 스마트채팅 만들기 8 - heroku 배포"
date: 2018.05.08
tags: [카카오톡, 플러스친구, 스마트채팅, API, firestore, heroku, github]
categories:
- Programming
- Python
---

# 카카오톡 플러스친구 스마트채팅 만들기 8 - heroku 배포

# 파일 추가

![](https://goo.gl/9x1zA3)


heroku 에서 운영하기 위해 필요한 파일들을 추가합니다. 

- Procfile
- requirements.txt
- runtime.txt
- uwsgi.ini


## Procfile

```python
web: uwsgi uwsgi.ini
```

## requirements.txt

로컬에서 실행할 때와는 다른게 `uwsgi` 를 추가해야 합니다. 

```python
Flask
Flask_RESTful
firebase-admin
uwsgi
```

## runtime.txt

실행 환경을 지정합니다.

```python
python-3.6.4
```

## uwsgi.ini

실행 모듈을 지정합니다. kakotalk_lunch_bot.py 에 있는 `app`을 실행합니다. 

```python
[uwsgi]
http-socket = :$(PORT)
master = true
die-on-term = true
module = kakaotalk_lunch_bot:app
memory-report = true
```

# Github 가입 및 배포

4개의 파일을 추가한 후 Github에 배포합니다. Heroku에서 github에 연결한 후에 Deploy를 할 것입니다. 


# Heroku 가입 및 앱 추가

[Cloud Application Platform | Heroku](https://www.heroku.com/)에 배포해서 플러스친구를 운영하겠습니다.

가입을 하고 로그인을 합니다.

![Create New App](https://goo.gl/fwaKk9)

앱을 생성합니다. 이름은 `kakao-lunch-bot` 으로 지정하였습니다. 동일한 이름은 사용이 불가능하므로 다른 이름을 지정하여야 합니다.

![](https://goo.gl/s4qRcq)

## Deployment method

`대시보드 - Deploy` 에서 GitHub를 연결합니다. 물론 소스는 GitHub에 배포되어 있어야 합니다.

![](https://goo.gl/bRqc95)

## App connected to GitHub

![](https://goo.gl/YnbQWD)

heroku에 배포할 Github 저장소를 검색해서 선택합니다. 

## Manual deploy

Manual deploy 에서 `Deploy Branch` 버튼을 눌러 배포를 시작합니다.

![](https://goo.gl/hgj8Pm)

정상적으로 배포가 되면 녹색 체크박스로 나타나게 되고 아래에 `View`버튼이 생성됩니다. 

누르면 해당 주소로 이동해서 웹브라우저에서 확인할 수 있습니다.



# 플러스친구 스마트채팅 설정

카카오톡 [플러스친구 관리자 센터](https://center-pf.kakao.com/login)에 로그인을 합니다. 

생성한 플러스친구를 선택합니다. 

좌측 사이드 메뉴 중에서 `스마트채팅`을 선택합니다.

![](https://goo.gl/jBrJDb)

우측에 있는 `API형`에서 Edit를 선택합니다. 

`앱 URL`에 heroku에 배포한 URL을 등록합니다. 

![](https://goo.gl/yPvkky)

서버가 정상적으로 동작하면 API테스트 시에 결과를 확인할 수 있습니다. 

![](https://goo.gl/fQyag2)


## Heroku Pricing

![](https://goo.gl/uFPX4Y)

무료는 30분간 활성화가 되지 않으면 잠자기 모드로 진입합니다. 잠자기 모드일 경우에 웹 접속이 발생하면 그 때 시동이 되기 때문에 결과값을 받는데 오래 걸립니다. 

![](https://goo.gl/FwvV9V)



## Heroku CLI

[Heroku CLI | Heroku Dev Center](https://devcenter.heroku.com/articles/heroku-cli)에서 CLI를 설치합니다.

### 로그인

```shell
$ heroku login
- 계정 입력
- 비밀번호 입력
```

### 로그 확인

로그인 상태에서 진행합니다. -a 파라미터로 앱 이름을 지정합니다.

```shell
$ heroku logs -a [HEROKU 앱 이름]
```

### 로그 tail

지속적으로 로그를 확인합니다. 

```shell
$ heroku logs --tail -a [HEROKU 앱 이름]
```