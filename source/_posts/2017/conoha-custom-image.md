---
title: 'ConoHa 커스텀 이미지 올리기'
date: 2017-01-12 19:13:45
tags: [conoha, vps, cloud]
categories:
- Service
- Cloud
---

# ConoHa 커스텀 이미지 올리기

## conoha 콘솔

Ubuntu 64비트로 설치 하였습니다. root 접속

`Login incorrect` 발생시, `텍스트 전송`을 통해서 암호를 입력합니다.

### vsftpd 패키지 설치

```sh
apt-get install vsftpd
```

### 사용자 추가

```sh
adduser ftpuser
```

### conf 수정

```sh
nano /etc/vsftpd.conf
```

write_enable=YES 부분의 주석을 삭제한다.

### 서비스 재시작
```sh
service vsftpd restart
```
### 파일 전송

FileZilla 등 프로그램을 통해서 ftp 접속

ISO 파일을 전송한다.

## web server 설치

### update & upgrade

```sh
apt-get update
apt-get upgrade
```

### Apache2 설치

```sh
apt-get install apache2
```

아이피로 접속해서 웹 페이지가 정상적으로 나오는지 확인합니다.

전송한 ISO 파일을 이동한다.

```sh
mv Windows.ISO /var/www/html
```

iso 파일의 권한을 변경한다

```sh
chmod o+r Windows.iso
```

## conoha-iso 다운로드

기본 정보들은 모두 환경 변수에 저장해 놓는다.


```sh
conoha-iso.exe download -i http://domain/Windows.iso

time="2017-04-20T23:45:24+09:00" level=info msg="Download request was accepted."
```

VPS 종료 후, 이미지 삽입

```sh
conoha-iso insert
```


## 출처
- http://goproprada.tistory.com/189
- https://blog.lael.be/post/73
- https://github.com/hironobu-s/conoha-iso
- http://badspell.tistory.com/13
