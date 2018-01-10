title: '[autohotkey] PPT 작성을 위한 AutoHotkey 스크립트'
tags: [autohotkey, ahk, ppt, script]
date: 2016-01-24 20:44:02

---

### WASD 키를 방향키로 변경하기

스크립트를 실행하면, fnOn의 값은 false이기때문에 정상적으로 키입력이 됩니다. 왼손으로 방향키 입력이 필요한 경우에 `Capslock` + `TAB` 키를 눌러서 fnOn의 값을 true변경합니다. 이때부터 WASD는 방향키로 동작하게 됩니다.

전체소스는 아래와 같습니다.

```autohotkey
fnOn == false
~Capslock & TAB::fnOn:=!fnOn
#If fnOn
	a::Send {Left}
	s::Send {Down}
	d::Send {Right}
	w::Send {Up}
#If
```

### 다른 키들도 변경하기

wasd 가 변경되어서 이미 일반 키보드로써의 기능은 잃어버렸습니다. 다른 키들도 변경되도록 하겠습니다. 동일한 형식으로 작성하여 원하는 키로 변경하면 됩니다.

텐키리스 키보드를 사용중일 때 유용할 수 있도록, 상태변경으로 키패드 입력이 되도록 하겠습니다.

```autohotkey
	m::Send 0
	j::Send 1
	k::Send 2
	l::Send 3
	u::Send 4
	i::Send 5
	o::Send 6
```language
```
mjkluio 의 키 입력을 각각 0123456으로 변경합니다. 789은 원래의 숫자키를 사용하므로 변경하지 않습니다.

PPT 작성중에 복사하기 & 붙여넣기 신공도 많이 사용하게 됩니다. 이것도 추가합니다. 

```autohotkey
	c::Send ^c
	v::Send ^v
	x::Send ^x
	z::Send ^z
```

펑션키도 활용합니다. PPT에서 페이지 단위로 이동할 경우를 고려한 키입력을 추가합니다.

```autohotkey
	F1::Send {Left}
	F2::Send {WheelUp 2}
	F3::Send {WheelDown 2}
	F4::Send {Right}
```

이렇게 사용하면 ~~장담할수 없지만~~ 작업효율이 0.000001% 정도 올라갈 것으로 예측되옵니다.

전체소스는 아래와 같습니다.

```autohotkey
fnOn == false
~Capslock & TAB::fnOn:=!fnOn

#If fnOn
	;;; function keys
	F1::Send {Left}
	F2::Send {WheelUp 2}
	F3::Send {WheelDown 2}
	F4::Send {Right}

	;;; numbers
	m::Send 0
	j::Send 1
	k::Send 2
	l::Send 3
	u::Send 4
	i::Send 5
	o::Send 6

	;;; cursor keys
	a::Send {Left}
	s::Send {Down}
	d::Send {Right}
	w::Send {Up}
	q::Send {BS}
	;e::Send {Delete}
	e::Send {Enter}
	r::Send {Enter}
	c::Send ^c
	v::Send ^v
	x::Send ^x
	z::Send ^z
#If
```


### 에피소드

`#If`를 몰랐을때에는 if 로 모든것을 비교했었습니다. 

```autohotkey
$w::
if (fnOn = true) {
 	Send {Blind}{up}
} else { 
 	Send {Blind}w
} 
return
```
이렇게요. 하나씩 키 입력을 지정 하려 하다 보니, 중복되는 코드가 너무 많아서 관리가 안되었습니다. 예전소스가 해당 파일에 주석으로 남아있어서 올렸습니다.


결론은..... `#If`를 사용합니다.