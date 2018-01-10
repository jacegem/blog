title: 'Unix/Linux Command df & du'
tags: [ unix, linux, command, df, du]
date: 2016-01-07 20:33:54

---

디스크용량을 확인하기 위해 df, du 명령어를 사용합니다.

## df
> Reports information about space on file systems.

파일 시스템 용량에 관한 정보를 제공합니다. 파일시스템, 블록, 사용가능, %사용, Iused, %Iused, 마운트 위치 정보를 제공합니다.

	파일 시스템     GB 블록  사용가능  %사용    Iused %Iused 마운트 위치
	/dev/hd4         330.00      0.00   100%   184939    93% /
	/dev/hd2          20.00     17.53    13%    46301     2% /usr
	/dev/hd9var        5.00      4.90     2%     4581     1% /var

### 옵션
	df -m : 메가바이트 단위로 정보를 확인합니다.
    df -g : 기가바이트 단위로 정보를 확인합니다.


## du
> Summarizes disk usage.

디스크 사용량을 요약해서 보여줍니다.

	사용량	파일
	368     ./log/log.log.2015-02-01.log
	296     ./log/log.log.2015-02-02.log
	8       ./log/log.log.2015-02-03.log
    ...
    10608   .

### 옵션
	du -a : 파일단위로 디스크 사용량을 보여줍니다.
    du -s : 현재 디렉토리의 총 디스크 사용량을 보여줍니다.


