# 4. 함수

## 기존 코드를 함수로 만들기

기존에 작성했던 코드를 함수로 재구성한다고 생각해보자.

{% tabs %}
{% tab title="Kotlin" %}
{% code title="Game.kt" %}
```kotlin
....
val healthStatus = when (healthPoints) {
        100 -> " 최상의 상태임!"
        in 90..99 -> " 약간의 찰과상만 있음."
        in 75..89 -> if (isBlessed) {
            " 경미한 상처가 있지만 빨리 치유되고 있음!"
        } else {
            " 경미한 상처만 있음."
        }
        in 15..74 -> " 많이 다친 것 같음."
        else -> " 최악의 상태임!"
}
....
```
{% endcode %}
{% endtab %}
{% endtabs %}

위와 같은 코드를 함수로 재구성하고 싶다면 해당 영역을 선택하고 Ctrl + Alt + M을 누르면 창이 뜨는데 name에 자신이 짓고 싶은 함수명을 적어주면 된다.

{% code title="Game.kt" %}
```kotlin
private fun formatHealthStatus(healthPoints: Int, isBlessed: Boolean): String {
    val healthStatus = when (healthPoints) {
        100 -> " 최상의 상태임!"
        in 90..99 -> " 약간의 찰과상만 있음."
        in 75..89 -> if (isBlessed) {
            " 경미한 상처가 있지만 빨리 치유되고 있음!"
        } else {
            " 경미한 상처만 있음."
        }
        in 15..74 -> " 많이 다친 것 같음."
        else -> " 최악의 상태임!"
    }
    return healthStatus
}
```
{% endcode %}

1번째 줄에 중괄호 나오기 전을 Header 그리고 중괄호부터 Body라고 한다.

* Header - 가시성 제한자, 함수 선언 키워드, 함수 이름, 매개 변수, 반환 타입
* 가시성 제한자 - 자바에서의 접근제어지시자를 생각하면 되겠다.
* 함수 이름 - 자바처럼 카멜표기법을 사용하는 것이 일반적
* 매개변수 - 변수 이름 : 타입

Kotlin은 함수를 사용할 때 매개변수 선언부에서 값을 설정해주면 매개변수에 넘어온 인자가 없을 때 해당 값이 default값으로 사용된다.

{% code title="Game.kt" %}
```kotlin
private fun castFireball(numFireballs: Int = 2) {
    println("한 덩어리의 파이어볼이 나타난다. (x$numFireballs)")
}

```
{% endcode %}

call 할때 castFireball\(\)하고 실행하면 해당 함수에 n라는 값으로 대체되어 실행된다.

## 단일 표현식 함수

기존에 자바를 공부했다면 void를 알 것이다. 이처럼 kotlin에서도 함수 body를 나타내는 중괄호, 그 안에 return 문을 전부 생략가능하다.

```kotlin
private fun formatHealthStatus(healthPoints: Int, isBlessed: Boolean) =
    when (healthPoints) {
        100 -> " 최상의 상태임!"
        in 90..99 -> " 약간의 찰과상만 있음."
        in 75..89 -> if (isBlessed) {
            " 경미한 상처가 있지만 빨리 치유되고 있음!"
        } else {
            " 경미한 상처만 있음."
        }
        in 15..74 -> " 많이 다친 것 같음."
        else -> " 최악의 상태임!"
    }
private fun castFireball(numFireballs: Int = 2) =
    println("한 덩어리의 파이어볼이 나타난다. (x$numFireballs)")
```

## ​Unit 함수

Kotlin에서는 이러한 함수를 Unit 함수라고 한다. 의미하는 것은 반환 타입이 Unit이라는 것인데 아무 값도 반환하지 않는 함수를 말한다. 

자바와 같은 언어에서는 void가 있겠다. 하지만 이 void는 중요한 제네릭을 처리하기 어렵다. 제네릭은 반환 타입을 무조건 타나내야 하기 때문에 void를 사용하면 마땅히 처리할 방법이 없지만 Kotlin은 이러한 함수를 Unit이라는 반환 타입으로 정의하고 제네릭 함수에서도 사용가능하도록 했다.

## 지명 함수 인자

Kotlin에서 함수를 호출할 때 지명 함수 인자를 사용하면 매개 변수가 정의된 순서를 따르지 않아도 호출할 수 있다. 매개 변수가 많아서 그 순서를 기억하기 힘들거나 변수의 의미를 명확하게 보여주기 위해 사용하면 된다.

printPlayerStatus\(auraColor = "NONE", isBlessed = true\) 이런식으로 말이다.

## Nothing 타입

Nothing 타입 또한 Unit 타입과 같이 값을 반환하지 않는 함수를 나타낼 때 사용된다. 하지만 차이점은 Nothing은 의도적 예외를 발생시키기 위해 사용되는 타입이라는 것이다.

```kotlin
fun shouldReturnAString() : String {
    TODO("문자열을 반환하는 코드를 여기에 구현해야 함")
}
```

TODO는 kotlin 표준 라이브러리 함수이며 저 해당 코드는 필요한 함수이지만 아직 구현이 없을 경우에 보류할 경우에 사용한다. 반환 타입이 String으로 되어있지만 아무것도 반환하지 않기때문에 컴파일러는 에러로 처리해야하지만 그렇지 않고 넘어가게 된다. 

## 백틱 함수 이름

Kotlin은 \`함수 이름\` 과 같이 백틱 기호로 표현하면 그 안에 공백과 특수문자를 사용해도 함수를 정의할 수 있다.

예를 들어 \`\*\*~prolly not a good idea!~\*\*\` 과 같은 이름의 함수를 정의할 수 있다는 말이다. 백틱을 사용하는 이유는 두 가지가 있다.

* 첫번 째, 자바와의 상호운용 때문이다. 코틀린 파일에서 자바의 Method를 호출할 수 있다. 하지만 예약어가 다르기 때문에 이러한 충돌을 피하기 위해서 사용한다 예를 들면 Java에서는 is가 예약어가 아니지만 Kotlin에서는 예약어이다. 그래서 Java의 Method명이 Kotlin의 예약어와 같다면 백틱을 활용할 수 있다.
* 두번 째, 함수 이름을 더 알기 쉽게 나타내기 위해서 사용한다. 하지만 일반적으로는 카멜 표기법이 사용되기 때문에 굳이 사용하지 않는다.



