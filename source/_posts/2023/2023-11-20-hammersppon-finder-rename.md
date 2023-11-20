---
title: "[Hammerspoon] Finder 에서 이름 변경하기"
date: 2023-11-20
tags: [hammerspoon, finder, rename]
categories:
  - Hammerspoon
  - Clipboard
---

<img src="https://i.imgur.com/n5N1MvX.png"  width="200" style="display:block;margin:0 auto;"/>

원래 파일명 앞에, 숫자를 붙여서, 
`[숫자].[파일이름]` 으로 변경하려고 한다. 

## 흐름

1. `finder` 에서 `enter` 키를 누른다. 
2. 파일명을 복사한다. `cmd + c`
3. 원하는 이름으로 변경한다. 
4. 문자열을 입력한다. 
5. `return` 키를 누른다.

## 문제점

### enter 키가 없다

hammerspoon 키맵에 `enter`가 없고, `return`만 있다. 
그래서, `applescript` 로 처리한다.

```lua
hs.osascript.applescript([[tell application "System Events" to key code 76]])
```

### `cmd + c` 로 바로 복사되지 않는다

콜백함수를 만들어서, 처리한다.

```lua
hs.pasteboard.callbackWhenChanged(function(state) 
  ...
end)
```

## 처리

기존에 변경한 이름이 있는 경우, 원래 이름을 받기 위해서, 정규표현식을 사용한다. 

```lua
local original = string.gmatch(content, '%d*%.?(.-)$')()
```

## 원래 이름을 사용하고 싶은 경우

특정값을 조건으로 처리한다. 

```lua
local after = num .. "." .. original
if num == 10 then
    after = original
end
```

## 코드

hotkey.bind 에 사용하기 위해서 function 으로 생성한다.


```lua
function rename(num)
    return function()
        hs.osascript.applescript([[tell application "System Events" to key code 76]])
        hs.pasteboard.callbackWhenChanged(function(state)
            if state then
                local content = hs.pasteboard.getContents()
                if content ~= nil then
                    local original = string.gmatch(content, '%d*%.?(.-)$')()
                    local after = num .. "." .. original
                    if num == 10 then
                        after = original
                    end
                    hs.eventtap.keyStrokes(after)
                else
                    hs.pasteboard.setContents(os.date('%H:%M'))
                end
                hs.eventtap.keyStroke(nil, 'return')
            end
        end)
        hs.eventtap.keyStroke({ 'cmd' }, 'c')
    end
end
```

### hotkey bind

```lua
for _, n in ipairs({ 5, 6, 7, 8, 9, 10 }) do
    hs.hotkey.bind( nil, "F" .. n, nil, rename(n))
end
```


## 참고 링크 

- https://www.hammerspoon.org/docs/hs.keycodes.html#map
- https://eastmanreference.com/complete-list-of-applescript-key-codes
