---
title: 'lightsail 에 portainer 설치하기'
date: 2024-06-24
tags: [aws, lightsail, portainer]
categories:
  - Service
  - AWS
---

## lightsail 인스턴스 추가

https://lightsail.aws.amazon.com/ls/webapp/home/instances 에서 `인스턴스 생성` 버튼 누르기
Linux/Unix 에서 Ubuntu 선택
[[듀얼 스택]] 선택 ➡️ 월별 $12 선택
리소스 이름 입력 후 `인스턴스 생성` 버튼 누르기
![](https://i.imgur.com/yrHhPia.png){:height 443, :width 590}

## ssh 연결

IP 확인
ssh 기본 키 다운로드 후 연결

```shell
ssh -i Lightsail_Default_Key.pem ubuntu@<IP>
```

## 도커 설치

참고: https://docs.docker.com/engine/install/ubuntu/

APT 소스에 레포지토리 추가

```shell
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

도커 설치

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

설치 확인

```shell
docker version
```

실행 확인

```shell
sudo docker run hello-world
```

## Portainer 설치

참고: https://docs.portainer.io/start/install-ce/server/swarm/linux

portainer 스택 다운로드

```shell
curl -L https://downloads.portainer.io/ce2-20/portainer-agent-stack.yml -o portainer-agent-stack.yml
```

도커 스웜 초기화

```shell
sudo docker swarm init
```

스택 Deploy

```shell
sudo docker stack deploy -c portainer-agent-stack.yml portainer
```

## Portainer 로그인

Lightsail > Network 에서 9443 포트 추가
`https://<IP>:9443` 으로 연결
![](https://i.imgur.com/Ql1PqYi.png){:height 398, :width 445}
username, password 생성

## Nginx Proxy Manager 스택 추가

Home > Primary > Stacks > `Add Stack` 버튼 누르기
Name 에 `nginx-proxy-manager` 입력
Web editor 에 다음 내용 입력

```shell
version: '3.8'
services:
  nginx-proxy-manager:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - proxy-data:/data
      - proxy-letsencrypt:/etc/letsencrypt
    networks:
      - service-network
      - portainer_agent_network

volumes:
  proxy-data:
  proxy-letsencrypt:

networks:
  service-network:
    external:
      name: service-network
  portainer_agent_network:
    external:
      name: portainer_agent_network
```

Actions > `Deploy the Stack` 버튼 누르기
