---
title: "[Hammerspoon] hs.settings 사용하기"
date: 2023-10-26
tags: [hammerspoon, settings]
categories:
  - Hammerspoon
  - Settings 
---

![](https://www.hammerspoon.org/images/hammerspoon.png)


## hs.settings

`hs.settings.set("key", "val")` 로 값을 설정하고
`hs.settings.get("key")` 으로 값을 읽어올 수 있다.

- https://www.hammerspoon.org/docs/hs.settings.html


## 시크릿 값 설정을 위한 파일 생성

`secrets/settings.lua` 파일을 생성한다. 

예제 코드로, "key" 키값에 "val2" 값을 설장하였다.


```lua
local obj = {}
obj.config = { ["key"] = "val2", }

for key, val in pairs(obj.config) do
  hs.settings.set(key, val)
end
```

## init.lua 에서 호출

init.lua 에 `require` 를 추가한다. 

```lua
require('secrets.settings')
```


## 키 바인딩 해서 값을 확인

콘솔에서 값이 출력되는 것을 확인할 수 있다. 


![](https://i.imgur.com/Au28IaF.png)


```lua
key:bindUp(capslock, "F6", function()
    print(hs.settings.get('key'))
end)
```

## key 값 정의

키값을 정의해서 사용하자.
그러면 사용하는 곳에서 오타가 발생할 우려가 없다.

`modules/const.lua` 파일을 생성한다. 

```lua
local obj = {}

obj.app = {}
obj.settings.imgurClientId = 'imgurClientId'

return obj
```

## secrets/settings.lua 수정

```lua
local const = require('modules.const')

local obj = {}

obj.config = { [const.settings.imgurClientId] = "YOUR-CLIENT-ID" }

for key, val in pairs(obj.config) do
  hs.settings.set(key, val)
end

return obj

```

## hotkey/function.lua 수정  


```lua
local const = require('modules.const')

key:bindUp(capslock, "F6", function()
    print(hs.settings.get(const.settings.imgurClientId))
end)
```
