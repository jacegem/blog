---
title: "[Karabiner] (1) capslock 키 변경하기"
date: 2023-11-28 
tags: [karabiner, hammerspoon, modifiers, hotkey]
categories:
  - Application
  - Karabiner
---

## karabiner 설치

![](https://i.imgur.com/Z1iLyUG.png)

https://karabiner-elements.pqrs.org/ 에서 다운받아 설치한다. 


## rule 추가 

![](https://i.imgur.com/mupSjBi.jpg)

`Complex Modifications` > `Add rule`을 선택한다. 
Examples 에서 보이는 `Change caps_lock to command+control+option+shift` 를 추가한다. 
옆에 보이는 `Enable`을 선택한다. 

## karabiner.json

`/.config/karabiner/karabiner.json`에 해당 룰이 추가된다.

### 기본 구조 

json 파일의 내용을 간단히 살펴보면 아래와 같다. 
여러개의 profile 를 가지고, selected 가 `true`가 된 것을 사용한다.

```json
{
    "profiles": [
        {
            "selected": true,
            "name": "Default profile",
            "complex-modifications": {
                "rules": [
                    <여기에 추가된다>
                ]
            }
        }
    ] 
}
```

![](https://i.imgur.com/DAxYgJq.png)
프로파일 목록은 `Profiles`에서 확인할 수 있다. 

### 추가된 룰 

```json
{
    "manipulators": [
        {
            "description": "Change caps_lock to command+control+option+shift.",
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
                    "key_code": "left_shift",
                    "modifiers": [
                        "left_command",
                        "left_control",
                        "left_option"
                    ]
                }
            ],
            "type": "basic"
        }
    ]
}
```

caps_lock 을 누르면, `command+control+option+shift`로 동작되는 설정이다. 

## 수정

command, shift 를 따로 사용하기 위해서, 
caps_lock 을 누르면, `control+option`로 동작되도록 하기 위해 해당 부분을 수정한다. 

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

## EventViwer 로 확인

![](https://i.imgur.com/CapdrWz.png)

Karabiner-Elements 메뉴바에서 `Launch EventViewer...`를 선택해서 EventViwer를 실행핸다. 

![](https://i.imgur.com/glSlkA4.png)

`CapsLock`키를 눌러서 이벤트를 확인한다. 
`left_option`과 `left_control`이 눌러진 것을 볼 수 있다. 


## 이후에...

이 설정을 이용해서, 이후에 Hammerspoon 으로 원하는 동작이 이뤄지도록 설정한다.
관련 내용은 [이곳](https://jacegem.github.io/blog/categories/Hammerspoon/)에서 확인 할 수 있다.

karabiner 전체 설정은 [이곳](https://github.com/jacegem/karabiner/blob/main/karabiner.json)에서 확인 할 수 있다. 

## link

- https://karabiner-elements.pqrs.org/
- https://jacegem.github.io/blog/categories/Hammerspoon/
- https://github.com/jacegem/karabiner/blob/main/karabiner.json
