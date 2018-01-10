title: '[hexo] change theme'
date: 2016-01-04 22:37:58
tags: [hexo, theme]

---
Hexo 테마 변경

## theme 변경
[여기](https://hexo.io/themes/)에서 테마를 선택한 다음에 다운로드 받습니다. 

hexo blog 설치 경로 밑에 `themes` 폴더가 있고, 그 밑에 해당 파일을 복사합니다. (압축 파일이면 풀어 놓습니다.) 해당 폴더에는 기본 테마인 `landscape`가 있습니다.

복사해 넣은 테마로 적용하기 위해서 `_config.yml`를 수정합니다.

```XML
#theme: landscape
theme: bootstrap
```
기존의 landscape 는 주석처리 하고, bootstrap을 추가합니다.




## 관련글
* [install-powerful-blog-framework-hexo](http://jacegem.github.io/blog/2016/01/hexo-install-powerful-blog-framework-hexo/)
* [hexo-change-theme](http://jacegem.github.io/blog/2016/01/hexo-change-theme/)
* [hexo-add-adsense-to-hexo](http://jacegem.github.io/blog/2016/01/hexo-add-adsense-to-hexo/)
* [hexo-add-tags-and-categories](http://jacegem.github.io/blog/2016/01/hexo-add-tags-and-categories/)