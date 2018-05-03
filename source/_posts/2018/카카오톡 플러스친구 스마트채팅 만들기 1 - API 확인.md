---
title: "카카오톡 플러스친구 스마트채팅 만들기 1 - API 확인"
date: 2018.05.01
tags: [카카오톡, 플러스친구, 스마트채팅, API, firestore, heroku, github]
categories:
- Programming
- Python
---

# 카카오톡 플러스친구 스마트채팅 만들기 1 - API 확인

같은 그룹에 있는 사람들이 식당을 선택하고, 가장 많이 선택된 식당을 보여주는 봇을 만들어 보겠습니다.

## 플러스친구 생성

![](https://goo.gl/cfQp8t)

카카오톡 [플러스친구 관리자 센터](https://center-pf.kakao.com/login)를 통해서 봇을 만들 수 있습니다. 

플러스친구 관리자센터에 로그인을 합니다. 

`+새 플러스친구 만들기`를 누르고 기본적인 정보를 입력합니다. 

![](https://goo.gl/a1DsNu)

좌측 사이드 메뉴 중에서 `스마트채팅`을 선택합니다.

![](https://goo.gl/jBrJDb)

우측에 있는 `API형`을 선택합니다.

앱 URL을 등록합니다. 

![](https://goo.gl/yPvkky)

서버가 정상적으로 동작하면 API테스트 시에 결과를 확인할 수 있습니다. 

![](https://goo.gl/fQyag2)

## API Document

![](https://goo.gl/iarZjY)

API형 페이지에서 우측상단에 있는 `API Document` 버튼을 눌러 API를 확인할 수 있습니다. 

API 는 두가지를 구현해 주어야 합니다.

- /keyboard
- /message

## 서버/keyboard

```python
Method : GET
URL : http(s)://:your_server_url/keyboard
Content - Type : application/json; charset=utf-8
```

keyboard는 사용자가 처음 플러스친구와 채팅을 시작할 때 보여줄 화면을 json으로 반환하면 됩니다.

다음과 같은 json으로 반환합니다. 

```python
{
    "type": "buttons",
    "buttons": ["#점심 선택", "#결과 보기", '#설정']
}
```

위에서 API테스트에서 나온 결과가 바로 /keyboard 결과입니다. 화면으로 보면 아래와 같이 나오게 됩니다.

![](https://goo.gl/YffR7E)

type 을 buttons 로 지정하였기 때문에 `상담원과의 대화가 불가능한 프로필입니다.`라고 나오게 됩니다. 

## 서버/message

```python
Method : POST
URL : http(s)://:your_server_url/message
Content - Type : application/json; charset=utf-8
```

플러스친구와 채팅을 시작하고 사용자의 입력이 들어오는 순간부터는 /message로 전송됩니다. 이 때 3가지 파라미터를 같이 받을 수 있습니다. 

| 필드명   | 타입   | 필수여부 | 설명                                                 |
| -------- | ------ | -------- | ---------------------------------------------------- |
| user_key | String | Required | 메시지를 발송한 유저 식별 키                         |
| type     | String | Required | text, photo                                          |
| content  | String | Required | 자동응답 명령어의 메시지 텍스트 혹은 미디어 파일 uri |

/message 에서는 messages와 keyboard를 반환할 수 있습니다. 

```python
{
  "message": {
    "text": "귀하의 차량이 성공적으로 등록되었습니다. 축하합니다!",
    "photo": {
      "url": "https://photo.src",
      "width": 640,
      "height": 480
    },
    "message_button": {
      "label": "주유 쿠폰받기",
      "url": "https://coupon/url"
    }
  },
  "keyboard": {
    "type": "buttons",
    "buttons": [
      "처음으로",
      "다시 등록하기",
      "취소하기"
    ]
  }
}
```

내용을 확인하면 아래 같습니다.

- message - text : 대화에 입력할 내용
- meesage - photo - url : 사진 주소
- meesage - photo - width : 사진 넓이
- meesage - photo - height : 사진 높이
- message - message_button - label : 버튼 라벨
- message - message_button - url : 버튼 링크
- keyboard - type : 키보드 타입 (버튼 또는 텍스트)
- keyboard - buttons : 키보드에 보여줄 버튼들


## 사용자 관리

사용자 관리를 위한 API가 2개 더 있습니다.

- /friend
- /chat_room/:user_key

## 서버/friend

```python
Method : POST/DELETE
URL : http(s)://:your_server_url/friend
Content - Type : application/json; charset=utf-8
```

사용자가 채팅을 시작하면 전달되는 API 입니다.

## 서버/chat_room/:user_key

```python
Method : DELETE
URL : http(s)://:your_server_url/chat_room/:user_key
Content - Type : application/json; charset=utf-8
```

사용자가 채팅을 종료하면 전달되는 API 입니다.


<script src="https://gist.github.com/jacegem/fee3dae8e7a0c630dc612b76ad3d1911.js"></script>