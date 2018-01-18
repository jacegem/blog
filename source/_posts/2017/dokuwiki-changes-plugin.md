---
title: 도쿠위키 Changes Plugin
date: 2017-01-18 19:13:45
tags: [wiki, dokuwiki]
categories:
- Application
- Wiki
---

# 도쿠위키 Changes Plugin

## 플러그인 설치

도쿠위키에서 changes 플러그인을 설치합니다.

![](https://goo.gl/e7Q6j7)

도쿠위키 확장플러그인 설치에서 `changes` 로 검색해도 찾을 수가 없습니다.

[플러그인 페이지](https://www.dokuwiki.org/plugin:changes)로 이동하여 zip 파일을 다운 받습니다.

그 후, 수동 설치 메뉴에서 다운 받은 파일을 선택하여 해당 플러그인을 설치합니다.

![](https://goo.gl/ezgkt8)

또는 URL (https://github.com/cosmocode/changes/zipball/master)을 입력하여 설치합니다.


## 문법 및 사용

```
{{changes>}}
```

### 이름공간으로 제어 Whitelist/blacklist by namespace

추가대상

```
{{changes>foo}}
{{changes>ns=foo}}
```

제외대상
```
{{changes>ns = -foo}}
{{changes>ns = foo, bar, -bar:baz}}
```

## 결과

![](https://goo.gl/o6Zxux)

위와 같이 최근에 수정된 사항이 목록으로 표출됩니다.


## 출처

- http://doku.ml/open/changes_%ED%94%8C%EB%9F%AC%EA%B7%B8%EC%9D%B8_%EC%82%AC%EC%9A%A9
- https://www.dokuwiki.org/plugin:changes
