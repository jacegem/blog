---
title: "카카오톡 플러스친구 스마트채팅 만들기 3 - 데이터 베이스(firestore)"
date: 2018.05.03
tags: [카카오톡, 플러스친구, 스마트채팅, API, firestore, heroku, github, python, flask]
categories:
- Programming
- Python
---

# 카카오톡 플러스친구 스마트채팅 만들기 3 - 데이터 베이스(firestore)

데이터 베이스로 `firestore`를 사용하였습니다.

크게 3개의 collection으로 구성합니다. 

![firestore collection](https://goo.gl/Be2xjb)

- group : 그룹을 관리합니다.
- select :  사용자가 선택한 데이터를 관리합니다.
- user : 사용자를 관리합니다.

## group

![group collection](https://goo.gl/a8TiWv)

현재 그룹에는 `연구소` 그룹만 있습니다. 상황에 따라 추가하거나 삭제할 수 있습니다.
그룹 document는 식당정보를 object로 가지고 있습니다. 이 object를 수정하여 식당정보를 추가하거나 삭제할 수 있습니다.

## select

![select collection](https://goo.gl/b9fDJp)

선택한 데이터 역시 그룹을 기반으로 관리됩니다. 그룹안에 history, today 컬렉션을 생성하여 사용자가 선택한 데이터를 날짜별로 관리합니다. 

> user_key 부분은 모자이크 처리하였습니다.

![today collection](https://goo.gl/A9z5xz)

history 부분은 사용하지 않으므로 넘어가겠습니다. 

## user

![user collection](https://goo.gl/KhzHC5)

> user_key 부분은 모자이크 처리하였습니다.

user_key를 기반으로 document 를 생성합니다. 각 사용자의 그룹 정보와 상태정보를 관리합니다. 


## 점심 선택

사용자가 `#점심 선택` 버튼을 누른 경우를 처리합니다.

```python
# resources/message.py

class Message(Resource):
  def __init(self):
      ...
  def post(self):
      select = Select(self.args)
     
      # 버튼 입력을 처리합니다.
      if self.content == Const.BTN_SELECT_LUNCH:
          return select.show_restaurant_list()

      # 사용자 입력을 처리합니다. 

      # default
      return Util.show_start_menu()
```

데이터베이스에 접근할 수 있도록 초기화를 합니다.

Cloud Platform 콘솔에서 [IAM 및 관리자 > 서비스 계정 | Google Cloud Platform](https://console.cloud.google.com/iam-admin/serviceaccounts?hl=ko)으로 이동합니다. 새 비공개 키를 생성하고 JSON 파일을 저장합니다. 그런 다음 이 파일을 사용하여 SDK를 초기화합니다.

다운받은 `serviceAccount.json` 파일을 같은 경로에 넣어준 상태입니다.

```python
# conf/firebaseInit.py

import os
import firebase_admin
from firebase_admin import credentials
from firebase_admin import firestore

FILE_DIR = os.path.dirname(os.path.abspath(__file__))
CONFIG_PATH = os.path.join(FILE_DIR, 'serviceAccount.json')

# Use a service account
cred = credentials.Certificate(CONFIG_PATH)
firebase_admin.initialize_app(cred)

fs = firestore.client()
```

추가적으로 필요한 상수들을 정의합니다.

```python
class Const:
  COL_GROUP = 'group'
  COL_USER = 'user'
  COL_SELECT = 'select'
  COL_TODAY = 'today'

  FIELD_TITLE = 'title'
  FIELD_GROUP = 'group'
  FIELD_STATE = 'state'
  FIELD_RESTAURANT = 'restaurant'
  FIELD_ADD_RESTAURANT_TITLE = 'add_restaurant_title'
  FIELD_ADD_RESTAURANT_DESC = 'add_restaurant_desc'
  FIELD_DELETE_RESTAURANT_TITLE = 'delete_restaurant_title'
  FIELD_MAX_HISTORY_LIST = 'daily_max_list'
  FIELD_IMG_SRC = 'img_src'

  STATE_SELECT_RESTAURANT = 'select_restaurant'
  STATE_SELECT_GROUP = 'select_group'
  STATE_ADD_RESTAURANT = 'add_restaurant'
  STATE_ADD_RESTAURANT_DESC = 'add_restaurant_desc'
  STATE_ADD_RESTAURANT_CONFIRM = 'add_restaurant_confirm'
  STATE_DELETE_RESTAURANT = 'delete_restaurant'
  STATE_DELETE_RESTAURANT_CONFIRM = 'delete_restaurant_confirm'

  ARG_USER_KEY = "user_key"
  ARG_TYPE = 'type'
  ARG_CONTENT = 'content'

  ## buttons
  BTN_SELECT_LUNCH = "#점심 선택"
  BTN_SEE_RESULT = "#결과 보기"
  BTN_SETTING = '#설정'

  DEFAULT_KEYBOARD = [BTN_SELECT_LUNCH, BTN_SEE_RESULT, BTN_SETTING]

  BTN_GOTO_START = "#처음으로"
  BTN_SETTING_GROUP = '#그룹 설정'
  BTN_ADD_RESTAURANT = "#식당 등록"
  BTN_ADD_RESTAURANT_CONFIRM = "#식당 등록 확인"
  BTN_DELETE_RESTAURANT = '#식당 삭제'
  BTN_DELETE_RESTAURANT_CONFIRM = '#식당 삭제 확인'
  BTN_DOANTE = '#후원 하기'
  
  SHOW_COUNT = 7
```




<script src="https://gist.github.com/jacegem/fee3dae8e7a0c630dc612b76ad3d1911.js"></script>