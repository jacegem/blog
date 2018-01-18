---
title: windows에서 django mysqldb 연결 시 에러
date: 2017-01-16 19:13:45
tags: [python, django]
categories:
- Programming
- Python
---

# windows에서 django mysqldb 연결 시 에러

```
Error loading MySQLdb module: No module named 'MySQLdb'
```


You can use mysqlclient instead of MySQLdb. MySqLdb is not compatible with Python 3.

```sh
pip install mysqlclient
```

## 접속정보 설정

```python
# settings.py

DATABASES = {
    'default':{
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'TABLE_NAME',
        'USER': 'USER',
        'PASSWORD': 'PASSOWRD',
        'HOST': 'HOST',
        'PORT': 'PORT'
    }
}
```



### 출처
- http://stackoverflow.com/questions/39574813/error-loading-mysqldb-module-no-module-named-mysqldb
