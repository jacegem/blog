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

1. 선택된 파일을 찾는다.
2. 구분자 `.` 를 찾는다. (`.` 로 설정한 경우입)
3. 원하는 값을 앞에 넣는다.
4. 파일명을 변경한다. 
5. `esc` 를 누른다. (이름 변경 상태인 경우에, `esc`를 눌러서 빠져 나온다. ) 

## Applescript

```lua
local renameFile = function(num)
    local success, res, desc = hs.osascript.applescript(
        [[  set num to "]] .. num .. [[" --

        tell application "Finder"
            set theFiles to (get selection)
            repeat with i from 1 to count of theFiles
                set thisFile to item i of theFiles
                set thisName to name of thisFile
                set theOffset to my (offset of "." in thisName)
                if theOffset < 4 then
                    set orgName to text (theOffset + 1) thru -1 of thisName
                    set newName to num & "." & orgName
                    if num = "10" then
                        set newName to orgName
                    end if
                    set name of thisFile to newName
                end if
            end repeat
        end tell]])
    -- print(success, res, desc)
    hs.eventtap.keyStroke(nil, 'escape')
end
```

## key bind

F5 ~ F10 키를 바인딩 한다. `F10` 은 원래로 돌아오는 용도다. 

```lua
for _, n in ipairs({ 5, 6, 7, 8, 9, 10 }) do
    hs.hotkey.bind(nil, "F" .. n, rename(n)) 
end
```
