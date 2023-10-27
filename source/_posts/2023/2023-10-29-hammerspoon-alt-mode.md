---
title: "[Hammerspoon] hotkey.modal 을 사용해서 새로운 키배열을 만들어보자"
date: 2023-10-29
tags: [hammerspoon, hotkey, modal, alt, mode]
categories:
  - Hammerspoon
  - Hotkey 
---

![](https://i.imgur.com/BqT4bCI.png)

키입력을 하지 않고, 마우스만 이용하는 상황이라면, 
키보드 배열을 변경해서 더 편리한 환경을 만들 수 있다. 

Hammerspoon 을 사용해서, 키보드 배열을 바꿔보자.

## hotkey/alt_mode.lua 생성 

`new()` 로 새로운 modal 을 생성한다.

```lua
local altMode = hs.hotkey.modal.new()
```

## enter 호출

modal을 시작하기 위한 키를 설정한다. 

`capslock`과 `tab`키를 누르면 altMode Modal 을 시작한다. 

```lua
hs.hotkey.bind(capslock, 'tab', function()
    hs.alert.show('altMode:enter()')
    altMode:enter()
end)
```
이후에 altMode 에 키를 bind 하여 사용할 수 있다. 

## exit 호출

해당 modal 을 종료하기 위한 exit 호출 키를 설정한다. 

`tab`키를 누르거나, `esc`를 누르면 해당 modal을 종료시킨다.

```lua
altMode:bind(nil, 'tab', function()
    hs.alert.show('altMode:exit()')
    altMode:exit()
end)

altMode:bind(nil, 'escape', function()
    hs.alert.show('altMode:exit()')
    altMode:exit()
end)
```

## modules/key.lua 생성

bindModal 함수를 생성한다. 
mod, key 에는 입력키를 설정한다. 
action 에는 변환 키 또는 함수가 전달될 수 있다.


```dart
local obj = {}

local function keyStrokeFn(value)
    return function()
        hs.eventtap.keyStroke(value[1], value[2])
    end
end

local function pcallFn(f)
    return function()
        pcall(f)
    end
end

function obj:bindModal(mod, key, action, modal)   
    if type(action) == 'table' then
        if modal ~= nil then
            if action[3] == 'up' then
                modal:bind(mod, key, nil, keyStrokeFn(action))
            else
                modal:bind(mod, key, keyStrokeFn(action), nil, keyStrokeFn(action))
            end
        else
            if action[3] == 'up' then
                hs.hotkey.bind(mod, key, nil, keyStrokeFn(action))
            else
                hs.hotkey.bind(mod, key, keyStrokeFn(action), nil, keyStrokeFn(action))
            end
        end
    elseif type(action) == 'function' then
        if modal ~= nil then
            modal:bind(mod, key, pcallFn(action), nil, pcallFn(action))
        else
            hs.hotkey.bind(mod, key, pcallFn(action), nil, pcallFn(action))
        end
    end
end

return obj
```

## 키 설정

`i`키를 입력하면 `up`이 실행되도록 한다. 

```dart
key:bindModal(nil, 'i', { nil, 'up' }, altMode)
key:bindModal(nil, 'j', { nil, 'left' }, altMode)
key:bindModal(nil, 'k', { nil, 'down' }, altMode)
key:bindModal(nil, 'l', { nil, 'right' }, altMode)
```

`,`키를 입력하면 `mouse:wheelFn('down')` 함수가 실행되도록 한다.  

```dart
key:bindModal(nil, ',', mouse:wheelFn('down'), altMode)
key:bindModal(nil, 'm', mouse:wheelFn('up'), altMode)
```


## 출처 

- https://www.hammerspoon.org/docs/hs.hotkey.modal.html
