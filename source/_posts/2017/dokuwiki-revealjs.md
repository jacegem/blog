---
title: 도쿠위키에서 reveal.js 사용하기
date: 2017-01-19 19:13:45
tags: [wiki, dokuwiki, revealjs]
categories:
- Application
- Wiki
---

# 도쿠위키에서 reveal.js 사용하기

[revealjs 도쿠위키 플러그인](https://www.dokuwiki.org/plugin:revealjs)을 설치하여 `revealjs` 를 사용합니다.

마크다운 텍스트를 사용하여 발표화면을 만들 수 있습니다.

문서는 [이곳](https://github.com/neuralyzer/dokuwiki-plugin-revealjs/blob/master/README.md)에서 확인 할 수 있습니다.

## 사용 가능한 테마 (Available themes)

Reveal.js 테마를 사용할 수 있습니다. 가능한 값 :

- black
- white
- beige
- blood
- league
- default
- moon
- night
- serif
- simple
- sky
- solarized
- dokuwiki

기본값은 `white` 입니다.

## 컨트롤 (Controls)

reveal.js 컨트롤을 표시합니다. 두 값

- false
- true

기본값는 `false` 입니다.

## 프로그래스 바 (Progress bar)

reveal.js 프로그래스 바를 표시합니다. 두 값

- false
- true

기본값은 `false` 입니다.

## 모든 목록 작성 (Build all lists)

한 목록씩 작성할지 여부를 설정합니다. 두 값

- false
- true

기본값은 `false` 입니다.

## 전환 (Transition)

슬라이드 전환. 가능한 설정:

- none
- fade
- slide
- convex
- concave
- zoom

기본값은 `fade` 입니다.

## 수평 슬라이드 레벨 Horizontal slide level

이 레벨 이상의 헤더는 수평 슬라이드를 시작합니다. 수직 (중첩) 슬라이드 시작 레벨 아래 - 슬라이드에 영향을주지 않으며 대체 슬라이드 표시기 (`---->` 및
`---->>`)로 표시됩니다. 가능한 설정 :

```
 H1 에서 슬라이드를 구별할지, H2에서 슬라이드를 구별하지를 설정합니다.
```

- 1
- 2

기본값은 `2` 입니다.

## 세로 슬라이드 머리글 확대 Enlarge vertical slide headers

horizontal_slide_level 아래 슬라이드의 헤더를 확대합니다 - 슬라이드에 영향을주지 않으며 대체 슬라이드 표시 (
`---->` 및 `---->>`)로 표시됩니다. 부울

- false
- true

기본값은 false 입니다.

## 이미지 테두리 Image borders

이미지 테두리를 표시합니다 (Reveal.js의 기본값). 부울 :

- false
- true

기본값은 false 입니다.

## 크기 Size

슬라이드의 기본 크기 (픽셀 단위) - 사용 가능한 공간에 맞게 슬라이드가 확대됩니다.

```
<width>x<height>
```

기본값은 960x700 입니다.

## 지원되는 dokuwiki 구문

헤드 라인, 테이블, 기울임 꼴, 굵은 글씨체와 같은 일반적인 것들을 제외하고 다음과 같은 구문 요소가 지원됩니다.

- 이미지 정렬 : 왼쪽 또는 오른쪽 또는 가운데 맞춤
- dokuwiki plugin wrap의 `<wrap lo> </ wrap>` 및 `<WRAP lo> </ WRAP>`는 작은 텍스트로 표시됩니다.
- `<WRAP clear></WRAP>`

## 추가 구문 Extra syntax

테마 선택 및 프레젠테이션 시작 버튼

페이지 상단에

```
~~REVEAL~~
```

이 위치에 버튼을 삽입합니다. 이 버튼을 클릭하면 기본 테마로 프레젠테이션이 시작됩니다.

또는 테마를 선택하려면

```
~~REVEAL theme_name~~
```

`theme_name`에 "사용 가능한 테마"아래에 나열된 reveal.js 테마 중 하나로 대체합니다.

다른 모든 옵션은 URL 쿼리 매개 변수 구문을 사용하여 wiki 페이지에서 덮어 쓸 수 있습니다.

```
~~REVEAL theme=sky&transition=convex&controls=1&show_progress_bar=1&build_all_lists=1&show_image_borders=0&horizontal_slide_level=2&enlarge_vertical_slide_headers=0&show_slide_details=1&open_in_new_window=1~~
```

부울 값은 숫자 (1 또는 0) 여야합니다. 프리젠테이션이 시작된 후 URL에서 직접 옵션을 변경하려면 페이지 맨 위에 `~~NOCACHE~~`를 넣어 DokuWiki의 캐싱을 비활성화해야합니다.

## 슬라이드 배경 Slide background

플러그인에서 구문을 소개합니다.

```
{{background>parameters}}
```

가능한 모든 매개 변수에 대해서는 아래의 대체 슬라이드 표시기를 참조하십시오.

이렇게 정의된 배경은 다음 슬라이드에 적용됩니다. 배경 태그는 다음 슬라이드를 여는 제목보다 앞에 있어야하며 해당 슬라이드에만 적용됩니다. 예를 들어

```
{{background>:wiki:dokuwiki-128.png}}
===== my heading=====

배경이 `있는` 슬라이드

===== my second heading=====

배경이 `없는` 슬라이드
```

배경이 있는 슬라이드 하나와 배경이 없는 두 번째 슬라이드를 만듭니다.


## 대체 슬라이드 표시기 Alternative slide indicators

```
---- salmon wiki:dokuwiki-128.png 10% repeat bg-slide no-footer ---->

<notes>
This slide has no content, but therefore a fancy background...
</notes>

<----
```

-`---->`는 기본 속도로 기본 슬라이드로 새로운 슬라이드를 엽니 다 (열려있는 이전 슬라이드는 암시 적으로 닫힙니다)
- 전체 예제 - 매개 변수는 CSS와 같이 동적으로 파싱됩니다. 매개 변수 순서는 중요하지 않으며 모든 키워드를 공백으로 분리했기 때문에 공백을 사용할 수 없습니다: `---- orange wiki:dokuwiki-128.png 10% repeat bg-slide zoom-in fade-out slow no-footer ---->`
  - 가능한 모든 HTML 색상 및 코드가 지원됩니다: `red`, `#f00`, `#ff0000`, `rgb(255,0,0)`, `rgba(255,0,0,0.5)`, `hsl(0,100%,50%)`, `hsla(0,100%,50%,0.5)`
  - 배경 이미지는 gif, png, jpg, jpeg, svg 인 경우 대문자와 소문자를 구분하지 않으며 DokuWiki 이미지 식별자 (`:wiki:dokuwiki-128.png`) 또는 일반적인 이미지 링크('http://host.tld/path/to/image.png')를 사용합니다.
  - 배경 이미지 크기는 후위 `%`와`px` 또는`auto`,`contain` 및`cover` 키워드로 인식됩니다. (Reveal.js의 기본값은 cover입니다)
    - 예 :`10 %`또는`250px` (일반적으로 백분율 값만 사용하는 것이 좋습니다. 나머지 슬라이드에서는 크기를 조정하고 위키 페이지의 슬라이드 배경 미리보기는 "실제"미리보기를 표시합니다)
  - 배경 이미지의 위치는`top`,`bottom`,`left`,`right`,`center` 키워드에 의해 인식됩니다 (Reveal.js의 기본값은 center입니다). xx, y 값은 px 또는 %로 대체됩니다. : 상단 왼쪽, 하단 중앙, 3 %, 5 %, 20px, 5 % (이미지 크기와 위치를 구별하기 위해 쉼표가 필요합니다. 일반적으로 퍼센트 값만 사용하는 것이 좋습니다. 슬라이드의 나머지 부분과 wiki 페이지의 슬라이드 배경 미리보기를 조정하면 "실제"미리보기가 표시됩니다.)
  - 배경 이미지 반복은 키워드`repeat`에 의해 인식됩니다 (Reveal.js에서는 no-repeat가 기본값 임).
  - 배경 전환 : 접두어 `bg-`뒤에 `none`,`fade`,`slide`,`convex`,`concave` 또는`zoom`이 옵니다.
  - 슬라이드 전환 : `none`, `fade`, `slide`, `convex`, `concave` 또는 `zoom` 뒤에 `-in` 또는 `-out` 를 붙여서 슬라이드에 다른 전환을 설정합니다.
  - 전환 속도 : `default`, `fast`, `slow`
- `---->>`는 수직 (중첩 된) 슬라이드를위한 새로운 슬라이드 컨테이너와 주어진 옵션을 가진 새로운 슬라이드를 엽니 다. 예 :`---- red zoom ---->>`
- 다음 `---->>`는 암시 적으로 이전 컨테이너(또는 슬라이드)를 닫습니다.
- 기술적 세부 사항:
  - 렌더링에서 슬라이드 모드가 "headers driven"에서 "special horizontal rule driven"으로 변경됩니다.이 모드에서는 헤더가 더 이상 슬라이드 변경 적용되지 않습니다.
  - 이 대체 슬라이드 표시기로 전체 프리젠테이션을 물론 만들 수 있습니다.
  - 이 슬라이드 모드에서 나가려면 슬라이드 또는 컨테이너를 명시 적으로 닫을 방법이 필요합니다.
  - `<<----`는 슬라이드 컨테이너를 닫습니다.
  - `<----`는 슬라이드를 닫습니다

```
`-` 네 개가 이어져 있습니다.
컨테이너가 슬라이드를 포함하는 관계입니다.
```

## 바닥글 Footers

때로는 모든 페이지에 대해 꼬리말을 원할 수도 있습니다. 이 꼬리말에는 회사의 로고 또는 이와 유사한 내용이 포함되어있을 수 있습니다. 꼬리말은 dokuwiki 플러그인 "wrap"을 사용하여 가장 편리하게 추가 할 수 있습니다. 문서 첫 부분 즉 첫 번째 제목 앞에있는 `~~NOCACHE~~` 또는 `~~REVEAL~~` 다음 블록에 각 페이지의 바닥 글을 가져 오려면

```
<wrap footer>Footer content here.</wrap>
```

이렇게하면 모든 단일 페이지에 바닥 글이 삽입됩니다. 특정 페이지 위치에서 바닥 글을 사라지게 하려는 경우 해당 페이지의 표제 전에 `{{no-footer}}`를 삽입합니다. 예를 들어

```
{{no-footer}}
===== my heading=====

바닥글이 없는 슬라이드


{{no-footer}}
{{background>:images:image1.png}}
===== my heading=====

Slide without footer and with background


{{background>:images:image1.png no-footer}}
===== my heading=====

no-footer as option in background definition


---- no-footer ---->

Slide with alternative slide indicator

---->

Next slide with footer and stop alternative
slide indicator mode

<----
```

## 발표자 노트 Speaker notes

- https://github.com/hakimel/reveal.js#speaker-notes
- 키워드: <notes> (매개 변수 없음)
- 위키 페이지에서 변경하지 않음
- 슬라이드 쇼에서 내용은 <aside class = "notes"> 및 -
보이지 않는 (발표자 노트에만 표시됨 - 단축키 s)
- 메모의 목록은 목록이 보이지 않고 명백한 효과없이 각 항목에 대해 다음 키를 눌러야하기 때문에 항상 증분이 아닙니다.

Example:

```
<notes>
- your content
- here
</notes>
```

## 단편 Fragments

- https://github.com/hakimel/reveal.js#fragments
- 인라인 사용을 위한 `<fragment>` (지원되는 서식 및 대체 만)
- 모든 Wiki 내용에 대한 `<fragment-block>`
- 전역 옵션을 겹쳐 쓰려면 `<fragment-list>` build_all_lists (false 인 경우)
- 글로벌 옵션을 덮어 쓰기위한 `<no-fragment-list>` build_all_lists (true 인 경우)
- 가능한 경우 스타일 및 색인 지원 - example_presentation.dokuwiki 및 http://lab.hakim.se/reveal-js/#/7/1 참조

Example:

```
<fragment>Hit the next arrow...</fragment>

<fragment>... to step through ...</fragment>

<fragment>... a</fragment> <fragment>fragmented</fragment> <fragment>slide.</fragment>
```

## PDF 출력

프레젠테이션을 PDF로 내보낼 수 있습니다. 이렇게하려면 URL에 `&print-pdf`를 추가하십시오. 페이지를 편집 할 수 있으면 슬라이드 쇼 시작 버튼 아래에 PDF 내보내기 링크가 렌더링됩니다.

예를 들어 DokuWiki reveal.js 프리젠 테이션의 URL이 대개

```
http://example-dokuwiki.com/doku.php?do=export_revealjs&id=example:page
```

브라우저의 주소 표시 줄에서 이를 수동으로 변경 해야합니다.

```
http://example-dokuwiki.com/doku.php?do=export_revealjs&id=example:page&print-pdf
```

그 후 프리젠테이션은 브라우저에서 이상하게 보이지만 브라우저의 인쇄 기능을 통해 인쇄 할 수 있습니다. 공식적으로 Chromium 및 Chrome 만 PDF 내보내기 용으로 지원됩니다. Reveal.js PDF 내보내기 문서도 확인하십시오.
