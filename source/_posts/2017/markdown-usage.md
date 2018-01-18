---
title: Markdown 사용법
date: 2017-02-21
tags: [markdown, pro]
categories:
- Programming
- ETC
---

# Markdown 사용법

처음 사용시에는 `라이브 렌더링`이 되는 것이 좋습니다. 텍스트로 마크 다운 형식으로 작성하면 바로 변형되서 화면에 보이는 것입니다.

줄바꿈으로 글을 작성하였지만, 렌더링시에 줄바꿈 글이 이어서 나오는 것을 볼 수 있습니다. 이럴때는 중간에 빈 줄을 넣어 명시적으로 줄바꿈을 표시해야 합니다. 물론, 이를 지원하는 프로그램도 있습니다. 이런 경우는 줄간격이 큰 것을 확인 할 수 있습니다.

## 윈도우 프로그램

마크다운을 기본으로 작성하는 문서 편집기 입니다.

- typora (`추천`) https://typora.io/ (라이브 렌더링)
- haroopad : http://pad.haroopress.com/

## 플러그인

코드 편집기에 플러그인을 추가해서 사용하는 방식입니다.

- visual studio code : https://code.visualstudio.com/
- atom : https://atom.io/


## 웹

- http://classeur.io/ (라이브 렌더링)
- https://www.gitbook.com
- https://trello.com/

# 문법

공통적으로 사용하는 마크다운 문법과 각 프로그램 및 서비스에서 사용하는 확장 문법이 있습니다. 기본 문법만 익히고 확장 문법은 각 프로그램에서 제공하는 방식을 이용하면 됩니다.

## 기본 문법

### 헤더 작성

<code>
# This is an H1
## This is an H2
###### This is an H6
</code>

출력결과

# This is an H1
## This is an H2
###### This is an H6



### 인용구

<code>
> This is a blockquote with two paragraphs. This is first paragraph.
>
> This is second pragraph.Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
> This is another blockquote with one paragraph. There is three empty line to seperate two blockquote.
</code>


출력결과

> This is a blockquote with two paragraphs. This is first paragraph.
>
> This is second pragraph.Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
> This is another blockquote with one paragraph. There is three empty line to seperate two blockquote.




### 목록

<code>
## un-ordered list
*  Red
*  Green
*  Blue

## ordered list
1. Red
2. Green
3. Blue
</code>

앞에 `* `, 또는 `- `를 사용하여 순서가 없는 목록을 작성할 수 있습니다. (공백이 필요합니다.)

순서가 필요한 경우에는 앞에 `1.`, `2.` 숫자 와 점(.)을 이어서 사용합니다.

숫자를 증가시키면서 작성할 필요는 없습니다. 아래와 같이 작성하여도 동일한 결과를 얻을 수 있습니다.

<code>
## ordered list
1. Red
1. Green
1. Blue
</code>

출력결과

## un-ordered list

*   Red
*   Green
*   Blue

## ordered list

1.  Red
1. 	Green
1.	Blue

### 코드

<code>
Use the `printf()` function.
</code>

출력 결과

Use the `printf()` function.



### 코드 블럭

<code>
​```
function test() {
  console.log("notice the blank line before this function?");
}
```
</code>

하이라이트 문법을 적용할 경우에는 \`\`\` 뒤에 언어명을 작성합니다.

<code>
```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```
</code>

출력결과


```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```




### 표 작성

<code>
| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |
</code>

출력 결과

| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |



### 링크

<code>
This is [an example](http://example.com/ "Title") inline link.

[이곳](http://doku.ml/open/마크다운_사용법) 의 주소는 http://doku.ml/open/마크다운_사용법 입니다.
</code>

출력 결과

This is [an example](http://example.com/ "Title") inline link.

[이곳](http://doku.ml/open/마크다운_사용법) 의 주소는 http://doku.ml/open/마크다운_사용법 입니다.


### 이미지

<code>
![typora](https://goo.gl/CXHGTE)

![Alt text](/path/to/img.jpg "Optional title")
</code>

출력결과

![typora](https://goo.gl/CXHGTE "typora")


### 강조

<code>
*single asterisks*

_single underscores_
</code>

*single asterisks*

_single underscores_

### 굵은 글씨

<code>
**double asterisks**
</code>

출력결과

**double asterisks**


### 밑줄

<code>
__double underscores__
</code>

출력결과

__double underscores__



## 출처

- http://support.typora.io/Markdown-Reference/
