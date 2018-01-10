title: '[highlight.js] Syntax Highlight - highlight.js'
tags:
  - highlight
  - syntax
  - js
date: 2016-01-07 10:32:37
---

# Syntax Highlight - highlight.js

마크다운 편집기로 문서를 작성하고 html코드를 복사하여 포스팅 하고 있습니다. 블로그에서 소스코드에 대해 `syntax highlight`를 적용하기 위해 hightlight.js 를 적용하였습니다.

```java
class Hello
{
    public static void main(String args[])
    {
          System.out.println("안녕하세요?");
    }
}
```

위의 자바 소스를 생성하면 아래와 같은 html 코드가 생성됩니다.

```html
<pre><code class="java">class Hello
  {
  	public static void main(String args[])
    {
          System.out.println(&quot;안녕하세요?&quot;);
    }
}
</code></pre>
```
이 코드를 가지고 highlight 할 수 있도록 설정합니다. 


## highlight.js
https://highlightjs.org/ 에 접속합니다. 

![](https://goo.gl/a6KE2c)

해당 사이트에 접속한 후에 `Get version` 버튼을 클릭합니다.

두개의 CDN 주소를 확인할 수 있습니다. 둘 중 하나만 복사해서 적용합니다.

#### cdnjs
```js
<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.0.0/styles/default.min.css">
<script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.0.0/highlight.min.js"></script>
```

#### jsdelivr
```js
<link rel="stylesheet" href="//cdn.jsdelivr.net/highlight.js/9.0.0/styles/default.min.css">
<script src="//cdn.jsdelivr.net/highlight.js/9.0.0/highlight.min.js"></script>
```

티스토리 관리에서, `꾸미기 > HTML/CSS 편집` 메뉴를 선택합니다.

위에서 복사한 CDN 주소와 함께 `<script>hljs.initHighlightingOnLoad();</script>` 코드를 `<body>` 태그 윗 부분에 추가합니다.

```js
<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.0.0/styles/monokai-sublime.min.css">
<script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.0.0/highlight.min.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
```
위의 코드를 복사해서 붙여 넣습니다.

![티스토리설정화면](https://goo.gl/qPklmw)

해당 소스를 붙여넣은 다음에 화면을 캡쳐하였습니다. 

사용할 수 있는 스타일은 [이곳](https://github.com/isagalaev/highlight.js/tree/master/src/styles)에서 확인 가능합니다.

![](https://goo.gl/a6KE2c)

좌측 하단에 보이는 `Language`, `style` 을 클릭하면 언어및, 스타일이 변경되면서 모습을 확인할 수 있습니다.

각각 스타일 및 언어에 대해서 어떠한 형태로 나오는지는 메인화면에서 확인할 수 있습니다. 또는 [데모사이트](https://highlightjs.org/static/demo/)에서 확인 가능합니다. 


* 141 languages and 65 styles (live demo)
* automatic language detection
* multi-language code highlighting
* available for node.js
* works with any markup
* compatible with any js framework


### 스타일 목록
https://github.com/isagalaev/highlight.js/tree/master/src/styles
  * agate.min.css
  * androidstudio.min.css
  * arta.min.css
  * ascetic.min.css
  * atelier-cave-dark.min.css
  * atelier-cave-light.min.css
  * atelier-dune-dark.min.css
  * atelier-dune-light.min.css
  * atelier-estuary-dark.min.css
  * atelier-estuary-light.min.css
  * atelier-forest-dark.min.css
  * atelier-forest-light.min.css
  * atelier-heath-dark.min.css
  * atelier-heath-light.min.css
  * atelier-lakeside-dark.min.css
  * atelier-lakeside-light.min.css
  * atelier-plateau-dark.min.css
  * atelier-plateau-light.min.css
  * atelier-savanna-dark.min.css
  * atelier-savanna-light.min.css
  * atelier-seaside-dark.min.css
  * atelier-seaside-light.min.css
  * atelier-sulphurpool-dark.min.css
  * atelier-sulphurpool-light.min.css
  * brown-paper.min.css
  * brown-papersq.png
  * codepen-embed.min.css
  * color-brewer.min.css
  * dark.min.css
  * darkula.min.css
  * default.min.css
  * docco.min.css
  * far.min.css
  * foundation.min.css
  * github-gist.min.css
  * github.min.css
  * googlecode.min.css
  * grayscale.min.css
  * hopscotch.min.css
  * hybrid.min.css
  * idea.min.css
  * ir-black.min.css
  * kimbie.dark.min.css
  * kimbie.light.min.css
  * magula.min.css
  * mono-blue.min.css
  * monokai-sublime.min.css
  * monokai.min.css
  * obsidian.min.css
  * paraiso-dark.min.css
  * paraiso-light.min.css
  * pojoaque.min.css
  * pojoaque.jpg
  * railscasts.min.css
  * rainbow.min.css
  * school-book.min.css
  * school-book.png
  * solarized-dark.min.css
  * solarized-light.min.css
  * sunburst.min.css
  * tomorrow-night-blue.min.css
  * tomorrow-night-bright.min.css
  * tomorrow-night-eighties.min.css
  * tomorrow-night.min.css
  * tomorrow.min.css
  * vs.min.css
  * xcode.min.css
  * zenburn.min.css

### CSS classes reference
* http://highlightjs.readthedocs.org/en/latest/css-classes-reference.html


## 참고
* http://jsonobject.tistory.com/174
* http://gseok.tistory.com/entry/TistoryTip-%ED%8B%B0%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%97%90%EC%84%9C-code-highlighter-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0
* http://prismjs.com/