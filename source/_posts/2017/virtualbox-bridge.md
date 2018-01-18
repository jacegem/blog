---
title: '버추얼박스에서 브리지끼리 통신하기'
date: 2017-04-05 19:13:45
tags: [javascript, vue, model]
categories:
- Application
- VM
---

## 버추얼박스에서 브리지끼리 통신하기

VirtualBox Bridge to Bridge

|  | VM ↔ Host | VM1 ↔ VM2 | VM → Internet | VM ← Internet |
|-----|--------|------       |--------        |----             |
| Host-only	| + | + | –	| – |
| Internal	| –	| +	| –	| – |
| Bridged	| +	| +	| +	| + |
| NAT	|–	|–	|+	|Port forwarding|
| NAT Network	|–	|+	|+	|Port forwarding|

`+` 를 통신이 되는 것을 나타냅니다. `-`는 통신 불가능입니다.


### VM 이 Bridge 상태에서 서로 통신을 하기 위해서는 서로 다른 어뎁터를 사용해야 합니다.

1번 서버는 `어뎁터1`을 사용

![](https://goo.gl/JssWKZ)

2번 서버는 `어뎁터2`를 사용

![](https://goo.gl/WfhyBq)

### 출처

- https://www.virtualbox.org/manual/ch06.html
