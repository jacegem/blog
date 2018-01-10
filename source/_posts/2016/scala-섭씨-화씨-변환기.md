title: '[scala] 섭씨 화씨 변환기'
tags: [ scala, 섭씨, 화씨, 변환기, 공식, 온도 ]
date: 2016-01-06 16:07:17

---


섭씨, 화씨 변환 공식은 아래와 같습니다.

	섭씨 = (화씨 - 32) * 5 / 9
	화씨 = 섭씨 * 9 / 5 + 32


변환을 해보면 아래와 같습니다. 왼쪽에 값을 입력한 후에 `변환` 버튼을 클릭하면 변환된 값이 오른쪽에 표시됩니다.


섭씨 → 화씨:
<input type="text" id="c" class="col-xs-4"><input readonly id="cf" class="col-xs-4"></br>

화씨 → 섭씨:
<input type="text" id="f" class="col-xs-4"><input readonly id="fc" class="col-xs-4"></br>

<input type="button" value="변환" onclick="trans()">

<script>
function trans(){
	var c = $('#c').val();
    var f = $('#f').val();
	var cf = c * 9 / 5 + 32 ;
    var fc = (f - 32) * 5 / 9;
	
	$('#cf').val(cf);
    $('#fc').val(fc);
}
</script>


scala object 파일을 생성합니다.

import 구문을 추가합니다
```scala
import swing._
import event._
```

MainFrame 을 생성합니다.

```scala
object TempConverter extends SimpleSwingApplication{
  def top = new MainFrame{
    ...
  }
}
```

소스
```scala
package week1

import swing._
import event._

object TempConverter extends SimpleSwingApplication{
  def top = new MainFrame{
    title = "섭씨 / 화씨 변환기"
    object celsius extends TextField { columns = 5}
    object fahrenheit extends TextField { columns = 5}
    contents = new FlowPanel {
      contents += celsius
      contents += new Label (" Celsius = ")
      contents += fahrenheit
      contents += new Label (" Fahrenheit")
      border = Swing.EmptyBorder(15, 10, 10, 10)
    }
    
    listenTo(celsius, fahrenheit)
    reactions += {
      case EditDone(`fahrenheit`) =>
        val f = fahrenheit.text.toInt
        val c = (f - 32) * 5 / 9
        celsius.text = c.toString
      case EditDone(`celsius`) =>
        val c = celsius.text.toInt
        val f = c * 9 / 5 + 32
        fahrenheit.text = f.toString
    }
  }
}
```

gist
<script src="https://gist.github.com/jacegem/2bfd48eaf0342c52aa8b.js"></script>



실행화면

![실행화면](https://goo.gl/rvFOmL)



