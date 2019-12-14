# 3. 조건문과 조건식

### 목차

* [java에 없는 비교연산자](chapter3.md#java에-없는-비교연산자)
* [조건 표현식](chapter3.md#조건-표현식)
* [범위 표현](chapter3.md#범위-표현)
* [when 표현](chapter3.md#when-표현)
* [문자열 템플릿](chapter3.md#문자열-템플릿)
* [기타 범위함수](chapter3.md#기타-범위함수)

## 1. java에 없는 비교연산자

| 값 | 의미 |
| :--- | :---: |
| `===` | 왼쪽, 오른쪽 두 개 인스턴스가 같은 객체를 참조하는지 검사 |
| `!==` | 왼쪽, 오른쪽 두 개 인스턴스가 같은 객체를 참조하지 않는지 검사 |

> 참고.  
> java에서는 위와 같은 비교연산자가 없기 때문에
>
> * 두 개의 인스턴스가 같은 객체 주소 인지 검사하기 위해 == 연산자를 사용하고  
> * 객체의 값 자체를 비교하기 위해서 equals\(\) 메소드를 사용하거나 또는 equals\(\) 메소드를 재정의하여 사용한다.  
>
>   > java에서 equals\(\) 를 재정의할 때는 hashCode\(\)도 무조건 재정의 필요.

## 2. 조건 표현식

kotlin에서는 조건에 맞는 값을 변수 초기화 시에 할당할 수 있다.

예시코드. \(java\)

```java
public class Java {
    public static void main(String[] args) {
        boolean auraVisible = true;
        String auraColor = "";
        if(auraVisible) {
            auraColor = "GREEN";
        } else {
            auraColor = "NONE";
        }

        System.out.println(auraColor);
    }
}
```

예시코드. \(kotlin\)

```kotlin
fun main(args: Array<String>) {
    val auraVisible = true
    val auraColor = if (auraVisible) "GREEN" else "NONE"
    println(auraColor)
}
```

## 3. 범위 표현\(range\)

kotlin에서는 **in** 이라는 키워드와 **..연산자** 로 값의 범위를 표현할 수 있다.  
1..5 는 1, 2, 3, 4, 5 까지의 범위를 표현한 것이다.

범위 표현 예시코드.

```kotlin
fun main(args: Array<String>) {
    val healthPoints = 80
    if(healthPoints in 90..99) {
    } else if (healthPoints in 80..89) {
    } 
}
```

범위 표현 예시코드\(with. 조건 표현식\)

```kotlin
fun main(args: Array<String>) {
    val healthPoints = 80
    val healthStatus = if(healthPoints in 90..99) {
        "최상의 상태"
    } else if (healthPoints in 80..89) {
        "약간의 찰과상"
    }

    println(healthStatus)
}
```

## 4. when 표현

kotlin에 있는 제어흐름으로 java의 switch문과 같은 동작을 한다.

예시코드.

```kotlin
fun main(args: Array<String>) {
    val race = "gnome"
    val faction = when (race) {
        "dwarf" -> "..."
        "gnome" -> "..."
        else -> "..."
    }
}
```

예시코드. \(with. 범위 표현\)

```kotlin
fun main(args: Array<String>) {
    val karma = 14
    val auraColor = when (karma) {
            in 0..5 -> "..."
            in 6..10 -> "..."
            in 11..15 -> "..."
            in 16..20 -> "..."
            else -> "..."
        }
}
```

예시코드. \(with. 중첩 조건\)

```kotlin
fun main(args: Array<String>) {
    val karma = 14
    val status = true
    val auraColor = when (karma) {
            in 0..5 -> "..."
            in 6..10 -> when (status) {
                true -> "..."
                else -> "..."
            }
            in 11..15 -> "..."
            in 16..20 -> "..."
            else -> "..."
        }
}
```

## 5. 문자열 템플릿

kotlin에서는 문자열 값안에 변수의 값을 포함시킬 수 있다.

예시코드.

```kotlin
fun main(args: String<Array>) {
    val name = "name"
    val status = "status"
    val check = true

    // 기본 java 형식
    println("name :" + name + " , status : " +status)               // name name , status : status

    // kotlin의 문자열 템플릿
    println("name $name , status : $status")                        // name name , status : status

    // kotlin의 문자열 템플릿 (with. 표현식)
    println("name $name , status : ${if (check) status else ""}")   // name name , status : status
}
```

## 6. 기타 범위함수

kotlin에서는 범위를 표현 할 수 있는 여러가지 함수를 기본 제공해준다.  
1. toList\(\) 2. downTo 3. until

예시코드.

```kotlin
fun main(args: String<Array>) {
    // 1. toList()
    println((1..3).toList())  // [1, 2, 3]

    // 2. downTo
    println(1 in 3 downTo 1)  // true

    // 3. until
    println(1 in 1 until 3)   // true
    println(3 in 1 until 3)   // false
}
```

