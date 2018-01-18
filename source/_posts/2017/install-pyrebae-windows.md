---
title: 윈도우에 pyrebase 설치
date: 2017-02-10 19:13:45
tags: [windows, python, pyrebase]
categories:
- Programming
- Python
---

# 윈도우에 pyrebase 설치

## 설치

```sh
Collecting jws>=0.1.3 (from python-jwt==2.0.1->pyrebase)
  Using cached jws-0.1.3.tar.gz
    Complete output from command python setup.py egg_info:
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "C:\Users\xxx\AppData\Local\Temp\pip-build-4k32bmcg\jws\setup.py", line 17, in <module>
        long_description=read('README.md'),
      File "C:\Users\xxx\AppData\Local\Temp\pip-build-4k32bmcg\jws\setup.py", line 5, in read
        return open(os.path.join(os.path.dirname(__file__), fname)).read()
    UnicodeDecodeError: 'cp949' codec can't decode byte 0xe2 in position 500: illegal multibyte sequence
```

setup.py 파일을 찾을 수 없었습니다. 그리하여

`jws-0.1.3-py3.4.egg` 파일을 다운받아 `easy_install`로 설치하였지만 동일한 오류 발생.

다운 받은 파일을 압축 풀어 `jws-0.1.3-py3.4\jws` 폴더를 파이썬 라이브러리 폴더에 복사

아나콘다의 경우 `[Anaconda_PATH]\Lib\site-packages` 에 복사합니다.

pip install pyrebase 를 실행하면 설치 완료


## 출처
- https://pypi.python.org/pypi/jws
- https://github.com/thisbejim/Pyrebase
