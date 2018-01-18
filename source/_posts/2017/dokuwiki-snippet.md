---
title: '[dokuwiki] - snippet 안보이게 처리'
date: 2017-01-20 19:13:45
tags: [wiki, dokuwiki, revealjs, snippet]
categories:
- Application
- Wiki
---

# [dokuwiki] - snippet 안보이게 처리

도쿠위키에서 마크다운으로 코드를 작성하면 코드 상단부에 다운로드 링크가 생긴다.
![](https://goo.gl/wnP8FV)

이를 제거하기 위해 `\dokuwiki\inc\parser\xhtml.php` 파일을 수정합니다.

615번째 줄 _hightlight 함수 안에 해당 내용을 추가합니다.

```php
function _highlight($type, $text, $language = null, $filename = null) {
	$filename = null;
	...
}
```

강제적으로 파라미터로 넘어온 `파일명`을 `null`로 변경하여, 화면상에 출력되지 않도록 합니다.

## 출처

- http://doku.ml/open/snippet_%EC%95%88%EB%B3%B4%EC%9D%B4%EA%B2%8C_%EC%B2%98%EB%A6%AC
