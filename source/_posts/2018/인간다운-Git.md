---
title: "[책] 인간다운 Git"
date: 2018.05.22
tags: [book, git]
categories:
  - Programming
---

# [책] 인간다운 Git

[![](http://image.yes24.com/goods/58214924/147x215)<br>인간다운 Git <br>데이비드 디마리 저/이태상 역](http://blog.yes24.com/lib/adon/View.aspx?blogid=9654534&goodsno=58214924&idx=25604&ADON_TYPE=B&regs=b)

## 버전 관리의 요소

버전 관리 시스템(Version Control System) `VCS`

어떤 작업물의 최종본만 갖고 있는 것이 아니라 그 이전 각 수정본을 모두 보유함으로써, 필요할 때 이전 버전을 참고하거나 그 버전으로 되돌릴 수 있게 하자

## 병합 충돌 (merge conflict)

포함하고자 하는 변경 사항을 먼저 스테이징하고 그 다음에 커밋을 하는 것은 마찬가지다

Git 은 충돌이 있는 각 파일에 충돌마커(`conflict marker`)라고 하는 불가사의하고 난해한 표식을 넣는다.

```shell
$ git merge master
```

꺾쇠괄호가 연속으로 있는 라인이 충돌 마커이며, 두 충돌 마커 사이가 충돌 영역이다.

```shell
$ git add about.html
$ git commit -m "merge branch"
```

# 리모트

`리모트 저장소(remote repository)`는 내 컴퓨터가 아닌 원격에 있는 git 프로젝트의 사본을 말한다.

## GitHub

실전에서 대부분의 팀은 GitHub로 호스팅하는 하나의 공유 사본을 주된 저장소로 URL며, 이를 git의 관용어로 오리진`origin`이라고 한다.

마스터 브랜치를 `주된`브랜치로 할 것인지가 사용자에게 달렸듯, 오리진 리모트 역시 마찬가지다.

```shell
$ git push <remote-name> <branch-name>
```

## 리모트 추가

```shell
$ git remote add origin <remote-repository>
$ git remote
origin
```

## SSH(Secure Shell)

오늘날의 오픈소스 생태계에 있어서 SSH 프로토콜의 유용성이 제한을 받는 한 가지 단점이 있다.

SSH는 `프라이빗 저장소`에만 사용할 수 있다는 점이다.

## Git 프로토콜

git 프로토콜은 git에만 있는 유일무이한 프로토콜이지만, 주로 읽기 전용이라는 이유로 오늘날에는 잘 사용되진 않는다. `HTTPS`가 더 나은 선택이다.

## 리모트 브랜치로 작업하기

```shell
$ git push origin new-homepage
$ git pull origin new-homepage
```

## 리모트의 병합 충돌

```shell
$ git pull origin master
$ git add -A
$ git commit -m "Merge origin"
```

## 푸시 거부

git의 어떤 장난에도 놀아나고 싶지 않다면, 항상 푸시하기 전 먼저 풀`pull`을 해서 로컬 사본을 최신으로 만들기 바란다.

변경 사항을 pull하는 것이 문제가 되는 경우는 거의 없으며 오히려 많은 이득을 얻는다.

## 브랜치 추적

```shell
$ git push -u origin new-homepage
```

추적 관계를 설정하는 가장 간단한 방법은 `git push`를 할 때 `--set-upstream`(또는 -u) 옵션을 추가하는 것이다.

## 기존 브랜치 체크아웃

```shell
$ git fetch origin
```

기본적인 `git branch` 명령은 로컬 사본에 존재하는 브랜치 목록을 보여주지만 `--remote`(-r) 옵션을 추가하면 git이 알고 있는 모든 리모트 브랜치를 보여준다.

```shell
$ git branch --remote
```

# 히스토리

## 로그보기

```shell
$ git log
```

`--pretty` 옵션을 사용하면 미리 정이된 다양한 형식 중 하나를 선택하거나, 사용자가 원하는 형식을 지정할 수 있게 한다.

```shell
$ git log --pretty=oneline
```

new-homepage 브랜치의 히스토리를 보여달라는 명령이다.

```shell
$ git log new-homepage
```

master 와의 병합 전까지 new-homepage에 추가됐던 모든 커밋을 훑어보기 쉽게 확인하는 명령

```shell
$ git log --oneline master..new-homepage
```

'heroku'라는 단어가 포함됐으며 Gemfile이라는 파일을 변경시킨 커밋만 필터링

```shell
$ git log --author=AUTHOR --grep=heroku --oneline Gemfile
```

## 커밋 메시지

연계 기능을 이용하려면 먼저 원하는 에디터를 git에 설정해야 한다.

```shell
$ git config --global core.editor "atom --wait"
```

커밋 메시지의 목적은 변경 사항을 요약하는 것이다. 이 커밋에서 변경된 내용을 간결하고 명쾌하게 설명하는 메시지를 작성해야 한다.

내가 어떤 일을 했고 이게 내가 한 일이다 라는 식의 훨씬 쉬운 얘기로 설명할 수 있다.

```shell
$ git commit -m "Update homepage for lunch"
$ git commit -m "Fix typo in screen.css"
```

## 커밋 비교

```shell
$ git diff
```

저장소의 현재 헤드 커밋(HEAD)과 그 전 커밋(HEAD~1)의 차이를 보기 위한 명령어

```shell
$ git diff --stat HEAD~1
```

통계를 보는 일이 의미 있다고 생각되면 git log 명령에 `--stat` 옵션을 붙여 사용할 수 있다.

```shell
$ git log --stat --pretty=format:"%h (%an) %s" HEAD~1..
```

## 태그

`경량(lightweight)`: 마치 브랜치와 비슷하게 단지 커밋을 가리키는 이름으로만 저장

```shell
$ git tag <tag-name> <commit-id>
```

`주석(annotated)`: 커밋 메시지와 같이 메시지를 추가로 포함

```shell
$ git tag <tag-name> -a -m "branch release"
```

브랜치와 마찬가지로 태그도 git push를 사용해 리모트로 공유할 수 있다.

```shell
$ git push <tag-name>
```

## 방향성 비순환 그래프

`DAG(directed acyclic graph)`는 데이터 구조의 한 종류다. 각 개별 노드가 다른 노드를 가리키고 그런 참조들이 정보의 사슬을 구축하며, 작업이 진행되면서 커밋이 추가됨에 따라 나무 뿌리처럼 퍼져 나가는 모양으로 계속 자라는 구조다.

## 명령어 목록

- git config [--global] <key> <value> : git 을 설정하는 명령
- git init : 현재 작업 사본 안에서 새 git 프로젝트를 만든다.
- git clone <url> [directory] : 해당 URL에 있는 git 프로젝트를 로컬 컴퓨터의 새 디렉터리로 복제
- git status [-s] \[path/to/thing]
- git add [--all] filename.txt
- git rm folder/filename.txt
- git mv oldpath.txt newpath.txt
- git reset filename.txt : 스테이징된 filename.txt를 git reset을 사용해 스테이징이 되지 않은 상태로 되돌린다.
- git commit [-a] \[-m "Your message"]
- git branch [-r|-a]
- git branch <branch-name> [<commit>]
- git checkout [-b] <branch-name-or-commit>
- git merge <other-branch>
- git remote add <name> <url>
- git remote rm <name>
- git push <remote-name> <branch-name>
- git pull <remote-name> <branch-name>
- git fetch <remote-name> : 리모트의 모든 사항을 로컬 사본으로 복사한다. git pull 명령을 실행하면 git fetch 작업은 자동으로 포함된다.
- git log [--oneline] \[--pretty] \[<branch-name-or-commit>]
- git diff [--stat] \[<branch-name-or-commit>]
- git tag [-a] \[-m] <tag-name> [<commit>]
- git tag [-d] <tag-name>
- git tag [-l]
- git push --tags <remote-name>

## 추천 Git 앱

- GitHub 데스크톱
- 타워(Tower)
- 소스트리(SourceTree)

## Git 호스팅 서비스

- GitHub
- Bitbucket
- Beanstalk

## 추가정보

- https://try.github.io/levels/1/challenges/1
