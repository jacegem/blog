layout: "post"
title: "UbuntuOpenStack"
date: "2016-12-27 14:33"

------------------

우분투 홈페이지에서 ISO 파일 다운로드

VirtualBox 로 설치

![language](https://goo.gl/8NCMIo)

![](https://goo.gl/XCzjMo)

[instal-guide-ubuntu](http://docs.openstack.org/liberty/ko_KR/install-guide-ubuntu/overview.html)

https://www.ubuntu.com/download/cloud#instructions

Installation instructions
=========================

1.	Set up your hardware -----------------------

Install [Ubuntu Server 14.04 LTS](http://releases.ubuntu.com/14.04/) on the machine designated to be the `MAAS` server.

You need to setup a private network with all machines plugged in and enough IP addresses available for all physical and virtual machines you plan to run. This network must not have a DHCP server: MAAS will fill in that role.

For the simplest topology, connect the second NIC of the dual-nic machines(s) to the same network

1.	Add required repositories ----------------------------

```shell
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:cloud-installer/stable
sudo apt-get update
```
