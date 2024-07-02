---
title: 'cpu 100% 프로세스 kill'
date: 2024-07-02
tags: [cpu, kill, process]
categories:
  - OS
  - Ubuntu
---

## top 명령어로 CPU 사용률 확인하기

```shell
top -bn1 | grep "Cpu(s)" | awk '{printf("CPU 사용률 : %.1f%%\n", 100 - $8)}'
```

실행 결과
CPU usage : 100%

## idle CPU 확인하기

```shell
top -bn1 | grep "Cpu(s)" | awk '{printf($8)}'
```

## bash 스크립트 작성

CPU 사용률이 90% 이상이라면 `chromium` 프로세스를 모두 kill 한다.

```shell
#!/bin/bash

idle=$(top -bn1 | grep "Cpu(s)" | awk '{printf($8)}')
echo "$idle"

if (( $(echo "$idle < 10.0" | bc -l) ))
then
   kill $(ps -e | grep "chromium" | awk '{print $1}' )
fi
```

## 참고

- https://www.infracody.com/2023/09/3-ways-to-check-linux-cpu-usage.html
