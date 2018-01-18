---
title: 'ConEmu 다운로드 및 설정'
date: 2017-01-11 19:13:45
tags: [console, conemu]
categories:
- Application
- Console
---

# ConEmu 다운로드 및 설정

## 다운로드

- 홈페이지 : https://conemu.github.io/
- 다운로드 : https://www.fosshub.com/ConEmu.html

다운받아서 설치를 진행하시면 됩니다.

## Split

![](https://conemu.github.io/img/ConEmu-Maximus5.png)

홈페이지의 이미지처럼 나누기 위해서 splite 단축키를 사용할 수 있습니다.

Setting 화면으로 들어가서, `split`으로 검색을 합니다.

![](https://goo.gl/v4LVer)

- Ctrl+Shift+O : 수직으로 절반 나눠서 창을 복사합니다.
- Ctrl+Shift+E : 수평으로 절반 나눠서 창을 복사합니다.

> 만약 단축키가 실행되지 않는다면, 다른 프로그램에 할당된 것은 아닌지 확인해 봐야 합니다. 저는 ShareX 프로그램에서 사용하고 있어서 실행되지 않았었습니다.

## 설정

타이틀바에서 마우스 우클릭을 하면 팝업 메뉴를 볼 수 있습니다.

![](https://goo.gl/e78Scb)

### Scheme 변경

![](https://goo.gl/smm98J)

설정 메뉴에서 → Features → Colors → Schemes 를 변경합니다.

## 텍스트 색 변경

`react-native-cli` 실행시에, 텍스트가 안보인다면,

![](https://goo.gl/c1uCvU)

설정 메뉴에서 → Features → Colors 메뉴에서 `0`번의 색을 흰색으로 변경합니다.

### 투명도 변경

![](https://goo.gl/sZspDi)

설정 메뉴에서 → Features → Transparency 메뉴에서 Active window transparency를 선택합니다.

슬라이드를 움직여 투명도를 조절합니다.

## 배경이미지 변경

https://wallpapersafari.com/kali-linux-wallpaper-hd/ 에서 kali 월페이퍼를 다운로드 합니다.

![](https://goo.gl/esDM7m)

설정 메뉴에서 → Main → Background 메뉴에서 Background image를 선택합니다.

## 프롬프트 변경

![](https://goo.gl/Cv2zBs)

```
set PROMPT=$E[32m$E]9;8;"USERNAME"$E\@$E]9;8;"COMPUTERNAME"$E\$S$E[92m$P$E[90m$_$E[33m$T$E[37;1m$G$S$E[m
```
`PROMPT` 환경변수를 추가하여 프롬프트를 변경할 수 있습니다.
