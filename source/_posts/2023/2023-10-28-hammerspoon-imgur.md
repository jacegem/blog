---
title: "[Hammerspoon] imgur 에 이미지를 업로드 하자"
date: 2023-10-28
tags: [hammerspoon, clipboard, imgur]
categories:
  - Hammerspoon
  - Clipboard 
---

![](https://i.imgur.com/OBclS1d.png) 


## Client ID 생성

이미지 업로드에 사용하기 위하여 imgur 에서 `Client ID` 를 생성한다.

![](https://i.imgur.com/fJRiFGM.png)

## hs.settings 설정

위에서 생성한 `Client ID` 값을 settings 에 설정한다.
> 바로, 값을 사용해도 됩니다.

```lua
local obj = {}
obj.config = { ["imgurKey"] = "YOUR-CLIENT-ID", }

for key, val in pairs(obj.config) do
  hs.settings.set(key, val)
end
```

## imgur 업로드 함수 

처리순서

1. 클립보드에 있는 이미지를 불러온다.
2. 임시파일로 저장한다.
3. base64 인코딩을 한다. 
4. imgur Client ID 를 사용하여 https://api.imgur.com/3/upload.json 에 업로드한다. 
5. 결과값에서 `link`를 확인한다.
6. link 를 클립보드에 넣는다. 


```lua
local obj = {}

function obj:upload()
  local image = hs.pasteboard.readImage()

  if image then
    local tempfile = "/tmp/imgur-tmp.png"
    image:saveToFile(tempfile)
    local b64 = hs.execute("base64 -i " .. tempfile)
    b64 = hs.http.encodeForQuery(string.gsub(b64, "\n", ""))

    local url = "https://api.imgur.com/3/upload.json"
    local headers = { Authorization = "Client-ID " .. hs.settings.get("imgurKey") }
    local payload = "type='base64'&image=" .. b64

    hs.http.asyncPost(url, payload, headers, function(status, body, headers)
      print(status, headers, body)
      if status == 200 then
        local response = hs.json.decode(body)
        local imageURL = response.data.link
        hs.urlevent.openURLWithBundle(imageURL, hs.urlevent.getDefaultHandler("http"))
        hs.pasteboard.setContents(imageURL)
        return imageURL
      end
    end)
  end
end

return obj;
```

## key binding

`hs.hotkey.modal` 를 사용하여 키를 할당한다. 
> 이 부분은 나중에 다룰 예정입니다.


`i`, `u` 키를 입력해서, `imgur:upload()`를 실행한다. 

```lua
local imgur = require("modules.imgur")

local obj = { {
  key = "i",
  desc = "image",
  children = { {
    key = "u",
    desc = "image upload",
    fn = function() imgur:upload() end
  },
  }
} }

return obj
```


## 출처

- https://imgur.com/
- https://github.com/heptal/stale-dotfiles/blob/master/roles/hammerspoon/files/imgur.lua
