---
title: "맥에서 비주얼스튜디오 코드 실행하기 Run vscode in Mac Shell"
date: 2018.01.27
tags: [mac, vscode, shell]
categories:
- Application
- IDE
---
# 맥에서 비주얼스튜디오 코드 실행하기 Run vscode in Mac Shell

비주얼스튜디오 코드를 실행하기 위해서 `code .`를 입력하였으나 실행이 되지 않는다.

```sh
➜ code .
zsh: command not found: code
```

위와 같이 찾을 수 없다는 에러가 나오면 환경변수 path 비주얼스튜디오 코드를 추가해야 합니다.

먼저 비주얼스튜디오 코드를 실행합니다.

`Command + Shift + P`를 눌러 팔레트를 실행합니다.

`Shell Command : Install code in PATH`를 검색하여 실행합니다.

![](https://i.stack.imgur.com/Ng886.png)

이후에는 쉘에서, `code .`를 입력하여 vscode 를 실행할 수 있습니다.



## Refs
- https://stackoverflow.com/questions/30065227/run-open-vscode-from-mac-terminal







