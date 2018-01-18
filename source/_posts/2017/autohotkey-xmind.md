---
title: '[Autohotkey] xmind 입력 모드로 변경하기'
date: 2017-01-05 19:13:45
tags: [autohotkey]
categories:
- Programming
- Autohotkey
---

# [Autohotkey] xmind 입력 모드로 변경하기

윈도우에서 xmind 사용시 한글 입력에 약간(?)의 불편함이 있습니다.

![](https://goo.gl/mS9Nah)

이처럼 입력모드가 아닌 상태에서 한글이 입력된다. 입력 도중 스페이스바가 눌리면 그때, 입력모드로 변경되면서, 스페이스바는 무시가 되는 상태가 되어 버립니다.

이를 방지하고자, 새로운 노드 생성시, 스페이스바를 자동으로 입력하게 하여 입력모드로 변경하는 AHK 스크립트를 작성합니다.

```python
#ifWinActive ahk_exe XMind.exe

global input_state := False

; tab 또는 enter 를 누르면, 입력 모드로 변경한다.
TAB::
    changeState()
    return     
ENTER::
    changeState()
    return     

changeState(){    
    Send {%A_ThisHotkey%}    
    if (input_state != 1){        
        Send {F2}       
    }
    input_state := !input_state        
}

#IfWinActive
```


전체 스크립트는 https://github.com/jacegem/ahk_script 에서 받을 수 있습니다.
