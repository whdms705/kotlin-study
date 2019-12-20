# 8. 숫자

### 목차

* [문자열을 숫자 타입으로 변환하기](문자열을-숫자-타입으로-변환하기)
* [Int 타입을 Double 타입으로 변환하기](Int-타입을-Double-타입으로-변환하기)
* [Double 타입 값의 형식 지정하기](Double-타입-값의-형식-지정하기)
* [Double 타입 값을 Int 타입 값으로 변환하기](Double-타입-값을-Int-타입-값으로-변환하기)
* [비트 연산](비트-연산)

## 1. 문자열을 숫자 타입으로 변환하기

코틀린에서 제공되는 문자타입 -> 숫자타입 변환 함수

1. toFloat
2. toDouble
3. toDoubleOrNull
4. toIntOrNull
5. toLong
6. toBigDecimal

```kotlin
fun main(args: Array<String>) {
    val todouble = "100"
    println(todouble.toDouble())
    
    val toNum = "100f"
    println(toNum.toLong())
    
    // null 복합연산자 사용하여 null 일 경우 0으로 반환
    val gold: Int = "5.91".toIntOrNull() ?: 0
    println(gold)
}
```

> 참고. "5.91" 문자열을 toInt() 함수를 사용할 경우 소수점 때문에 예외 발생.
  
  
## 2. Int 타입을 Double 타입으로 변환하기

Int 타입 값에 소수점 지원 타입 값과 연산을 수행하면 된다.  

```kotlin
fun main(args: Array<String>) {
    var playerGold = 10
    var playerSilver = 10
    val totalPurse = playerGold + (playerSilver / 100.0)
    
    println(totalPurse) // 10.1
}
```

## 3. Double 타입 값의 형식 지정하기

Double 타입의 결과값이 실수의 근사치를 표현하기 위해 4.18999999~ 로 나타난다.   
String 타입의 format 함수를 사용하면 실수 값의 형식을 변경할 수 있다.

```kotlin
fun main(args: Array<String>) {
    val remaininBalance = totalPurse - price
    println("남은 잔액: ${"%.2f".format(remaininBalance)}")  // 소수점 이하 두 자리까지 반올림
}
```

> 예시코드에 사용된 형식 문자열인 %.2f 는 다른 언어들의 표준 형식 문자열과 똑같으므로 자세한 내용은
자바 api 문서를 참고하면 된다.  
https://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html

## 4. Double 타입 값을 Int 타입 값으로 변환하기

kotlin에는 roundToInt(), roundToLong() 처럼 실수 값을 정수로 바꿔주는 함수가 존재.

예시코드.

```kotlin
fun main(args: Array<String>) {
    val remaininBalance = "100"
    val remainingGold = remaininBalance.toInt()    
    val remainingSilver = (remaininBalance % 1 * 100).roundToInt()
}
```

해당 함수를 사용하면 뒤에 소수점을 버리고 정수값만 취한다.


## 5. 비트 연산

kotlin에서 제공하는 비트 연산 함수들

예시코드.

```kotlin
fun main() {
    // 정숫값을 이진 형태의 문자열로 반환
    val a = Integer.toBinaryString(42)
    println(a) // 101010

    // 파라미터로 지정된 비트 수만큼 모든 비트를 왼쪽으로 이동 (부호비트는 그대로)
    val b = 42.shl(2)
    println(b) // 10101000

    // 파라미터로 지정된 비트 수만큼 모든 비트를 오른쪽으로 이동 (부호비트는 그대로)
    val c = 42.shr(2)
    println(c) // 1010

    // 파라미터로 지정된 비트 수만큼 모든 비트를 오른쪽으로 이동 (부호비트도 이동)
    val d = 42.ushr(2)
    println(d)

    // 모든 비트의 값을 반대로 변경
    val e = 42.inv()
    println(e)

    // xor, or, and 연산 수행
    val f = 42.xor(33)
    val g = 42.or(33)
    val h = 42.and(33)
    println("$f, $g, $h")
}
```


