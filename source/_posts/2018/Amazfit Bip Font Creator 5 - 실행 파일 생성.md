---
title: "Amazfit Bip Font Creator 5 - 실행 파일 생성"
date: 2018.05.15
tags: [amazfit, bip, python, pyqt5, font, pyinstaller]
categories:
- Programming
- Python
---

# Amazfit Bip Font Creator 5 - 실행 파일 생성

![](https://www.pyinstaller.org/_images/pyinstaller-draft1c-header-trans.png)

실행 파일은 pyinstaller 로 생성합니다. 

먼저 pyinstaller 를 설치합니다. 

```shell
$ pip install pyinstaller
```

이 후 `pyinstaller 파일명` 을 입력하여 exe파일을 생성할 수 있습니다. 

여기서 몇 가지 옵션을 사용합니다. 

```shell
$ pyinstaller --onefile --clean --windowed --icon=.\assets\font.ico Amazfit_Bip_Font_Creator.py
```

- --onefile : 하나의 파일로 생성합니다. 이 옵션이 없으면 여러개의 파일로 생성됩니다
- --clean : 기존 파일들을 삭제합니다.
- --windowed : 윈도우창으로 실행되도록 합니다. 이 옵션이 없으면 cmd 창으로 실행됩니다.
- --icon: 아이콘을 지정합니다. 

이렇게 생성하면 내부적으로 사용하는 리소스가 표현되지 않을 수 있습니다. 

## spec 파일 수정

`pyinstaller 파일명` 을 실행하면 같은 경로에 `파일명.spec`파일이 생성됩니다. 

```python
# -*- mode: python -*-

block_cipher = None


a = Analysis(['Amazfit_Bip_Font_Creator.py'],
             pathex=['C::\\YOUR_PATH'],
             binaries=[],
             datas=[],
             hiddenimports=[],
             hookspath=[],
             runtime_hooks=[],
             excludes=[],
             win_no_prefer_redirects=False,
             win_private_assemblies=False,
             cipher=block_cipher)

pyz = PYZ(a.pure, a.zipped_data,
             cipher=block_cipher)
exe = EXE(pyz,
          a.scripts,
          a.binaries,
          a.zipfiles,
          a.datas,
          name='Amazfit_Bip_Font_Creator',
          debug=False,
          strip=False,
          upx=True,
          runtime_tmpdir=None,
          console=False , icon='assets\\font.ico')
```

여기에 a.datas 로 리소스를 추가합니다. 

```python
a.datas += [('.\\assets\\font.png', '.\\assets\\font.png', 'DATA')]
```

추가한 spec 파일입니다.

```python
# -*- mode: python -*-

block_cipher = None


a = Analysis(['Amazfit_Bip_Font_Creator.py'],
             pathex=['C::\\YOUR_PATH'],
             binaries=[],
             datas=[],
             hiddenimports=[],
             hookspath=[],
             runtime_hooks=[],
             excludes=[],
             win_no_prefer_redirects=False,
             win_private_assemblies=False,
             cipher=block_cipher)
a.datas += [('.\\assets\\font.png', '.\\assets\\font.png', 'DATA')]
pyz = PYZ(a.pure, a.zipped_data,
             cipher=block_cipher)
exe = EXE(pyz,
          a.scripts,
          a.binaries,
          a.zipfiles,
          a.datas,
          name='Amazfit_Bip_Font_Creator',
          debug=False,
          strip=False,
          upx=True,
          runtime_tmpdir=None,
          console=False , icon='assets\\font.ico')
```

이 후 `spec`파일을 사용하여 exe를 생성합니다. 

```shell
$ pyinstaller Amazfit_Bip_Font_Creator.spec
```

![](http://bit.ly/2rkhPgP)

그러면 /dist 폴더에 `Amazfit_Bip_Font_Creator.exe`파일이 생성됩니다. 

<script src="https://gist.github.com/jacegem/5f5cead38e05c87bbff4363e49490348.js"></script>