title: '[hexo] install powerful blog framework hexo'
date: 2016-01-05 02:10:45
tags: [hexo, framework, blog, install, nodejs]

---
파워풀 블로그 프레임워크 HEXO 설치하기

## node.js 설치
[Git Bash](https://git-scm.com/)를 설치합니다.
[node js](https://nodejs.org/en/)를 설치합니다.

![](http://goo.gl/ERmzp0)

윈도우 방화벽 설정을 허용합니다.

![](http://goo.gl/NG423g)

버전을 확인합니다.
```bash
node -v
npm -v
```

hexo 프레임워크를 설치합니다.
```bash
npm install hexo-cli -g
npm install hexo-deployer-git --save
hexo -v
```

실행 결과
```
C:\Temp>npm install hexo-cli -g
npm WARN optional dep failed, continuing fsevents@1.0.6
C:\Users\[USER]\AppData\Roaming\npm\hexo -> C:\Users\[USER]\AppDat
a\Roaming\npm\node_modules\hexo-cli\bin\hexo
hexo-cli@0.2.0 C:\Users\[USER]\AppData\Roaming\npm\node_modules\hexo-cli
├── abbrev@1.0.7
├── minimist@1.2.0
├── bluebird@3.1.1
├── tildify@1.1.2 (os-homedir@1.0.1)
├── chalk@1.1.1 (supports-color@2.0.0, ansi-styles@2.1.0, escape-string-regexp@1.0.4, strip-ansi@3.0.0, has-ansi@2.0.0)
├── hexo-util@0.5.1 (html-entities@1.2.0, highlight.js@9.0.0, camel-case@1.2.2)
└── hexo-fs@0.1.5 (escape-string-regexp@1.0.4, graceful-fs@4.1.2, chokidar@1.4.2)

C:\Temp>npm install hexo-deployer-git --save
npm WARN optional dep failed, continuing fsevents@1.0.6
hexo-deployer-git@0.0.4 node_modules\hexo-deployer-git
├── chalk@0.5.1 (ansi-styles@1.1.0, escape-string-regexp@1.0.4, supports-color@0.2.0, strip-ansi@0.3.0, has-ansi@0.1.0)
├── moment@2.11.0
├── swig@1.4.2 (optimist@0.6.1, uglify-js@2.4.24)
├── hexo-fs@0.1.5 (escape-string-regexp@1.0.4, graceful-fs@4.1.2, bluebird@3.1.1, chokidar@1.4.2)
└── hexo-util@0.1.7 (ent@2.2.0, bluebird@2.10.2, highlight.js@8.9.1)

C:\Temp>hexo -v
hexo-cli: 0.2.0
os: Windows_NT 6.1.7601 win32 x64
http_parser: 2.5.0
node: 4.1.0
v8: 4.5.103.33
uv: 1.7.4
zlib: 1.2.8
ares: 1.10.1-DEV
modules: 46
openssl: 1.0.2d
```


## hexo 설치

블로그로 사용할 디렉토리를 생성합니다. 

```bash
mkdir blog
cd blog
```

블로그를 초기화 합니다.
```bash
hexo init
npm install
hexo serve
```

`http://localhost:4000/` 로 접속하면 블로그 화면을 볼 수 있습니다.

### 참고
 * [Setting up Hexo on Windows](http://www.bravo-kernel.com/2015/10/setting-up-hexo-on-windows/)
 * [Github Pages와 Hexo](http://wit.nts-corp.com/2013/09/10/148)

## github 생성
저장소를 생성합니다.

![](https://goo.gl/EVuH5z)

저장소 이름을 설정합니다. 해당 저장소에서 `Settings > GitHub Pages > Automatic page generator > Launch automatic page generator` 를 클릭하여 적당한 템플릿을 선택하여 생성합니다.

`ssh-keygen`을 실행하여 ssh 키를 생성합니다. 해당 실행파일은 `C:\Program Files\Git\usr\bin` 에 있습니다.

```cmd
C:\Program Files\Git\usr\bin>ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/[USER]/.ssh/id_rsa):
Created directory '/c/Users/[USER]/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/[USER]/.ssh/id_rsa.
Your public key has been saved in /c/Users/[USER]/.ssh/id_rsa.pub.
```
.ssh/id_rsa 키를 저장하고 싶은 디렉토리를 입력합니다.
이어서 암호를 두 번 입력합니다. 이때 암호를 비워두면 키를 사용할 때 암호를 묻지 않습니다.

`id_rsa.pub` 파일의 내용을 복사해서, github에 등록합니다.

저장소에서 `Settings > Deploy keys > Deploy keys > Add deploy key` 를 클릭합니다. 복사한 내용을 등록하고 `Allow write access`를 선택한 다음 `Add key`를 눌러서 키를 등록합니다.

### 참고
* https://help.github.com/articles/generating-ssh-keys/
* https://git-scm.com/book/ko/v2/Git-%EC%84%9C%EB%B2%84-SSH-%EA%B3%B5%EA%B0%9C%ED%82%A4-%EB%A7%8C%EB%93%A4%EA%B8%B0

## configuration

블로그를 위해 생성한 디렉토리의 `_config.yml` 파일을 수정합니다. 

URL을 변경합니다.
```
url: https:/[YOUR_ID].github.iom/blog
root: /blog/
```
뒤에 blog가 붙은 이유는, 해당 저장소의 이름을 blog로 지정하였기 때문입니다. 

Deployment 를 수정합니다.
```
deploy:
  type: git
  repository: git@github.com:[YOUR_ID]/blog.git
  branch: gh-pages
```


## 포스팅
ssh 에이전트를 실행합니다.
```bash
ssh-agent -s
clip < id_rsa.pub
```

블로그를 위한 정적 파일을 생성하고 배포합니다.

```bash
hexo clean
hexo generate
hexo deploy
```

```bash
FATAL Error: Host key verification failed.
fatal: Could not read from remote repository.
```
위와 같은 에러가 발생하면 `ssh -T git@github.com` 을 실행하여 확인합니다.

```
C:\Program Files\Git\usr\bin>ssh -T git@github.com
The authenticity of host 'github.com ([IP])' can't be established.
RSA key fingerprint is SHA256:[fingerprint].
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,[IP]' (RSA) to the list of known hosts.
Hi [YOUR_ID]/blog! You've successfully authenticated, but GitHub does not provide shell access.
```

### 참고
* https://help.github.com/articles/generating-ssh-keys/
* http://blog.star-flare.com/2014/11/23/hexo-setting-google-adsense/
* https://github.com/hexojs/hexo/issues/1495

## 관련글
* [install-powerful-blog-framework-hexo](http://jacegem.github.io/blog/2016/01/hexo-install-powerful-blog-framework-hexo/)
* [hexo-change-theme](http://jacegem.github.io/blog/2016/01/hexo-change-theme/)
* [hexo-add-adsense-to-hexo](http://jacegem.github.io/blog/2016/01/hexo-add-adsense-to-hexo/)
* [hexo-add-tags-and-categories](http://jacegem.github.io/blog/2016/01/hexo-add-tags-and-categories/)


