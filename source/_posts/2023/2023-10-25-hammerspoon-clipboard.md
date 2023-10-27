---
title: "[Hammerspoon] 클립보드 링크를 마크다운 이미지 코드로 변경하기"
date: 2023-10-25
tags: [hammerspoon, clipboard, markdown, image]
categories:
  - Hammerspoon
  - Clipboard 
---

![](https://www.hammerspoon.org/images/hammerspoon.png)


## 처리 순서

먼저 클립보드의 내용을 가지고 온다. 

문자열에서, 앞뒤의 공백을 제거한다.

마크다운 이미지 `![](링크주소)` 의 형태로 변경한다. 

해당문자열을 클립보드에 복사한다. 

`Cmd + v` 키로 붙여넣기를 한다. 


## 함수 작성 

```lua
function obj:markdownImage()
    local clipboard = hs.pasteboard.getContents()
    local text = clipboard:match "^%s*(.-)%s*$"
    local result = "![](" .. text .. ")"
    hs.pasteboard.setContents(result)
    hs.eventtap.keyStroke('cmd', 'v')
end
```

## 단축키 설정을 위한 함수 작성 

앱 별로 다른 동작을 할수 있도록 함수를 작성한다. 

키를 눌렀다가 뗄때 작동하도록 하기 위해, bind 함수의 네번째 인자에 함수를 작성한다. 

- https://www.hammerspoon.org/docs/hs.hotkey.html#bind


```lua
function obj:bindUp(mod, key, strokeMod, strokeKey, apps)
    hs.hotkey.bind(mod, key, nil, function()
        local found = runByApps(apps, strokeMod)

        if found == false then
            if mod == strokeMod and key == strokeKey then
                hs.eventtap.event.newKeyEvent(mod, key, true):setProperty(hs.eventtap.event.properties
                    .keyboardEventAutorepeat, 1):post()
            else
                hs.eventtap.keyStroke(strokeMod, strokeKey)
            end
        end
    end)
end
```


## 단축키 설정


Caps + 1 로 만든다. 


> karabiner 를 이용하여
> Caps 는 control + option 으로 동작하도록 설정한다.

앱별로 다른 동작을 하도록 설정한다. 

logseq, brave, code 에서는, 마크다운 이미지 코드 생성함수를 호출한다.

chrome, arc 에서는 다른 동작을 하도록 한다. 

그리고, 설정한 앱이 없다면, `1` 키를 누르도록 한다.


```lua
local key = require('modules.key')
local cf = require('modules.common_function')
local const = require('modules.const')

local capslock = const.key.capslock

key:bindUp(capslock, '1', nil, '1', {
  [const.logseq] = cf.markdownImage,
  [const.brave] = cf.markdownImage,
  [const.code] = cf.markdownImage,
  [const.chrome] = cf.dokuwikiImage,
  [const.arc] = function()
    hs.eventtap.keyStroke({ "control" }, "1")
  end
})
```
