---
title: "[Hammerspoon] 한글입숨 만들기"
date: 2023-10-31
tags: [hammerspoon, text, 한글, 입숨, lorem, ipsum]
categories:
  - Hammerspoon
  - Text
---

![](https://i.imgur.com/xz7fwIK.png)

## 개요

해머스푼으로 `한글입숨`을 만들어보자.
앱 디자인을 하면서 문자열이 필요할때 사용한다.

준비된 문자열에서 원하는 길이만큼 임의의 위치에서 잘라서 클립보드에 저장한다.
그리고 붙여넣기만 하면 된다. (Cmd + v)

## 문자열 준비

해머스푼 밑에 특정 경로에 텍스트 파일로 준비한다.

```shell
./text/ipsum/
├── 별.txt
├── 별_헤는_밤.txt
├── 새벽_편지.txt
├── 별을_굽다_김혜순.txt
├── 별국_공광규.txt
├── 별의_여인숙_이성선.txt
├── 소나기.txt
├── 유리창.txt
├── 저녁에_김광섭.txt
└── 사랑하는_별_하나_이성선.txt
```

별 헤는 밤에는 아래 내용이 들어 있다.

```dart
별 하나에 추억과
별 하나에 사랑과
별 하나에 쓸쓸함과
별 하나에 동경과
별 하나에 시와
별 하나에 어머니, 어머니,
...
```

## 파일 읽기

특정 경로에 있는 파일 목록을 읽는다.

```lua
local dir = os.getenv("HOME") .. "/.hammerspoon/text/ipsum/"
local file_list = io.popen('ls -a "' .. dir .. '"')
if file_list == nil then
  return
end
...
file_list:close()
```

그중에서 `.txt` 파일만 찾는다.
`lines_from` 함수를 호출하여 파일 내용을 가져온다.

```lua
local obj = {}
obj.text = ""

for file in file_list:lines() do
  local ext = file:sub(-4)
  if ext == ".txt" then
    if (#obj.text ~= 0) then
      obj.text = obj.text .. " "
    end

    local lines = lines_from(dir .. file)
    obj.text = obj.text .. lines
  end
end
```

## 파일 내용 읽기

파일이 있는지 확인하고
줄 단위로 읽어서, 공백제거후에 하나의 문자열로 합친다.

```lua
local function file_exists(file)
  local f = io.open(file, "rb")
  if f then f:close() end
  return f ~= nil
end

local function trim(s)
  return (string.gsub(s, "^%s*(.-)%s*$", "%1"))
end

local function lines_from(file)
  if not file_exists(file) then return "" end
  local lines = ""
  for line in io.lines(file) do
    line = trim(line)
    if (#line ~= 0) then
      if (#lines ~= 0) then
        lines = lines .. " "
      end
      lines = lines .. line
    end
  end
  return lines
end
```


## 한글입숨 만들기

요청시 길이를 전달하여 원하는 길이의 문자열을 만든다.
기본 길이는 10으로 하였다.

```lua
function utf8.sub(s, i, j)
  i = utf8.offset(s, i)
  j = utf8.offset(s, j + 1) - 1
  return string.sub(s, i, j)
end

function obj:create(len)
  len = len or 10
  local max_len = utf8.len(obj.text)
  local start = math.random(max_len - len)
  if (start < 0) then start = 0 end
  local res = utf8.sub(obj.text, start, start + len)
  res = trim(res)
  return res
end
```

## 단축키 등록

`hs.hotkey.modal`을 사용하여 길이별로 단축키를 등록한다.

> hs.hotkey.modal 사용은 이후에 따로 이야기 할께요.

```lua
local obj = { {
  key = "k",
  desc = "K",
  children = { {
    key = "l",
    desc = "Korean Lorem ipsum 10",
    fn = function()
      local res = korean_ipsum:create(10)
      hs.pasteboard.setContents(res)
    end
  }, {
    key = "1",
    desc = "Korean Lorem ipsum 100",
    fn = function()
      local res = korean_ipsum:create(100)
      hs.pasteboard.setContents(res)
    end
  }, {
    key = "2",
    desc = "Korean Lorem ipsum 200",
    fn = function()
      local res = korean_ipsum:create(200)
      hs.pasteboard.setContents(res)
    end
  }, {
    key = "3",
    desc = "Korean Lorem ipsum 300",
    fn = function()
      local res = korean_ipsum:create(300)
      hs.pasteboard.setContents(res)
    end
  }, }
} }
```

## 출처 

- https://stackoverflow.com/questions/43138867/lua-unicode-using-string-sub-with-two-byted-chars
- http://guny.kr/stuff/klorem/
