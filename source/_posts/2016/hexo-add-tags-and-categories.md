title: '[hexo] add tags and categories'
date: 2016-01-05 20:48:27
tags: [tag, category, hexo]

---
태그, 카테고리 추가하기

## tag 추가
`hexo new "title"` 로 문서를 생성하면 기본 틀이 생성됩니다.

tag 입력은 한줄로 입력할 때에는 `tags: [tag1, tag2]`  이와 같이 입력하며 여러 줄로 입력할 때에는 아래와 같습니다. 
```markdown
tags:
	- tag1
	- tag2
```

## category 추가
category는 순서가 중요합니다.
```md
categories:
	- ca1
	- ca2
```
위와 같이 작성하면 카테고리를 지정한 순서대로 하위 카테고리로 설정이 되기 때문에 아래와 같은 구조로 생성 됩니다.

  * ca1
    * ca2


## 관련글
* [install-powerful-blog-framework-hexo](http://jacegem.github.io/blog/2016/01/hexo-install-powerful-blog-framework-hexo/)
* [hexo-change-theme](http://jacegem.github.io/blog/2016/01/hexo-change-theme/)
* [hexo-add-adsense-to-hexo](http://jacegem.github.io/blog/2016/01/hexo-add-adsense-to-hexo/)
* [hexo-add-tags-and-categories](http://jacegem.github.io/blog/2016/01/hexo-add-tags-and-categories/)
