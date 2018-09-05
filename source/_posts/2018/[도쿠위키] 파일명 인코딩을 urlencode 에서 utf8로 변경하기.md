---
title: "[도쿠위키] 파일명 인코딩을 urlencode 에서 utf8로 변경하기"
date: 2018.09.05
tags: [도쿠위키, 파일명, 인코딩, urlencode, utf-8]
categories:
  - application
  - wiki
---

  # [도쿠위키] 파일명 인코딩을 urlencode 에서 utf8로 변경하기

  오래전에 설치한 도쿠위키라서 파일 저장시에 `url`로 인코딩 되도록 지정되어 있었습니다.

  ![](https://lh3.googleusercontent.com/-ZPAWvmcaM_w/W48kdR2K--I/AAAAAAAAG1g/qE4ovsiE1nsQbwLqKGxNcrV8aOa6BZg8wCHMYCw/s0/StrokesPlus_2018-09-05_34.png)

  이것을 `utf-8`로 변경하니 문서를 읽지 못하게 되었습니다. 

  ![](https://lh3.googleusercontent.com/-edYCsfDehL0/W48krmfaPJI/AAAAAAAAG1k/flEC9HNqSHw23QUqBKyEzToDBdixasoQQCHMYCw/s0/StrokesPlus_2018-09-05_35.png)

  ## UTF-8 로 변경

  그리하여, 저장된 파일명들을 모두 utf-8로 변경하기로 합니다. 

  도쿠위키에서 파일들은 `/data/pages` 밑에 저장됩니다.


  ```python
  
  import os
  from glob import glob
  import urllib.parse
  
  
  def change_file(path):
      files = glob(path, recursive=True)
      for file in files:
          if os.path.isfile(file):
              dir_name = os.path.dirname(file)
              file_name = os.path.basename(file)
              print('{} and {}'.format(dir_name, file_name))
              new_name = urllib.parse.unquote(file_name)
              print("encode to {}".format(new_name))
              os.rename(os.path.join(dir_name, file_name), os.path.join(dir_name, new_name))
  
  
  def change_folder(path):
      files = glob(path, recursive=True)
      for file in files:
          if os.path.isdir(file):
              new_name = urllib.parse.unquote(file)
              print("encode to {}".format(new_name))
              os.rename(file, new_name)
  
  
  if __name__ == "__main__":
      base_folder = '/YOUR_PATH/pages'
      change_file(base_folder + '/**/*.txt')
      change_folder(base_folder + '/**/')
  
  ```

  디렉토리 구조가 깊게 되어 있다면 폴더명 변경시에 해당 디렉토리가 변경되므로, 폴더명 변경을 여러번 해야 합니다. 




  ## 에러 처리

  ### You should check your config and permission settings.

  에러 발생시 폴더에 권한을 부여합니다.


  ```shell
  chmod -R 777 data/ ; chmod -R 777 lib/ ; chmod -R 777 conf/
  ```


  ## 참고

  - https://forum.dokuwiki.org/post/56689