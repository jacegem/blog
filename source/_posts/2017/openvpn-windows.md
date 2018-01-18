---
title: OpenVPN Windows 서버 및 클라이언트 설치
date: 2017-02-27
tags: [OpenVPN, Windows]
categories:
- Application
- VPN
---

# OpenVPN Windows 서버 및 클라이언트 설치

# 서버설정

## 다운로드 및 설치

OpenVPN 설치파일 다운로드 → 설치

- http://openvpn.net/index.php/open-source/downloads.html

easy-rsa-old 다운로드 → 압축해제

- https://github.com/OpenVPN/easy-rsa-old



## 커맨드 실행

cmd를 `관리자 모드`로 실행합니다.
windows 폴더에서 커맨드를 실행합니다.

### 설정파일 초기화

```shell
> init-config

copy vars.bat.sample vars.bat 1 file(s) copied.
```

## 환경변수 설정

```shell
> vars
```

만약 아래와 같은 에러가 발생하면 var.bat 스크립트를 수정합니다.

```shell
'#' is not recognized as an internal or external command,
operable program or batch file.
```

var.bat 파일을 에디터로 열어서 26번째 줄을 주석처리 합니다. 그리고 `vars`를 다시 실행합니다.

```shell
rem # Private key size
```

## 기존 설정 파일 삭제

```shell
> clean-all
```

실행을 하면 key 폴더 밑에 파일이 2개 생성됩니다.
- index.txt
- serial

## Root CA (Certificatie Authority) 생성

```shell
> build-ca
```

만약 openssl-1.0.0.cnf 파일이 없다는 에러 메시지가 나오면 해당 파일을 복사합니다.

```shell
No such file or directory:bss_file.c:175:fopen('openssl-1.0.0.cnf','rb')
```

openssl-1.0.0.cnf 파일은 다운 받은 easy-rsa\2.0\openssl-1.0.0.cnf 에 있습니다.

정상적으로 실행되면 입력값을 물어봅니다.

[Common Name]에서 값을 입력합니다.

2개 파일이 생성됩니다.
- ca.crt
- ca.key

## 서버키 생성

```shell
> build-key-server server
```

[Common Name] 값을 입력합니다.

3개 파일이 생성됩니다.

- server.crt
- server.csr
- server.key

## DH Parameter 생성

```shell
> build-dh
```

keys 폴더 밑에 `dh4096.pem` 파일이 생성됩니다.

## ta키 생성

keys 폴더로 이동하여 명령을 실행합니다.

```shell
> openvpn --genkey --secret ta.key
```

`ta.key` 파일이 생성됩니다.

## 인증서 및 키파일 복사

c:\Program Files\OpenVPN\config 폴더에 다음 파일을 복사합니다.

- ca.crt
- ca.key
- dh4096.pem
- ta.key
- server.crt
- server.key

## 서버 config파일 설정


c:\Program Files\OpenVPN\sample-config 폴더에서 server.ovpn 파일을 c:\Program Files\OpenVPN\config 폴더로 복사합니다.

복사한 server.ovpn 파일을 편집기로 열어서 다음 항목을 찾아(Ctrl+F) 필요한 값을 변경합니다.

- port : VPN서비스를 위한 포트
- proto : TCP 또는 UDP를 선택
- dh dh4096.pem
- push "redirect-gateway def1 bypass-dhcp"
- push "dhcp-option DNS 168.126.63.1"
- push "dhcp-option DNS 8.8.8.8"
- client-to-client
- tls-auth ta.key 0 # This file is secret
- cipher AES-256-CBC


## 서버 실행

커맨드창에서 c:\Program Files\OpenVPN\config 폴더로 이동하여 명령어를 실행합니다.

```shell
> openvpn server.ovpn
```

```
Options error: --dh fails with 'dh2048.pem': No such file or directory
Options error: Please correct these errors.
Use --help for more information.
```

에러 발생시 dh2048.pem 를 `dh dh4096.pem` 으로 변경합니다.

### 서비스 등록 확인

```shell
> services.msc
```

# 클라이언트 설정

## 서버에서 클라이언트 인증서 및 키 생성

서버의 easy-rsa 폴더에서 명령어를 실행합니다.

```shell
> build-key client1
```
[Common Name] 값을 입력합니다.

## 인증서 및 키파일 복사

생성된 인증서 및 키 파일을 클라이언트의 config 폴더에 복사합니다.

- ca.crt
- ca.key
- client.key
- client.crt
- ta.key

## 클라이언트 config파일 설정

클라이언트의 c:\Program Files\OpenVPN\sample-config 폴더에서 `client.ovpn` 파일을 config 폴더로 복사합니다.

나중에 혹시 있을지 모르는 복수의 접속을 위해 client1.ovpn 으로 파일명을 변경합니다.

복사한 client1.ovpn 파일을 편집기로 열어서 다음 항목을 찾아(Ctrl+F) 필요한 값을 변경합니다.

- proto : TCP 또는 UDP를 선택(서버의 설정에 맞춘다)
- remote : 서버의 IP. 형식은 [IP Port] 의 형식임(콜론이 없음에 주의).
- cert : 인증서 파일. client1.crt로 변경합니다
- key : 키 파일. client1.key로 변경합니다
- tls-auth ta.key 0 : 맨 앞의 세미콜론을 제거합니다
- cipher : 맨 앞의 세미콜론을 제거합니다




## 출처
- http://takuma99.tistory.com/134 [Live without regrets!]
- http://haebi.kr/entry/OpenVPN-설치-및-사용기
