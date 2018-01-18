---
title: Synology에서 자동 다운로드된 디렉토리 삭제
date: 2017-03-08
tags: [synology, 디렉토리, 삭제]
categories:
- Application
- NAS
---


# Synology에서 자동 다운로드된 디렉토리 삭제


사용하고 있는 삭제 스케쥴에, 빈 디렉토리를 지우는 명령어를 추가합니다.


```shell
# delete old files
find /volume2/video/ -mtime +1000 -exec rm {} \;
find "/volume2/video/예능" -mtime +60 -exec rm {} \;
find "/volume2/video/다큐" -mtime +90 -exec rm {} \;

# 빈 디렉토리 삭제
find /volume2/video/ -empty -type d -delete
```


## 출처
- https://askubuntu.com/questions/73709/how-do-i-delete-all-empty-directories-in-a-directory-from-the-command-line
