---
title: Gitbook 과 GitHub 연동
date: 2017-02-06 19:13:45
tags: [gitbook, github, integration]
categories:
- Service
- GitBook
---

# Gitbook 과 GitHub 연동

Gitbook 세팅화면에 Github 메뉴가 생겼습니다.

![](https://goo.gl/lXgaRA)

Github 저장소와 연결하면, Github에 소스를 올리면 해당 내용이 반영되서, Gitbooks에서 보여지게 됩니다.

![](https://goo.gl/S6WcVA)

## 문제 발생

Gitbook 에서 유투브 플러그인 사용하는데 이 부분이 문제가 되었습니다.

```
{% youtube %}https://www.youtube.com/watch?v=fHyTA-UIcqs{% endyoutube %}
```

이때 `{% youtube %}` 를 인식할 수 없어 계속해서 오류가 발생합니다.

```sh
 Your site is having problems building: The tag youtube on line 5 in doc/ac1c_bc1c_c790_ac00_ac16_cd94_c5b4_c57c_d560_9_ac0.md is not a recognized Liquid tag. For more information, see https://help.github.com/articles/page-build-failed-unknown-tag-error/.
```

## GiHub Pages 사용 중지

프로젝트 → Settings 를 선택합니다.

![](https://goo.gl/EbwjHE)

GiHub Pages 설정중에서 Source 를 `None` 으로 변경합니다.

![](https://goo.gl/fdTyeO)

변경 이후 꼭 `Save` 버튼을 클릭합니다.
