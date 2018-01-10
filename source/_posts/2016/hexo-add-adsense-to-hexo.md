title: '[hexo] add adsense to hexo'
date: 2016-01-05 13:12:26
tags: [ hexo, adsense, layout, ejs ]

---
[hexo] 애드센스 추가하기

## adsense 추가

`bootstrap` 테마를 기준으로 adsense를 추가해보도록 하겠습니다.

### 상단에 추가하는 방법

블로그 레이아웃의 전체적인 구조는 layout.ejs 파일을 통해서 확인할 수 있습니다. layout.ejs파일을 수정하여 상에 애드센스를 추가합니다. 

> theme > bootstrap > layout > layout.ejs

```xml
<div class="row">
    <div class="col-sm-8 blog-main">
      <!-- ad start -->
      <%- partial('_custom_ad/google-adsense') %>
      <!-- ad end -->
      <%- body %>
    </div>
    <div class="col-sm-3 col-sm-offset-1 blog-sidebar">
      <%- partial('_partial/sidebar', null, {cache: !config.relative_link}) %>
    </div>
</div>
```
중간에 있는 `<%- body %>` 이 부분이 본문이 표출되는 레이아웃 입니다. 따라서, `<%- body %>` 윗 부분에 애드센스를 추가합니다. 

```xml
<!-- ad start -->
<%- partial('_custom_ad/google-adsense') %>
<!-- ad end -->
```
위의 코드를 추가하였습니다.

실질적 동작을 위한 코드가 들어있는 파일을 추가합니다. 

> theme > bootstrap > layout > _custom_ad > google-adsense.ejs

해당 파일에 애드센스에서 제공하는 코드를 입력합니다. 각 계정 및 광고 크기마다 다른 코드가 생성됩니다.

```xml
<div>
<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- 반응형 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-[yours]"
     data-ad-slot="[yours]"
     data-ad-format="auto"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>
</div>
```

위의 코드는 `반응형`으로 생성하였기 때문에, 다른 위치에서도 이 파일을 사용하여 애드센스를 추가합니다.


### 하단에 추가하는 방법
하단도 상단과 마찬가지로 `<%- body %>` 부분 밑에 코드를 추가함으로써 삽입이 가능합니다. 이 경우에는 댓글 부분 밑에 추가되기 때문에, 이를 변경하기 위해서 삽입 코드의 위치를 변경합니다.

태그 밑 부분에 출력하기 위해서 tag.ejs 파일을 수정합니다.

> themes > bootstrap > layout > _partial > post > tag.ejs

`tag.ejs` 파일의 맨 밑에 삽입 코드를 추가합니다. 

```XML
<!-- ad start -->
<%- partial('_custom_ad/google-adsense') %>
<!-- ad end -->
```

### 사이드에 추가하는 방법
사이드에 추가하기 위해서 sidebar.ejs 파일을 수정합니다. 
`sidebar.ejs` 파일 맨 맽에 코드를 추가합니다. 

> themes > bootstrap > layout > _partial > sidebar.ejs

```XML
<!-- ad start -->
<%- partial('_custom_ad/google-adsense') %>
<!-- ad end -->
```

반응형 광고 크기를 사용하였기 때문에, 하나의 코드로 여러군데에서 사용하였습니다. 각각 다른 광고 크기를 사용해야 한다면 추가되는 파일을 따로 생성해서 추가하도록 코드를 변경해야 합니다.


## 관련글
* [install-powerful-blog-framework-hexo](http://jacegem.github.io/blog/2016/01/hexo-install-powerful-blog-framework-hexo/)
* [hexo-change-theme](http://jacegem.github.io/blog/2016/01/hexo-change-theme/)
* [hexo-add-adsense-to-hexo](http://jacegem.github.io/blog/2016/01/hexo-add-adsense-to-hexo/)
* [hexo-add-tags-and-categories](http://jacegem.github.io/blog/2016/01/hexo-add-tags-and-categories/)