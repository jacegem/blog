---
title: "[Hammerspoon] 스페이스들을 chooser 를 통해서 선택해보자"
date: 2023-11-15
tags: [hammerspoon, space, move]
categories:
  - Hammerspoon
  - Space
---


<img src="https://i.imgur.com/oJIh89a.png"  width="200" style="display:block;margin:0 auto;"/>

스페이스들을 chooser 를 통해서 선택해보자.

### Screen 과 Space 구분

디스플레이 메뉴에서 확인할 수 있는 것이 `Screen`이다. 

![](https://i.imgur.com/39bZbCJ.png)

그리고, 각각의 Screen 에서 나눠져 있는 것이 `Space`이다. 

![](https://i.imgur.com/qvpg9nN.png)


## missionControlSpaceNames

`hs.spaces.missionControlSpaceNames()` 를 사용해서, 각 스크린에서 가지고 있는 스페이스들에 대한 정보를 얻을 수 있다. 

> a key-value table in which the keys are the UUIDs for each screen and the value is a key-value table where the space ID is the key and the Mission Control name of the space is the value.

함수 결과값의 형태는 다음과 같다. 

```
{
  [screen-1 UUID] = {
    [space-1 ID] = 'space-1 name',
    [space-2 ID] = 'space-2 name',
  },
  [screen-2 UUID] = {
    [space-3 ID] = 'space-3 name',
    [space-4 ID] = 'space-4 name',
  }
}
```

이렇게 실행하면 모든 space들의 id 와 이름을 얻을 수 있다.

```lua
local function merge(t1, t2)
    for k, val in pairs(t2) do
        t1[k] = val
    end
    return t1
end

local idName = {}
local names = hs.spaces.missionControlSpaceNames()
for _screenId, screen in pairs(names) do
    merge(idName, screen)
end
```

## 데이터 변환

이걸 `chooser`에 사용하기 위한 데이터로 변환한다.

```lua
local choices = {}

for id, name in pairs(idName) do
    table.insert(choices, {
        uuid = id,
        text = name,
        subText = id
    })
end
```

## chooser 선택 시 실행

choose에 space 들의 이름을 보여주고, 하나를 선택하면 id 값을 받아서, 해당 space로 이동한다. 

```lua
chooser_util:start(choices, function(id)
    hs.spaces.gotoSpace(id)
end)
```

## chooser_util.lua 파일 생성

chooser 생성을 위한 파일을 만든다. 

```lua
local obj = {
  choices = {},
  fn = nil,
}

local function createChooser()
  obj.chooser = hs.chooser.new(function(choice)
    if not choice then
      return
    end

    if obj.fn ~= nil and choice.uuid ~= nil then
      obj.fn(choice.uuid)
    end
  end)
end

local function choiceChangedCallback(query)
  if query == '' then
    obj.chooser:choices(obj.choices)
    return
  end

  query = query:lower()

  if obj.matchCache[query] == nil then
    local words = split(query, " ")
    local choices = {}

    for _, c in pairs(obj.choices) do
      if c['text'] and isAllInText(c['text']:lower(), words) then
        table.insert(choices, c)
      end
    end

    choices = sortBySelected(choices)

    obj.matchCache[query] = choices
  end

  obj.chooser:choices(obj.matchCache[query])
end

function obj:start(choices, fn)
  obj.choices = choices
  obj.fn = fn
  
  createChooser()

  obj.chooser:choices(obj.choices)
  obj.chooser:queryChangedCallback(choiceChangedCallback)
  obj.chooser:query(nil)
  obj.chooser:show()
end

return obj
```

## 실행 화면

![](https://i.imgur.com/oJIh89a.png)



## 링크

-  https://www.hammerspoon.org/docs/hs.spaces.html#missionControlSpaceNames
