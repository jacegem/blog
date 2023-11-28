---
title: "[Karabiner] (3) 동시에 누르기"
date: 2023-11-30
tags: [karabiner, hammerspoon, modifiers, hotkey]
categories:
  - Application
  - Karabiner
---


동시에 2개의 키를 눌러서, 원하는키로 변경한다. 
관련내용은 [이곳](https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-definition/from/simultaneous/)에서 확인 할 수 있다. 

## from.simultaneous

s+d 키를 눌러서 `option+control`로 변경해보자. 
설정에 `from.simultaneous`를 추가한다. 

```json
{
  "description": "s+d ➡️ ctrl+opt",
  "copy-flip": true,
  "manipulators": [
    {
      "from": {
        "simultaneous": [
          {
            "key_code": "s"
          },
          {
            "key_code": "d"
          }
        ],
        "modifiers": {
          "optional": [
            "any"
          ]
        },
      },
      "to": [
        {
          "key_code": "left_control",
          "modifiers": [
            "left_option"
          ]
        }
      ],
      "type": "basic"
    }
  ]
},
```

## from.simultaneous_options

여기에 [옵션](https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-definition/from/simultaneous-options/)들을 설정할 수 있다. 

- detect_key_down_uninterruptedly : 관련 없는 이벤트로 키다운 감지를 중단할지 여부를 지정합니다.
- key_down_order : 키다운 순서 제한
- key_up_order : 키업 순서 제한
- key_up_when : key_up 이벤트가 게시되는 시기
- to_after_key_up : 모든 이벤트 실행된 이후 이벤트

```json
"simultaneous_options": {
  "detect_key_down_uninterruptedly": true,
  "key_down_order": "insensitive",
  "key_up_order": "insensitive",
  "key_up_when": "all",
  "to_after_key_up": []
}
```

detect_key_down_uninterruptedly 를 `true`로 설정해서, 관련 없는 키가 들어오면 중단되도록 한다. 
key_down_order, key_up_order 는 `insensitive`로 설정해서, 순서와 관계없도록 한다. 
key_up_when 은 `all`로 설정해서 모든 키가 올라온 이후에 처리되도록 한다. 
s + d 키를 동시에 누른 이후에, s키를 누른상태에서 d키를 놓아도 그대로 입력되도록 할 수 있다. (option+control 유지)

## 전체 설정 


```json
{
  "description": "s+d ➡️ ctrl+opt",
  "copy-flip": true,
  "manipulators": [
    {
      "from": {
        "simultaneous": [
          {
            "key_code": "s"
          },
          {
            "key_code": "d"
          }
        ],
        "modifiers": {
          "optional": [
            "any"
          ]
        },
        "simultaneous_options": {
          "detect_key_down_uninterruptedly": true,
          "key_down_order": "insensitive",
          "key_up_order": "insensitive",
          "key_up_when": "all",
          "to_after_key_up": []
        }
      },
      "to": [
        {
          "key_code": "left_control",
          "modifiers": [
            "left_option"
          ]
        }
      ],
      "type": "basic"
    }
  ]
},
```

## 조합 

위와 같은 방법으로 조합가능한 키들을 모두 추가한다. 
> 설정 파일은 [이곳](https://github.com/jacegem/karabiner/blob/main/karabiner.json)에서 볼수 있습니다.

아래와 같이 표기하였습니다.

- control ➡️ `ctrl`
- option ➡️ `opt`
- command ➡️ `cmd`
- shift ➡️ `sft` 
- capslock ➡️ `caps`

![](https://i.imgur.com/9zXT1h4.png)
![](https://i.imgur.com/JnbzDvQ.png)
![](https://i.imgur.com/tByC2Ng.png)
![](https://i.imgur.com/V7yDy9S.png)

## links 

- https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-definition/from/simultaneous/
- https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-definition/from/simultaneous-options/
- https://github.com/jacegem/karabiner/blob/main/karabiner.json
