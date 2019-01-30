---
title: "Amazfit Bip Font Creator 4 - 폰트 생성"
date: 2018.05.14
tags: [amazfit, bip, python, pyqt5, font]
categories:
- Programming
- Python
---

# Amazfit Bip Font Creator 4 - 폰트 생성

![](http://bit.ly/2rijfIX)

쓰레드 초기화가 끝났으면, font_creator_thread.start()로 `run`을 실행합니다. 

```python
class FontCreator(QThread):
    # 생략
    def run(self):
        self.set_progress_text.emit("1/2")
        self.create_bmp()
        self.set_progress_text.emit("2/2")
        self.pack_bmp()

        if self.delete_bmp:
            shutil.rmtree(self.bmp_dir)

        self.done.emit()
```

먼저 bmp 파일들을 생성하고 패킹하여 폰트파일을 생성합니다. 

## create_bmp

```python
# bip_font_creator.py

import os
import binascii
import glob
import shutil
from PyQt5.QtCore import QThread, pyqtSignal
from fontTools.ttLib import TTFont
from PIL import ImageFont, ImageDraw, Image

all_range = (0x0000, 0xFFFF)

black_histogram = [256, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
                   0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
                   0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
                   0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
                   0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
                   0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
                   0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
                   
class FontCreator(QThread):
    # 생략
    def create_bmp(self):
        for i in range(all_range[0], all_range[1]):
            self.set_progress.emit(i, all_range[1])

            result = self.char_in_font(chr(i), self.tt_font)
            if result is False:
                # print('없음: {}'.format(i))
                continue

            image = Image.new('1', (16, 16), "black")
            draw = ImageDraw.Draw(image)
            draw.text((self.margin_left, self.margin_top), chr(i), font=self.image_font, fill="white")

            if i is not 32 and i is not 127 and image.histogram() == black_histogram:
                print("{:04x} is black".format(i))
                continue

            file_name = "bmp/{:04x}2.bmp".format(i)
            file_path = os.path.join(self.root_path, file_name)

            if self.overwrite_bmp:
                if os.path.isfile(file_path):
                    continue

            image.save(file_path, "bmp")
```


pyqtSignal 로 전달하기 위해 `emit`을 호출합니다. 

```python
self.set_progress.emit(i, all_range[1])
```

`all_range = (0x0000, 0xFFFF)` 로 전체 범위의 폰트 bmp 를 생성합니다. 폰트파일에 해당 문자 가 있는지 확인하기 위해 `char_in_font` 함수를 호출합니다. 

```python
class FontCreator(QThread):
    # 생략
    def char_in_font(self, unicode_char, font):
        for cmap in font['cmap'].tables:
            if cmap.isUnicode():
                if ord(unicode_char) in cmap.cmap:
                    return True
        return False
```

해당 문자가 폰트 파일에 있으면 `True`를 리턴합니다. 없으면 False

Image 로 text 를 생성한 다음 빈 문자인지 확인하기 이해 histogram을 비교합니다.

```python
image = Image.new('1', (16, 16), "black")
draw = ImageDraw.Draw(image)
draw.text((self.margin_left, self.margin_top), chr(i), font=self.image_font, fill="white")

if i is not 32 and i is not 127 and image.histogram() == black_histogram:
    print("{:04x} is black".format(i))
    continue
```

black_histogram 은 위에서 정의한 배열입니다. `black_histogram = [256, 0, ... , 0]`

그리고 덮여쓰기 설정 여부에 따라 이미지를 저장합니다. (save)

```python
if self.overwrite_bmp:
    if os.path.isfile(file_path):
        continue

image.save(file_path, "bmp")
```

## pack_bmp

```python
class FontCreator(QThread):
    # 생략
    def pack_bmp(self):
        base = os.path.basename(self.font_path)
        file, ext = os.path.splitext(base)
        ft_file_name = 'amazfit_bip_{}.ft'.format(file)
        print('ft_file_name', ft_file_name)
        ft_path = os.path.join(self.ft_dir, ft_file_name)
        self.pack_font(ft_path)
```

> `pack_font` 는 [tools/bipfont.py at master · amazfitbip/tools · GitHub](https://github.com/amazfitbip/tools/blob/master/bipfont.py) 를 사용하였습니다.

실행파일에서 사용하기 위해 경로 관련된 부분만 변경하였습니다. 

```python
class FontCreator(QThread):
    # 생략
    def pack_font(self, font_path):
        print('Packing', font_path)
        font_file = open(font_path, 'wb')
        header = bytearray(binascii.unhexlify('4E455A4B08FFFFFFFFFF01000000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF0000'))
        bmps = bytearray()

        range_nr = 0
        seq_nr = 0
        startrange = -1

        bmp_files = sorted(glob.glob(os.path.join(self.root_path, 'bmp' + os.sep + '*')))
        bmp_len = len(bmp_files)
        path_len = len(self.root_path) + 1

        for i in range(0, bmp_len):
            print('pack_font', i, bmp_len)
            self.set_progress.emit(i, bmp_len)

            margin_top = int(bmp_files[i][path_len + 8])

            if (i == 0):
                unicode = int(bmp_files[i][path_len + 4:-5], 16)
            else:
                unicode = next_unicode

            if (i + 1 < len(bmp_files)):
                next_unicode = int(bmp_files[i + 1][path_len + 4:-5], 16)
            else:
                next_unicode = -1

            if (unicode != next_unicode):
                if (startrange == -1):
                    range_nr += 1
                    startrange = unicode

                img = Image.open(bmp_files[i])
                img_rgb = img.convert('RGB')
                pixels = img_rgb.load()

                x = 0
                y = 0
                char_width = 0

                while y < 16:
                    b = 0
                    for j in range(0, 8):
                        if pixels[x, y] != (0, 0, 0):
                            b = b | (1 << (7 - j))
                            if (x > char_width):
                                char_width = x
                        x += 1
                        if x == 16:
                            x = 0
                            y += 1
                    bmps.extend(b.to_bytes(1, 'big'))
                char_width = char_width * 16 + margin_top
                bmps.extend(char_width.to_bytes(1, 'big'))

                if unicode + 1 != next_unicode:
                    endrange = unicode
                    sb = startrange.to_bytes(2, byteorder='big')
                    header.append(sb[1])
                    header.append(sb[0])
                    eb = endrange.to_bytes(2, byteorder='big')
                    header.append(eb[1])
                    header.append(eb[0])
                    seq = seq_nr.to_bytes(2, byteorder='big')
                    header.append(seq[1])
                    header.append(seq[0])
                    seq_nr += endrange - startrange + 1
                    startrange = -1
            else:
                print('multiple files of {:04x}'.format(unicode))

        rnr = range_nr.to_bytes(2, byteorder='big')
        header[0x20] = rnr[1]
        header[0x21] = rnr[0]

        font_file.write(header)
        font_file.write(bmps)
```


## 실행 완료

실행이 완료되면 

1. 실행파일 경로에 config.ini 파일이 생성됩니다. 
2. `OK` 메시지 창이 출력됩니다. 
3. 그리고 ft 폴더안에 폰트 파일이 생성됩니다. 



<script src="https://gist.github.com/jacegem/5f5cead38e05c87bbff4363e49490348.js"></script>
