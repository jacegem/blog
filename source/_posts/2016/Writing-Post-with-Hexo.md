title: Writing Post with Hexo
tags: [ writing, post, hexo, resophnotes, draft, publish, deploy, blog ]
date: 2016-01-07 14:10:24

---


[hexo] Writing Post with Hexo

HEXO로 포스팅하기

## 메모하기

`ResophNotes`를 사용해서 초안을 작성합니다. markdown 문법을 지원하기 때문에 어느 정도는 확인을 할 수 있습니다.(hexo 마크다운 문법과 정확히 일치하지는 않습니다.) 또는 메모장과 같은 다른 텍스트 편집기를 이용해서 작성하면 됩니다.

![ResophNotes](https://goo.gl/c1VD6g)


## 초안 생성

hexo 에서 초안 문서를 생성합니다.

> hexo new draft "title"

해당 문서 draft.md 에서 지정해 놓은 형식으로 `_draft` 폴더에 생성됩니다. 새로 생성되는 파일의 레이아웃에 대한 정의는 `scaffold`폴더에 있습니다.

> scaffold/draft.md
> source/_draft/title.md

새로 생성된 문서로 텍스트 편집기에서 작성한 내용을 복사합니다.

## 로컬에서 확인
파일을 생성하고 로컬 서버를 실행하여 초안 문서를 확인합니다

> hexo generate
> hexo serve --draft

옵션인 `--draft`를 꼭 붙여서 실행해야 draft 문서도 확인할 수 있습니다. 브라우저를 통하여 `localhost:4000`으로 접속합니다. 화면을 살펴보면서 내용을 수정합니다.

> localhost:4000

root 경로를 `/blog`로 설정한 경우에는 localhost:4000으로 접속하면 /blog로 접속됩니다.

## publish

명령어를 통하여 발행합니다.

> hexo publish "title"

발행을 하면 _draft 폴더에 있는 파일은 post 폴더로 이동됩니다. 

> source/_post/title.md

다시 한번 내용을 수정하면서 문법을 확인합니다. 문법 확인으로 [한국어 맞춤법/문법 검사기](http://speller.cs.pusan.ac.kr/PnuSpellerISAPI_201504/Default.htm)를 사용합니다.

## deploy
배포할 파일을 생성한 다음 배포명령어를 실행하여 포스팅합니다.

> hexo generate
> hexo deploy

배포한 이후, 잠깐 시간이 지난 뒤에 수정된 내용이 적용된 것을 확인할 수 있습니다.

## clean

생성한 배포 파일을 삭제할 경우에는 `clean` 옵션을 사용합니다.

> hexo clean


## 단축 명령어

hexo new draft "title"
> hexo n draft "title"

hexo generate
> hexo g

hexo serve --draft
> hexo s --draft

hexo publish "title"
> hexo p "title"

hexo deploy
> hexo d

hexo generate
hexo deploy
> hexo d -g