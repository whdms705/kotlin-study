# 6장 null 안전과 예외
[예제소스 github](https://github.com/whdms705/kotlinNote/tree/master/src/ch06).

## 코틀린의 null 처리 개요

* 코틀린에는 null 값이 지정될 수 있는 요소가 있는가 하면 그렇지 않은 요소도 있다.

* String은 null 불가능 타입이며 컴파일러가 null 값의 지정을 막는다.

``` kotlin

    var signatureDrink = "맥주"
    signatureDrink = null

```


* 코틀린은 null 값을 가질 수 있다고 특별히 지정되지 않으면 변수가 null을 가질수 없다.
따라서 , null 값으로 생기는 문제를 런타임이 아닌 컴파일 시점에 방지할 수 있다.


## 코틀린의 명시적 null 타입

``` kotlin

    var beverage = readLine()
        beverage = null
        println(beverage)

```

## null 안전 처리

*코틀린은 null가능 타입과 불가능 타입을 구분한다.

*null가능 타입의 변수를 사용하거나 또는 null 가능 타입을 반환하는 함수의 반환값을 사용할 때 위험할 수 있는 경우를 컴파일러가 안다.

* 가능한 한  null 불가능 타입을 사용하자
    null 불가능 타입은 항상 null이 아닌 어떤 값을 갖는다는 것이 보장된다.
    null 값의 안전한 처리를 위한 추가 코드가 필요 없다.
    

`null가능 변수에 null불가능 함수 호출을 한다면?   ` 



``` kotlin

    // capitalize() 문자열의 첫 자만 대문자로 바꿔 준다.
    // realLine의 반환 타입이 String? 이므로 null 값을 반환할 수 있기 때문에 capitalize() 호출 못하게 막음
    var beverage = readLine().capitalize()

    println(beverage)
    println(beverage)

```



* null 가능 타입을 안전하게 처리하는 방법 3가지


### 첫번째 방법 : 안전 호출 연산자

* 안전 호출 연산자 ?.

함수 호출에 사용되는 변수나 다른 함수(여기서는 readLine)의 반환값이 null이 아닐 때만 해당 함수가 안전하개 호출되므로 nullPointException을 방지 할 수 있다.


* 안전 호출 연산자와 함께 let 함수 사용하기

해당 타입의 값이 null이 아닐 때 변수에 새로운 값을 지정하거나 다른 함수를 호출하는 등의 추가 작업을 수행하고 싶다면 , 
이때는 let 함수에 안전 호출 연산자를 사용하는 것이 한 가지 방법이다.

``` kotlin
var beverage = readLine()?.let { 
        if(it.isNotBlank()){
            it.capitalize()
        }else{
            "맥주"
        }
    }

```

1. readLine 함수의 반환값이 null이 아니면 let이 호출되며
2. 중괄호로 전달된 익명함수 실행
3. readLine의 반환값이 빈 문자열이 아니면 해당 값의 첫 자를 대문자로 변환 후 반환
4. 빈 문자열인 경우 "맥주"를 반


### 두번째 방법 : non-null 단언 연산자

non-null 단언 연산자인 !! 도 null 가능 타입에 대해 함수를 호출하기 위해 사용될 수 있다.
이연산자는 null이 될 수 없다는 것을 단언(assertion)하는 연산자이다.

>> null이 아니면 정상적으로 코드를 실행
>> null이면 런타임 시에 `NullpointExcetion` 예외를 발생시킨다.


``` kotlin
    var beverage = readLine()!!.capitalize()
    
    
    println(beverage)

```



### 세번째 방법 : 값이 null인지 if로 검사하기

``` kotlin

    var beverage = readLine()
    
    beverage = null

    if(beverage != null){
        beverage = beverage.capitalize()
    }else{
        println("beverage가 null입니다.")
    }

```

* 값이 null일 때 실행되는 코드가 복잡할 경우에는 if/else를 사용하면 우리가 알기 쉬운 형태로 복잡한 로직을 나타낼 수 있다.
* 복잡한 경우가 아니라면 안전 호출 연산자를 사용하면 코드가 간결해지고 유연하지기 때문에 안전 호출 연산자를 사용
* 안전 호출 연산자는 연속적으로 함수를 호출할 수 있다.


``` kotlin

    beverage?.capitalize()?.plus(",large")

```


#### null 복합연산자 > ?:

>> 왼쪽 피연산자의 결과가 null이면 오른쪽의 피연산자를 실행
>> null이 아니면 왼쪽 피연산자의 결과를 반환한다.

* null 복합연산자를 이용해서 beverage의 값이 null이면 기본값인 "맥주"를 출력하도록하는 코드


``` kotlin

    var beverage = readLine()
    beverage = null

    if(beverage != null){
        beverage = beverage.capitalize()
    }else{
        println("beverage가 null입니다.")
    }

    val beverageServed: String = beverage ?: "맥주"
    println(beverageServed)

```

* null 복합 연산자는 let 함수에 전달되는 표현식의 if/else 대신 사용될 수 있다.

``` kotlin

    var beverage = readLine()
    
    beverage?.let { 
        beverage = it.capitalize()
    } ?: println("beverage가 null 입니다.")

```




## 예외

* 예외상황 : null 가능 변수에 !! 연산자를 사용하는 것은 위험하다.



``` kotlin

    fun main(args: Array<String>) {
        var swordsJuggling : Int? = null
        val isJugglingProfficinet = (1..3).shuffled().last() == 3
        if(isJugglingProfficinet){
            swordsJuggling = 2    
        }
        
        swordsJuggling = swordsJuggling!!.plus(1);
        
        println(swordsJuggling)
    }

```

>> 코드에서는 1부터 3까지 세 번의 기회중에서 한 번(3일때)은 칼의 개수가 2가 되지만 나머지 두번은 null이 되므로 예외가 발생

>> swordsJuggling 변수의 값이 null일때 nullPointException이 발생하면서 프로그램이 크래시
코틀린에서 변수의 기본 타입이 null 불가능인 이유때문


`미처리 예외`
처리되지 않는 예외

`크래시 `
프로그램 실행이 중단되는 것


### 예외 던지기

####  코틀린에서 제공해주는 예외
* IllegalStateException 던지기



``` kotlin

    fun main(args: Array<String>) {
        var swordsJuggling : Int? = null
        val isJugglingProfficinet = (1..3).shuffled().last() == 3
        if(isJugglingProfficinet){
            swordsJuggling = 2
        }
    
        proficiencyCheck(swordsJuggling)
        swordsJuggling = swordsJuggling!!.plus(1);
    
        println("$swordsJuggling 개의 칼로 저글링합니다. ")
    }
    
    fun proficiencyCheck(swordsJuggling: Int?){
        swordsJuggling ?: throw IllegalStateException("플레이어가 저글링을 할 수 없음")
    }

```

#### 커스텀 예외 던지기

* 커스텀 예외 정의하기

``` kotlin

    fun main(args: Array<String>) {
        var swordsJuggling : Int? = null
        val isJugglingProfficinet = (1..3).shuffled().last() == 3
        if(isJugglingProfficinet){
            swordsJuggling = 2
        }
    
        proficiencyCheck(swordsJuggling)
        swordsJuggling = swordsJuggling!!.plus(1);
    
        println("$swordsJuggling 개의 칼로 저글링합니다. ")
    }
    
    fun proficiencyCheck(swordsJuggling: Int?){
        //swordsJuggling ?: throw IllegalStateException("플레이어가 저글링을 할 수 없음")
        swordsJuggling ?: throw UnskilledSwordJugglerException()
    }
    
    class UnskilledSwordJugglerException() :
            IllegalStateException("플레이어가 저글링을 할 수 없음")

```

#### 예외 처리

* 예외처리가 되지 않으면 복구 불가능한 상태를 나타내는 것이기 때문에 예외처리를 해준다.

* try/catch 문에서 예외를 처리했으므로 프러그램이 계속 실행되면서 catch 블록의 println이 실행되고
try/catch 문을 벗어나서 그다음에 있는 println이 실행되서 다음 결과가 출력된다.

``` kotlin

    fun main(args: Array<String>) {
        var swordsJuggling : Int? = null
        val isJugglingProfficinet = (1..3).shuffled().last() == 3
        if(isJugglingProfficinet){
            swordsJuggling = 2
        }
    
        try{
            proficiencyCheck(swordsJuggling)
            swordsJuggling = swordsJuggling!!.plus(1);
        }catch (e:Exception){
            println(e)
        }
    
        println("$swordsJuggling 개의 칼로 저글링합니다. ")
    }
    
    fun proficiencyCheck(swordsJuggling: Int?){
        swordsJuggling ?: throw UnskilledSwordJugglerException()
    }
    
    class UnskilledSwordJugglerException() :
            IllegalStateException("플레이어가 저글링을 할 수 없음")

```

## 전제조건

*예기치 않은 값으로 인해 우리가 의도하지 않는 방향으로 프로그램이 실행될 수 있다.
따라서 , 코드를 작성할 때는 입력값을 검사하는데 많은 노력이 필요하다.

`전제 조건 함수`
일부 코드가 실행되기 전에 충족되어야 하는 전제 조건을 정의할 수 있다.




* 전제 조건 함수 사용하기
>> null 값을 검사하여 UnskilledSwordJugglerException을 던지는 코드를 checkNotNull 함수로 교체해보는 예제


`checkNotNull`

첫번째 인자는 null을 검사할 값
두번째는 첫번째 인자가  null일 때 콘솔에 출력할 메시지



``` kotlin

    fun main(args: Array<String>) {
        var swordsJuggling : Int? = null
        val isJugglingProfficinet = (1..3).shuffled().last() == 3
        if(isJugglingProfficinet){
            swordsJuggling = 2
        }
    
        try{
            proficiencyCheck(swordsJuggling)
            swordsJuggling = swordsJuggling!!.plus(1);
        }catch (e:Exception){
            println(e)
        }
    
        println("$swordsJuggling 개의 칼로 저글링합니다. ")
    }
    
    fun proficiencyCheck(swordsJuggling: Int?){
        // 전제 조건 함수 chdeckNotNull사용
        checkNotNull(swordsJuggling,{"플레이어가 저글링을 할 수 없음2"})
    }

```



## null: 무엇이 좋을까?

*null은 기본값이 없는 변수의 초깃값으로 자주 사용되며 , 실제로 많은 언어에서 값을 지정하지 않고 변수를 정의할 수 있으며
이런 변수의 값은 기본으로 null이 된다.

*하지만 null 기본값으로 지정되는 방식으로 인해 다른 언어에서는 nullPointException이 발생한다.

*코틀린에서는 null을 안전하게 사용할 수 있는 방법을 제공한다.


## 궁금증 해소하기 : checked 예외와 unchecked 예외

*코틀린에서는 모든 예외가 unchecked 예외다.

*checked 예외는 발생했더라도 특별히 할 수 있는게 없다.
ex). chatch(e:IOEception){} : 경보가 울렸는데 무시하는 것과 같다
>> 이렇게 되면 실제로 뭔가 잘못되어 문제가 생겼을 때 그 정보를 무시하는 것이 되므로 원인을 찾기 어렵다.

*checked 예외는 문제를 해결하기보다는 오히려 더 많은 문제를 야기하므로 코틀린을 포함한 현대의 언어에서는 unchecked 예외를 지원한다.

