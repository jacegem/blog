---
title: "Amazfit Bip Font Creator 1 - UI 구성"
date: 2018.05.11
tags: [amazfit, bip, python, pyqt5, font]
categories:
- Programming
- Python
---

# Amazfit Bip Font Creator 1 - UI 구성

Amazfit Bip 에서 사용할 폰트를 생성합니다. `Qt5`를 사용하여 UI를 생성합니다. 

## Qt5 사용

QT에서 UI를 생성할 경우

`QMainWindow`를 사용시에는 Widget을 생성하고 `setCentrlWidget`을 호출해야 합니다. 

```python
class AmazfitBipFontCreator(QMainWindow):
    def __init__(self):
        widget = QWidget()
        # UI 구성
        self.setCentralWidget(widget)
        self.show()
```

위와 다르게, `QWidget`, `QDialog`를 사용한다면 그냥 `self.show()`를 해서 보여줄 수 있습니다. 

```python
class AmazfitBipFontCreator(QDialog):
    def __init__(self):
        # UI 구성
        self.show()
```

# 화면 구성

![UI](https://goo.gl/6QThxs)

1. 폰트 파일 선택
2. Margin 설정
3. 옵션 설정
4. 폰트 생성
5. 진행률 표시

위와 같이 화면구성을 하겠습니다. 

## QMainWindow

```python
# Amazfit_Bip_Font_Creator.py

import configparser
import os
import sys
import webbrowser

from PyQt5.QtCore import pyqtSlot
from PyQt5.QtGui import QIcon
from PyQt5.QtWidgets import (QMainWindow, QApplication, QDesktopWidget, QGroupBox, QWidget, QFileDialog, QLabel, QLineEdit, QMessageBox,
                             QSpinBox, QPushButton, QStyleFactory, QHBoxLayout, QVBoxLayout, QCheckBox, QProgressBar)

from qt.bip_font_creator import FontCreator
from qt.button import DonateButton, FullButton

class AmazfitBipFontCreator(QMainWindow):
    def __init__(self):
        super().__init__()
        self.title = 'Amazfit Bip - Font Creator (v0.1)'
        self.left = 10
        self.top = 10
        self.width = 700
        self.height = 300

        self.lbl_ttf = None  # Font file name
        self.chk_delete_bmp = None  # option delete bmp
        self.chk_overwrite_bmp = None
        self.lbl_prog = None
        self.progress = None
        self.sb_margin_top = None
        self.sb_margin_left = None

        self.initUI()
        self.center()
```

self.initUI()에서 화면을 구성하며
self.cetner()에서 화면 중앙으로 이동합니다. 

```python
class AmazfitBipFontCreator(QMainWindow):
    # .. 생략 ..    
    def center(self):
        # geometry of the main window
        qr = self.frameGeometry()
        # center point of screen
        cp = QDesktopWidget().availableGeometry().center()
        # move rectangle's center point to screen's center point
        qr.moveCenter(cp)
        # top left of rectangle becomes top left of window centering it
        self.move(qr.topLeft())

    def initUI(self):

        self.setWindowTitle(self.title)
        self.setMinimumWidth(self.width)
        self.setMinimumHeight(self.height)

        widget = QWidget()
        widget.setLayout(self.get_layout())

        self.setCentralWidget(widget)

        icon = QIcon(self.resource_path('assets/font.png'))
        self.setWindowIcon(icon)
        self.show()
```

self.get_layout()에서 화면을 구성하겠습니다. 

```python
class AmazfitBipFontCreator(QMainWindow):
    # .. 생략 .. 
    def get_layout(self):
        layout = QVBoxLayout()
        
        vbox_create = QVBoxLayout()
        vbox_create.addLayout(self.get_create_box())        
        
        hbox_progress = QHBoxLayout()
        self.lbl_prog = QLabel('Done')
        hbox_progress.addWidget(self.lbl_prog)
        self.progress = QProgressBar()
        self.progress.setMaximum(100)
        self.progress.setMinimum(0)
        hbox_progress.addWidget(self.progress)

        layout.addLayout(vbox_create)
        layout.addLayout(hbox_progress)
        return layout
```

- QVBoxLayout() 으로 설정 부분과 진행률 부분을 나눕니다.
  - QVBoxLayout() 으로 설정을 위한 항목들을 나눕니다.
  - QHBoxLayout() 으로 진행률을 표시합니다.

```python
class AmazfitBipFontCreator(QMainWindow):
    # .. 생략 ..     
    def get_create_box(self):
        create_box = QVBoxLayout()
        create_group = QGroupBox('Create')
        create_layout = QVBoxLayout()

        # TTF 선택 라벨, 버튼
        row_ttf = QHBoxLayout()

        self.lbl_ttf = QLineEdit('No file selected')
        self.lbl_ttf.setDisabled(True)
        self.lbl_ttf.setMinimumWidth(300)
        row_ttf.addWidget(self.lbl_ttf)

        btn_ttf = QPushButton('Select TTF file')
        btn_ttf.clicked.connect(lambda: self.select_file(self.lbl_ttf))
        row_ttf.addWidget(btn_ttf)

        # margin-top, margin-left
        row_margin = QHBoxLayout()
        row_margin.addWidget(QLabel('Margin-Top:'))
        self.sb_margin_top = QSpinBox()
        self.sb_margin_top.setMinimum(-99)
        row_margin.addWidget(self.sb_margin_top)

        row_margin.addWidget(QLabel('Margin-Left:'))
        self.sb_margin_left = QSpinBox()
        self.sb_margin_left.setMinimum(-99)
        row_margin.addWidget(self.sb_margin_left)

        # 옵션들
        row_option = QHBoxLayout()
        self.chk_delete_bmp = QCheckBox('Delete BMP files')
        self.chk_delete_bmp.setChecked(True)
        row_option.addWidget(self.chk_delete_bmp)
        self.chk_overwrite_bmp = QCheckBox('Overwrite BMP files')
        row_option.addWidget(self.chk_overwrite_bmp)

        # create 버튼
        row_create = QHBoxLayout()
        self.btn_create = FullButton('''Font File Create
        
        *.ft file will be created in the ft sub-folder'''
                                     )
        self.btn_create.clicked.connect(self.create_font)
        row_create.addWidget(self.btn_create)

        # 전체 순서 결정
        create_layout.addLayout(row_ttf)
        create_layout.addLayout(row_margin)
        create_layout.addLayout(row_option)
        create_layout.addLayout(row_create)

        # set layer
        create_group.setLayout(create_layout)
        create_box.addWidget(create_group)
        return create_box
```

`QSspinBox`에서 음수를 사용하기 위해서는 `setMinimum`을 설정해야 합니다. 

```python
self.sb_margin_top.setMinimum(-99)
```

## FullButton

Button을 자주사용하게 되어 `FullButton` 클래스를 생성하였습니다. 최대크기로 버튼을 생성합니다. 

```python
# button.py

from PyQt5.QtWidgets import QPushButton, QSizePolicy

class FullButton(QPushButton):
    def __init__(self, title):
        super().__init__(title)
        super().setSizePolicy(QSizePolicy.Expanding, QSizePolicy.Expanding)
```

## resource_path

`assets/font.png` 파일을 아이콘으로 사용합니다. 
pyinstaller로 실행파일을 생성했을 때에는 경로가 달라지므로, `resource_path`함수를 사용하였습니다. 

```python
class AmazfitBipFontCreator(QMainWindow):
    # .. 생략 ..    
    def resource_path(self, relative_path):
        if hasattr(sys, '_MEIPASS'):
            return os.path.join(sys._MEIPASS, relative_path)
        return os.path.join(os.path.abspath("."), relative_path)
```



<script src="https://gist.github.com/jacegem/5f5cead38e05c87bbff4363e49490348.js"></script>
