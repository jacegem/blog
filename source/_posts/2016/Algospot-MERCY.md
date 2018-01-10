title: '[algospot] MERCY'
tags: [ algospot, mercy, algorithm, 알고리즘, 알고스팟 ]
date: 2016-01-10 13:48:56

---
[algospot] 알고스팟 - MERCY

scala로 작성해보려 합니다. 먼저 튜토리얼을 확인해 봅니다.

	스칼라의 경우에도, main() 함수가 있는 오브젝트의 이름은 항상 Main 이어야 한다.

```scala
object Main {
  def main(args: Array[String]): Unit = {
    var cases = Integer.parseInt(readLine())
    while (cases > 0) {
      println("Hello, " + readLine() + "!")
      cases -= 1
    }
  }
}
```

위의 예시코드를 확인할 수 있습니다.

	Scala의 새 버전(2.11.1)에서는 scala.readLine()함수 대신 scala.io.StdIn.readLine()을 사용해야 한다고 합니다. 정수를 입력 받을 때는 scala.io.StdIn.readInt().

코드를 살피던 중 위와 같은 댓글을 확인할 수 있었습니다. 버전차이로 인한 문제가 발생할 수 있을지도 모른다는 생각이었지만, 일단은 예시코드를 변형하여 답안을 제출하겠습니다.

```scala
object Main { 
  def main(args: Array[String]): Unit = {
    var cases = Integer.parseInt(readLine())
    while (cases > 0) {
      println("Hello Algospot!")
      cases -= 1
    }
  }
}
```

MERCY 문제의 경우 `Hello Algospot!`를 출력하면 되기 때문에, 예시코드에서 출력부분만 바꾸어서 제출하였습니다.

버전 차이로 인한 오답이 될 줄 알았지만, 무사히 `정답`으로 처리되었습니다.

![정답](https://goo.gl/XPz8Oj)

![소스](https://goo.gl/pZDUiw)