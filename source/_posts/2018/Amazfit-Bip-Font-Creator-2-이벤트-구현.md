---
title: "Amazfit Bip Font Creator 2 - 이벤트 구현"
date: 2018.05.12
tags: [amazfit, bip, python, pyqt5, font]
categories:
- Programming
- Python
---

# Amazfit Bip Font Creator 2 - 이벤트 구현

![UI](https://goo.gl/6QThxs)

1. Select TTF file 버튼 이벤트
2. Font File Create 버튼 이벤트

## Select TTF file

```python
class AmazfitBipFontCreator(QMainWindow):
    # .. 생략 .. 
    def get_create_box(self):
        # .. 생략 .. 
        btn_ttf = QPushButton('Select TTF file')
        btn_ttf.clicked.connect(lambda: self.select_file(self.lbl_ttf))
```

이벤트 처리시에 파라미터 전달을 위해 `lambda`로 구현하였습니다. 파일선택다이얼로그에서 파일을 선택할 경우 라벨에 표시하기 위해 표시 대상(self.lbl_ttf)을 전달합니다. 

```python
class AmazfitBipFontCreator(QMainWindow):
    # .. 생략 ..
    def select_file(self, target):
        # Select the file dialog design.
        dialog_style = QFileDialog.DontUseNativeDialog
        dialog_style |= QFileDialog.DontUseCustomDirectoryIcons

        # Open the file dialog to select an image file.
        file_chosen, _ = QFileDialog.getOpenFileName(self, "QFileDialog.getOpenFileName()", "", "TTF (*.ttf)")

        # Show the path of the file chosen.
        if file_chosen:
            target.setText(file_chosen)
        else:
            target.setText("No file was selected. Please select an TTF.")
```

파일다이얼로그를 `*.ttf` 로 조건을 설정합니다.
파일선택시 넘겨진 대상 `target`에 텍스트를 설정합니다. 
파일이 선택되지 않으면 "No file was selected. Please select an TTF." 문자열을 설정합니다. 

![](http://bit.ly/2rj0PaY)

## Font File Create

```python
class AmazfitBipFontCreator(QMainWindow):
    # .. 생략 .. 
    def get_create_box(self):
        # .. 생략 ..                 
        self.btn_create = FullButton('''Font File Create
        
        *.ft file will be created in the ft sub-folder'''
                                     )
        self.btn_create.clicked.connect(self.create_font)
```

`Font File Create` 버튼이 눌리면 `self.create_font`를 실행합니다. 

```python
class AmazfitBipFontCreator(QMainWindow):
    # .. 생략 .. 
    def create_font(self):
        print('run test from main')
        font_path = self.lbl_ttf.text()
        delete_bmp = self.chk_delete_bmp.isChecked()
        overwrite_bmp = self.chk_overwrite_bmp.isChecked()
        margin_top = self.sb_margin_top.text()
        margin_left = self.sb_margin_left.text()

        if not font_path.lower().endswith('.ttf'):
            msg_box = QMessageBox()
            msg_box.setText("Please Select TTF File")
            msg_box.exec()
            return

        self.config['PATH'] = {}
        self.config['PATH']['ttf'] = font_path
        with open(self.config_file_name, 'w') as configfile:
            self.config.write(configfile)

        self.btn_create.setEnabled(False)
        self.set_progress_text('Start!')

        font_creator_thread = FontCreator(font_path, margin_top, margin_left, delete_bmp, overwrite_bmp, self.root_path, self)
        font_creator_thread.set_progress_text.connect(self.set_progress_text)
        font_creator_thread.set_progress.connect(self.set_progress)
        font_creator_thread.done.connect(self.create_done)
        font_creator_thread.start()
```

폰트 생성에 필요한 설정값들을 설정합니다. 

```python
font_path = self.lbl_ttf.text()
delete_bmp = self.chk_delete_bmp.isChecked()
overwrite_bmp = self.chk_overwrite_bmp.isChecked()
margin_top = self.sb_margin_top.text()
margin_left = self.sb_margin_left.text()
```

font 파일이 설정되지 않았다면 메시지 창을 보여주고 return 합니다. 

```python
if not font_path.lower().endswith('.ttf'):
    msg_box = QMessageBox()
    msg_box.setText("Please Select TTF File")
    msg_box.exec()
    return
```

font 파일이 정확히 설정되었다면 다음 실행시에 사용하기 위해 ini 파일에 기록합니다. 

```python
self.config['PATH'] = {}
self.config['PATH']['ttf'] = font_path
with open(self.config_file_name, 'w') as configfile:
    self.config.write(configfile)
```

폰트 생성을 시작할 준비가 되었다면 버튼을 비활성화 합니다. 
```python
self.btn_create.setEnabled(False)
self.set_progress_text('Start!')
```

폰트 생성을 위한 스레드를 실행합니다. 
```python
font_creator_thread = FontCreator(font_path, margin_top, margin_left, delete_bmp, overwrite_bmp, self.root_path, self)
font_creator_thread.set_progress_text.connect(self.set_progress_text)
font_creator_thread.set_progress.connect(self.set_progress)
font_creator_thread.done.connect(self.create_done)
font_creator_thread.start()
```



<script src="https://gist.github.com/jacegem/5f5cead38e05c87bbff4363e49490348.js"></script>