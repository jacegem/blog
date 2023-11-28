---
title: "[Karabiner] Capslock 키에 ESC 키 추가하기"
date: 2023-11-29
tags: [karabiner, hammerspoon, modifiers, hotkey]
categories:
  - Application
  - Karabiner
---


기존에 설정한 대로 caplock 키를 누르면 `control+option` 로 동작하지만, 
다른키가 눌리지 않는다면, `escape`로 동작하도록 만든다.  

### capslock 룰

설정된 상태는 아래와 같다. 

```json
{
    "manipulators": [
        {
            "description": "Change caps_lock to control+option.",
            "from": {
                "key_code": "caps_lock",
                "modifiers": {
                    "optional": [
                        "any"
                    ]
                }
            },
            "to": [
                {
                    "key_code": "left_option",
                    "modifiers": [
                        "left_control"
                  ]
                }
            ],
            "type": "basic"
        }
    ]
}
```

## 단독키인 경우 설정

`caplock`키가 단독으로 눌리면 `escape`로 동작하도록 하기 위해 아래 내용을 추가한다.

```json
"to_if_alone": {
    "key_code": "escape"
},
```

- https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-definition/to-if-alone/


## 추가된 상태

```json
{
  "description": "caps_lock ➡️ ctrl+opt",
  "manipulators": [
    {
      "from": {
        "key_code": "caps_lock",
        "modifiers": {
          "optional": [
            "any"
          ]
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
      "to_if_alone": {
        "key_code": "escape"
      },
      "type": "basic"
    }
  ]
},
```

## EventViewer 로 확인

![](https://i.imgur.com/PBz4kOm.png)

`left_option`과 `left_control`이 눌리고 난 뒤에 `escapse`가 눌린것을 볼 수 있다. 


## link

- https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-definition/to-if-alone/
