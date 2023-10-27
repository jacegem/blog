---
title: "[Hammerspoon] capslock 키를 사용하자"
date: 2023-10-23
tags: [hammerspoon, hotkey, capslock]
categories:
  - Hammerspoon
  - Hotkey 
---

![](https://i.imgur.com/D2Ob7rb.png)


Hammerspoon 에서 바로 `capslock`키를 사용할 수 없으므로, `karabiner`를 통해서 설정한다.

## karabiner 설치 

https://karabiner-elements.pqrs.org/ 에서 다운받아서 설치한다.


## karabiner 설정 

![](https://i.imgur.com/qFP2svM.png)

Complex Modification 에서 Change Capslock ... 를 추가한다. 

기본 설정은, cmd + option + ctrl + shift 이므로, 활용도가 떨어진다. 

`~/.config/karabiner/karabiner.json` 파일을 수정한다. 

해당 파일 중간쯤 보면, "Change caps_lock to command+control+option+shift." 로 적힌 부분을 찾을 수 있다. 

이곳에 있는 `to` 부분을 수정한다. 

capslock 키를 누르면 left_control, left_option 키가 눌린 것으로 처리하는 설정이다. 


```json
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
                "key_code": "left_control",
                "modifiers": [
                    "left_option"
                ]
            }
        ],
        "type": "basic"
    }
]
```


## modules/const.lua 생성 

option과 ctrl키를 `capslock` 으로 정의해서 사용한다. 

```lua
local obj = {}
obj.app = {}

obj.capslock = { "option", "ctrl" }

return obj
```

cmd, shift를 추가적으로 등록해서 사용한다. 

```lua
obj.capslock = { "option", "ctrl" }
obj.capslockCmd = { "cmd", "option", "ctrl" }
obj.capslockShift = { "option", "ctrl", "shift" }
obj.capslockCmdShift = { 'cmd', "option", "ctrl", "shift" }
```

이렇게 하면 capslock 를 사용하는 방법이 다양해 진다. 

## capslock를 사용하여 hotkey 설정

```lua
local const = require('modules.const')
local capslock = const.key.capslock

hs.hotkey.bind(capslock, 'tab', function()
    hs.alert.show('altMode:enter()')
    altMode:enter()
end)
```

## 출처 

- https://karabiner-elements.pqrs.org/
