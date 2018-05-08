---
title: "Amazfit Bip Font Creator 3 - 쓰레드 실행"
date: 2018.05.13
tags: [amazfit, bip, python, pyqt5, font, thread]
categories:
- Programming
- Python
---

# Amazfit Bip Font Creator 3 - 쓰레드 실행

![](http://bit.ly/2rplp9N)


## 쓰레드 생성

```python
# bip_font_creator.py

import os
import binascii
import glob
import shutil
from PyQt5.QtCore import QThread, pyqtSignal
from fontTools.ttLib import TTFont
from PIL import ImageFont, ImageDraw, Image

class FontCreator(QThread):
    set_progress_text = pyqtSignal(str)
    set_progress = pyqtSignal(int, int)
    done = pyqtSignal()

    def __init__(self, font_path, margin_top, margin_left, delete_bmp, overwrite_bmp, root_path, parent=None):
        QThread.__init__(self, parent)
        self.font_path = font_path
        self.margin_top = int(margin_top)
        self.margin_left = int(margin_left)
        self.delete_bmp = delete_bmp
        self.overwrite_bmp = overwrite_bmp

        self.tt_font = TTFont(self.font_path)
        self.image_font = ImageFont.truetype(self.font_path, 15)
        self.root_path = root_path

        self.bmp_dir = None
        self.ft_dir = None
        self.bmp_dir = self.create_directory('bmp')
        self.ft_dir = self.create_directory('ft')
```

QThread 를 상속받아 구현합니다. 

- font_path : 폰트 파일 경로
- margin_top : 비트맵 상단 여백
- margin_left : 비트맵 좌측 여백
- delete_bmp : 비트맵 삭제 여부
- overwrite_bmp : 비트맵 덮어쓰기 여부
- root_path : 실행 경로
- parent : 부모 쓰레드

parent를 꼭 __init__을 해주어야 합니다. 
```python
QThread.__init__(self, parent)
```

## pyqtSignal

부모 쓰레드에 전달을 위한 시그널을 3개 생성합니다. 

```python
set_progress_text = pyqtSignal(str)
set_progress = pyqtSignal(int, int)
done = pyqtSignal()
```

- set_progress_text : 텍스트 변경시 사용
- set_progress : 진행률 변경시 사용
- done : 완료시 사용

```python
class AmazfitBipFontCreator(QMainWindow):
    # .. 생략 .. 
    def create_font(self):
    # .. 생략 .. 
    font_creator_thread = FontCreator(font_path, margin_top, margin_left, delete_bmp, overwrite_bmp, self.root_path, self)
    font_creator_thread.set_progress_text.connect(self.set_progress_text)
    font_creator_thread.set_progress.connect(self.set_progress)
    font_creator_thread.done.connect(self.create_done)
    font_creator_thread.start()
```

AmazfitBipFontCreator 에서 create_font 실행시에, 부모 쓰레드 전달을 위해 `self`를 넘겨주었습니다. 
그리고 시그널 들을 함수에 연결하였습니다. 

- set_progress_text.connect(`self.set_progress_text`)
- set_progress.connect(`self.set_progress`)
- done.connect(`self.create_done`)


## pyqtSignal Connect 

```python
from PyQt5.QtCore import pyqtSlot
# .. 생략 .. 
class AmazfitBipFontCreator(QMainWindow):
    # .. 생략 .. 
    @pyqtSlot(str)
    def set_progress_text(self, text):
        self.lbl_prog.setText(text)
        
    @pyqtSlot(int, int)
    def set_progress(self, current, end):
        val = current / end * 100
        self.progress.setValue(int(val))
        
    @pyqtSlot()
    def create_done(self):
        self.set_progress_text('Finished')
        self.set_progress(1, 1)
        self.btn_create.setEnabled(True)
        
        msg_box = QMessageBox()
        msg_box.setWindowTitle('Font Creator')
        msg_box.setText('Finished')        
        msg_box.addButton(QPushButton('OK'), QMessageBox.NoRole)        
        msg_box.exec()
```

`@pyqtSlot` 어노테이션을 사용하고 파라미터 타입`(str)`을 지정합니다. 

- @pyqtSlot(str)
- @pyqtSlot(int, int)
- @pyqtSlot()

완료 시그널이 보내지면 `OK`메시지 창을 보여줍니다. 

```python
msg_box = QMessageBox()
msg_box.setWindowTitle('Font Creator')
msg_box.setText('Finished')        
msg_box.addButton(QPushButton('OK'), QMessageBox.NoRole)        
msg_box.exec()
```



<script src="https://gist.github.com/jacegem/5f5cead38e05c87bbff4363e49490348.js"></script>



