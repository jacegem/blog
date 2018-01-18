---
title: Run Python as a Windows Service
date: 2017-03-06
tags: [python, windows, service]
categories:
- Programming
- Python
---

# Run Python as a Windows Service

단순하게 `sc.exe` 를 사용한다.

샘플코드인데

```sh
sc create PythonApp binPath= "C:\Python34\Python.exe --C:\tmp\pythonscript.py"
```

`--` 이것이 왜 있는거지...???

빼고 서비스를 생성해보자.

```sh
sc create django binpath= "%py32% %conoha%\system-trading-django\twisted_run.tac" start= "auto"
```

%py32%, %conoha% 는 시스템 변수로 등록해 놓았다.

중요한 것은 `binpath=` 뒤에 공백한칸이 있는 것이다. 모든 `=` 뒤에는 공백이 있어야 한다.




### 출처
- http://stackoverflow.com/questions/32404/is-it-possible-to-run-a-python-script-as-a-service-in-windows-if-possible-how
