# 9. 표준 함수

코틀린 라이브러리에 있는 표준 함수는 보편적으로 사용될 수 있는 유틸리티 함수이며, 람다를 인자로 받아 동작한다.

코틀린에서 주로 사용되는 표준 함수의 종류

* apply
* let
* run
* with
* also
* takeIf

코틀린의 표준 함수는 내부적으로는 확장 함수\(Extension Function\)이며, 확장 함수를 실행하는 주체를 수신자 또는 수신자 객체라고 한다. 이러한 확장함수를 사용하면 특정 타입\(ex. class\)을 변경하지 않고 해당 타입에 새로운 기능을 추가할 수 있다. 확장과 확장 함수에 대해서는 Chapter 18 에서 배우겠지만 간단히 어떤 느낌인지만 보자.

```kotlin
fun main() {
    val car = Car()
    println(car.getBrandName())
}

class Car {
    fun getPrice() : Int {
        return 10000
    }
}

fun Car.getBrandName() : String {
    return "BMW"
}
```

Car 클래스에는 존재하지 않는 getBrandName\(\) 이라는 함수가 만들어졌다. Car클래스에 멤버를 추가시키거나 기존 함수를 수정하지도 않았다. 이런게 바로 확장이라고 한다.

## apply

구성 함수라고 생각할 수 있다. 우리가 사용할 객체를 구성할 때 반복되는 코드의 양을 줄이기 위해 사용된다.

```kotlin
//before use the apply
val menuFile = File("menu-file.txt")
    menuFile.setReadable(true)
    menuFile.setWritable(true)
    menuFile.setExecutable(true)

//after use the apply
//여기서 수신자 객체는 File 객체가 된다.
val menuFile = File("menu-file.txt").apply {
    setReadable(true)
    setWritable(true)
    menuFile.setExecutable(true)
}
```

이처럼 apply를 사용하면 변수명을 생략하고 함수를 호출할 수 있는 것을 볼 수 있다. 이것이 가능한 이유는 해당 수신자에 대한 모든 함수 호출이 가능하도록 apply 함수가 사용 범위를 설정해주기 때문이다.

람다 내부의 모든 함수 호출이 수신자에 관련되어 호출되므로 이와 같은 경우를 **연관범위** 또는 수신자에 대한 **암시적 호출**이라고도 한다.

## let

let은 함수의 인자로 전달된 람다를 실해한 후 결과를 반환해 준다. 이때 it 키워드를 사용해서 let을 호출한 수신자 객체를 참조할 수 있다.

```kotlin
//List에 첫 번째 요소의 제곱근을 구하는 코드
//let을 사용하지 않는다 아래처럼 구할 것
val firstElement = listOf(1,2,3).first()
val firstItemSquared = firstElement * firstElement

//let을 사용한다면
val firstItemSquared = listOf(1,2,3).first().let {
    it * it
}
```



또한 let은 null 복합 연산자와 같이 사용하면 null 가능 타입에서 발생될 수 있는 예외를 방지하고 null일 때 기본값을 지정할 수 있다.

```kotlin
//let을 사용하지 않는다
fun formatGreeting(vipGuest: String?): String {
    return if (vipGuest != null) {
        " 오랜만입니다, $vipGuest. 테이블이 준비되어 있으니 들어오시죠."
    } else {
        " 저희 술집에 오신 것을 환영합니다. 곧 자리를 마련해 드리겠습니다."
    }
}

//let을 사용할 경우라면
fun formatGreeting(vipGuest: String?): String {
    return vipGuest?.let {
        " 오랜만입니다, $it. 테이블이 준비되어 있으니 들어오시죠."
    } ?: " 저희 술집에 오신 것을 환영합니다. 곧 자리를 마련해 드리겠습니다."
}
```

여기서 let과 apply의 차이점을 보자면 일단 let은 수신자 객체를 람다로 전달한다. 하지만 apply는 아무것도 전달하지 않는다. 또한, apply는 람다의 실행이 끝나면 현재의 수신자 객체를 반환한다. 반면에 let은 람다에 포함된 마지막 코드 줄의 실행 결과를 반환한다. 

또한 let은 변수의 값이 예기치 않게 변경되는 위험을 줄일때도 사용된다. let이 람다에 전달하는 인자는 읽기 전용의 함수 매개변수이기 때문이다. 이와 같은 응용은 chapter 12 에서 더 알아 볼 것이다.

## run

run은 apply와 동일한 연관 범위를 제공하여 비슷해 보이지만 apply가 수신자 객체를 반환하는 반면 run은 그렇지 않다.

```kotlin
//파일에 특정 문자열이 포함되어 있는지 검사하는 코드
val menuFile = File("menu-file.txt")
val servesDragonsBreath = menuFile.run {
    readText().contains("Dragon's Breath")
}
```

apply에서 보았던 것처럼 readText함수는 암시적으로 수신자인 File객체에 대해 수행되기 때문에 변수명을 생략하고 사용할 수 있다. 하지만 여기서는 수신자 객체가 반환되는 것이 아닌 람다의 결과가 반환된다. 이 함수에서는 true / false 이다.

또한 run은 수신자에 대한 함수 참조를 실행하기 위해 사용될 수 있다.

```kotlin
fun nameIsLong(name: String) = name.length >= 20

"Madrigal".run(::nameIsLong) // false
"Polarcubis, Supreme Master of NyetHack".run(::nameIsLong) // true
```

물론 위 코드는 nameIsLong\("Madrigal"\)을 호출한 것과 동일하지만 함수 호출이 여러 개 있을 경우에는 run을 사용하면 편리하다.

```kotlin
fun nameIsLong(name: String) = name.length >= 20
fun playerCreateMessage(nameTooLong: Boolean): String {
    return if (nameTooLong) {
        "Name is too long. Please choose another name."
    } else {
        "Welcome, adventurer"
    }
}
//run사용할 경우
"Polarcubis, Supreme Master of NyetHack"
    .run(::nameIsLong)
    .run(::playerCreateMessage)
    .run(::println)

//run 사용하지 않은 경
pintln(playerCreateMessage(nameIsLong("Polarcubis, Supreme Master of NyetHack")))
```

확실히 가독성 면에서 run을 사용한 코드가 알아보기 편하다.

## with

run과 동일하게 동작하지만 호출 방식이 다르다. 지금까지 보았던 표준 함수들은 수신자 객체로 호출하였다. 그러나 with는 수신자 객체를 첫 번째 매개변수의 인자로 받는다.

```kotlin
val nameTooLong = with("Polarcubis, Supreme Master of NyetHack") {
    legth >= 20
}

```

하지만 다른 표준 함수들과 일관성이 없으므로 with 대신 run을 사용할 것을 권장한다.

## also

let과 비슷하게 동작한다. also도 let처럼 호출한 수신자 객체를 람다의 인자로 전달한다. 하지만 also는 람다의 결과가 아닌 수신자 객체를 반환한다.

예를 들면 두 가지의 서로 다른 처리를 also를 사용해서 연쇄 호출이 가능하다. 즉 하나는 파일 이름을 출력하고, 다른 것은 파일의 내용을 fileContents 변수에 저장한다.

```kotlin
var fileContents: List<String>
File("file.txt")
    .alse {
        print(it.name)
    }
    .alse {
        fileContents = it.readLines()
    }
}
```

## takeIf

다른 표준 함수와는 약간 다르게 동작한다. 람다에 제공된 조건식을 실행한 후 결과에 따라 true 또는 false를 반환한다. 그리고 조건식의 결과가 true면 수신자 객체가 반환되며, false면 null을 반환한다.

```kotlin
//takeIf를 사용하지 않으면
val file = File("myfile.txt")
val fileContents = if(file.canRead() && file.canWrite()) {
    file.readText()
} else {
    null
}

//takeIf를 사용한다면
val fileContents = File("myfile.txt")
    .takeIf { it.canRead() && it.canWrite() }
    ?.readText()
```

takeIf는 File객체를 참조하는 변수가 필요 없고, null을 반환하는 코드 또한 필요 없다. 따라서 변수에 값을 지정하는 데 필요한 어떤 조건, 또는 처리를 계속하기 전에 만족되어야 하는 조건을 검사하는 데 유용하다.

## takeUnless

takeIf를 반대되 함수이다. takeIf와 똑같지만 지정한 조건식이 false일 경우 원래 값을 반환한다.

다음 예는 파일의 속성이 숨김이 아니면 파일을 읽으며, 그렇지 않으면 null을 반환한다.

```kotlin
val fileContents = File("myfile.txt").takeUnless { it.isHidden }?.readText()
```

* 만일 조건이 true면 해당 값을 반환해라 - takeIf
* 만일 조건이 false면 해당 값을 반환해라 - takeUnless

위의 예처럼 간단한 조건의 경우에는 takeUnless를 사용해도 무방하지만 복잡한 조건을 검사할 때는 takeUnless보다는 takeIf가 더 이해하기 쉬기 때문에 takeIf를 사용할 것을 권한다.

## 정리

<table>
  <thead>
    <tr>
      <th style="text-align:center">&#xD568;&#xC218;</th>
      <th style="text-align:center">&#xC218;&#xC2E0;&#xC790; &#xAC1D;&#xCCB4;&#xB97C; &#xB2E4;&#xC758; &#xC778;&#xC790;&#xB85C;
        &#xC804;&#xB2EC;&#xD558;&#xB294;&#xAC00;?</th>
      <th style="text-align:center">&#xC5F0;&#xAD00; &#xBC94;&#xC704;&#xB97C; &#xC81C;&#xACF5;&#xD558;&#xB294;&#xAC00;?</th>
      <th
      style="text-align:center">&#xBC18;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center">let</td>
      <td style="text-align:center">o</td>
      <td style="text-align:center">x</td>
      <td style="text-align:center">&#xB78C;&#xB2E4;&#xC758; &#xACB0;&#xACFC;</td>
    </tr>
    <tr>
      <td style="text-align:center">apply</td>
      <td style="text-align:center">x</td>
      <td style="text-align:center">o</td>
      <td style="text-align:center">&#xC218;&#xC2E0;&#xC790; &#xAC1D;&#xCCB4;</td>
    </tr>
    <tr>
      <td style="text-align:center">run</td>
      <td style="text-align:center">x</td>
      <td style="text-align:center">o</td>
      <td style="text-align:center">&#xB78C;&#xB2E4;&#xC758; &#xACB0;&#xACFC;</td>
    </tr>
    <tr>
      <td style="text-align:center">with</td>
      <td style="text-align:center">x</td>
      <td style="text-align:center">o</td>
      <td style="text-align:center">&#xB78C;&#xB2E4;&#xC758; &#xACB0;&#xACFC;</td>
    </tr>
    <tr>
      <td style="text-align:center">also</td>
      <td style="text-align:center">o</td>
      <td style="text-align:center">x</td>
      <td style="text-align:center">&#xC218;&#xC2E0;&#xC790; &#xAC1D;&#xCCB4;</td>
    </tr>
    <tr>
      <td style="text-align:center">takeIf</td>
      <td style="text-align:center">o</td>
      <td style="text-align:center">x</td>
      <td style="text-align:center">
        <p>&#xC218;&#xC2E0;&#xC790; &#xAC1D;&#xCCB4;</p>
        <p>null &#xAC00;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:center">takeUnless</td>
      <td style="text-align:center">o</td>
      <td style="text-align:center">x</td>
      <td style="text-align:center">
        <p>&#xC218;&#xC2E0;&#xC790; &#xAC1D;&#xCCB4;</p>
        <p>null &#xAC00;&#xB2A5;</p>
      </td>
    </tr>
  </tbody>
</table>

