title: '[scala] Hello World'
tags:
  - hello
  - scala
  - world
categories: []
date: 2016-01-05 21:16:00
---
[scala] Hello World

## IDE
http://scala-ide.org/ 에서 [Scala IDE](http://scala-ide.org/download/sdk.html)를 다운로드 하여 실행합니다.

## HelloWorld
```scala
object HelloWorld {
  def main(args: Array[String]) {
    println("Hello, world!")
  }
}
```
코드를 작성하고 실행합니다.

IDE에서 `HelloWorld` 프로젝트를 생성하고, HelloWorld Object 를 생성합니다. 코드를 작성하고 Scala Application으로 실행하면 결과를 확인할 수 있습니다.

	Hello, world!

![호출 순서](https://goo.gl/fVk1Nk)

HelloWorld 오브젝트를 실행하면, 안에 있는 main 함수가 호출되고, 그 안에서 println문이 실행됩니다.


## Timer
```scala
object Timer {
  def oncePerSecond(cb: () => Unit) {
    while (true) { cb(); Thread sleep 1000 }
  }
  def timeFlies() {
    println("time flies like an arrow...")
  }
  def main(args: Array[String]) {
    oncePerSecond(timeFlies)
  }
}
```

코드를 작성하고 실행합니다. 기존 프로젝트에, `Timer` Object 만 추가하고 작성해도 됩니다. 

	time flies like an arrow...
	time flies like an arrow...
	time flies like an arrow...


1초 마다 해당 문구열이 출력되는 것을 확인할 수 있습니다.

main 함수에서, timeFiles 함수를 파라미터로 oncePerSecond를 호출합니다. oncePerSecond 함수에서는 파라미터로 전달받은 timeFiles함수를 cb 객체에 담습니다.
while문 진입이후에는 cb 함수를 호출하고, 1초간 슬립을 하는 무한 루프를 돌게 됩니다. cb 함수 호출시에, `println("time flies like an arrow...")` 해당 문이 동작하게 되어, 같은 물자열이 계속 출력되게 됩니다.


## 참고
* [스칼라 학교](https://twitter.github.io/scala_school/ko/)
* [Effective Scala](http://twitter.github.io/effectivescala/)
* [자바 프로그래머를 위한 스칼라 튜토리얼](http://docs.scala-lang.org/ko/tutorials/scala-for-java-programmers.html)