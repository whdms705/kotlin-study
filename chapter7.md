# 7. 문자열

## 부분 문자열 추출하기

### substring 함수

{% code title="Tavern.kt" %}
```kotlin
const val TAVERN_NAME = "Taernyl's Folly"

fun main(args: Array<String>) {
    placeOrder()
}

private fun placeOrder() {
    val indexOfApostrophe = TAVERN_NAME.indexOf('\'')
    val tavernMaster = TAVERN_NAME.substring(0 until indexOfApostrophe)
    println("마드리갈은 $tavernMaster 에게 주문한다.")
}
```
{% endcode %}

* indexOf\(구분자\) : 아포스트로피\('\)의 인덱스 구함
* substring\(IntRange\) : 문자열의 범위에 해당되는 문자열 추출

| 이스케이프 시퀀스 | 의미 |
| :--- | :--- |
| \t | 탭 \(tab\) 문자 |
| \b | 백스페이스 \(backspace\) 문자 |
| \n | 개행 \(newline\) 문자 |
| \r | 캐리지 리턴 \(carriage return\) |
| \" | 큰따옴표 \(double quotation mark\) |
| \' | 작은따옴표/아포스트로피\(single quotation mark/apostrophe\) |
| \\ | 역슬래쉬 \(backslash\) |
| \$ | 달러 기호 \(dollar sign\) |
| \u | 유니코드 \(unicode\) 문자 |

### split 함수

{% code title="Tavern.kt" %}
```kotlin
private fun placeOrder(menuData: String) {
    val indexOfApostrophe = TAVERN_NAME.indexOf('\'')
    val tavernMaster = TAVERN_NAME.substring(0 until indexOfApostrophe)
    println("마드리갈은 $tavernMaster 에게 주문한다.")

    val data = menuData.split(',')
    val type = data[0]
    val name = data[1]
    var price = data[2]
    // val (type, name, price) = menuData.split(',')
    val message = "마드리갈은 금화 $price 로 $name ($type)를 구입한다."
    println(message)
    
    var phrase = "와, $name 진짜 좋구나!"
    println("마드리갈이 감탄한다: ${toDragonSpeak(phrase)}")
}
```
{% endcode %}

* split\(구분자\) : 문자열의 각 부분을 별개의 문자열로 추출하여 생성
* 해체선언 : List에 저장된 각 요소를 하나의 표현식에서 다수의 변수로 지정                                                                                                                           val \(type, name, price\) = menuData.split\(','\)

## 문자열 변경하기

{% code title="Tavern.kt" %}
```kotlin
private fun toDragonSpeak(phrase: String) =
        phrase.replace(Regex("[aeiou]")) {
            when(it.value) {
                "a" -> "4"
                "e" -> "3"
                "i" -> "1"
                "o" -> "0"
                "u" -> "|_|"
                else -> it.value
            }
        }
```
{% endcode %}

* replace\(정규표현식, 익명 함수\) : 해당 정규표현식에 맞게 익명함수에서 문자열 변경

### 문자열은 불변이다

* 문자열은 var,val중 어느것으로 정의되든 코틀린의 모든 문자열\(String 타입\)은 자바처럼 불변이다.

## 문자열 비교

{% code title="Tavern.kt" %}
```kotlin
private fun placeOrder(menuData: String) {
    val indexOfApostrophe = TAVERN_NAME.indexOf('\'')
    val tavernMaster = TAVERN_NAME.substring(0 until indexOfApostrophe)
    println("마드리갈은 $tavernMaster 에게 주문한다.")

    val data = menuData.split(',')
    val type = data[0]
    val name = data[1]
    var price = data[2]
    // val (type, name, price) = menuData.split(',')
    val message = "마드리갈은 금화 $price 로 $name ($type)를 구입한다."
    println(message)
    
    val phrase = if (name == "Dragon's Breath") {
        "마드리갈이 감탄한다: ${toDragonSpeak("와, $name 진짜 좋구나!")}"
    } else {
        "마드리갈이 말한다: 감사합니다 $name."
    }
    println(phrase)
}
```
{% endcode %}

* 비교 연산자 == 사용 \( 문자열의 각 문자를 같은 순서로 하나씩 비교 \)
* 참조 동등 비교 === \( 힙 메모리 영역에 있는 같은 객체를 참조하는지 검사 \)
* 자바 비교 : 자바에서는 == 연산자가 두 문자열의 참조를 비교, 값을 비교할때는 equals 메서드 사용

