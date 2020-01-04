# 11장 Map
[예제소스 github](https://github.com/whdms705/kotlinNote/tree/master/src/ch11).

## Map

* 키와 값의 쌍으로 데이터가 저장되며 , 정수를 사용한 인덱스 기반 처리 대신 키를 기반으로 데이터를 처리한다.

* Map의 키는 고유하다.

* 키와 대응되는 값으 고유하지 않아도 된다.



### Map 생성하기

* Map은 mapOf , mutableMapOf 함수를 사용하여 생성한다.

* mapOf , mutableMapOf 함수 차이점은?



`읽기 전용 Map 생성하기`

``` kotlin

    val partronGold = mapOf("Elo" to 10.5 ,"Mordoc" to 8.0 ,"Sophie" to 5.5 )
    
    fun main(args: Array<String>) {
        println(partronGold)
    }

```

* to 함수 대신에 Pair 타입을 사용해서 Map 항목 생성할 수 있다.

``` kotlin

    
    val partronGold2 = mapOf(
        Pair("Eli",10.5),
        Pair("Mordoc",8.0),
        Pair("Sophie",6.5)
    );
    
    fun main(args: Array<String>) {
        println(partronGold2)
    }

```

>> 그러나 to 함수를 사용하는 것이 더 간결하고 알기 쉽다고 한다.


`중복된 키 추가하기해보기`

``` kotlin

    fun main(args: Array<String>) {
        // 중복된 키 추가하기
        val partronGold3 = mutableMapOf("Elo" to 10.5 ,"Mordoc" to 8.0)
        println(partronGold3)
    
        partronGold3 +="Mordoc" to 11.0
    
        println(partronGold3)
    
    }

```

>> += 연산자를 사용해서 Map에 항목을 추가했지만 이미 있는 키는 새로 추가되지 않고 키에 대응하는 값이 변경된다.
{Elo=10.5, Mordoc=8.0} <br>
{Elo=10.5, Mordoc=11.0}



### Map에서 값 가져오기

#### Map에서 값을 가져오는 함수

* []인덱스
* getVale
* getOrElse
* getOrDefault

``` kotlin

    val partronGold = mapOf("Elo" to 10.5 ,"Mordoc" to 8.0 ,"Sophie" to 5.5 )
    
    fun main(args: Array<String>) {
    
        //Map에서 값 가져오기
    
        // 키의 값을  가져오며 , 키가 없으면 null을 반환한다.
        println(partronGold["Elo"])
        println(partronGold["Mordoc"])
        
        //getValue 키의 값을 가져오며, 키가 없으면 예외를 발생시킨다.
        println(partronGold.getValue("Elo"))
        println(partronGold.getValue("aaa"))
    
        //getOrElse 키의 값을 가져오며, 키가 없으면 지정된 익명함수를 실행해서 결과값 반환한다.
        println(partronGold.getOrElse("aaaa"){"No such patron"})
    
    
        // getOrDefault 키의 값을 가져오거나 , 또는 키가 없으면 우리가 지정한 기본값을 반환한다.
        println(partronGold.getOrDefault("aaa",0.0))
        
    }

```


### Map에 항목 추가하기

>> 수정가능한 Map을 만들고 싶다면 mutableListOf 

#### 변경 가능한 Map의 변경자 함수

* []인덱스
* +=
* put
* putAll
* getOrPut
* remove
* -
* -=
* clear



``` kotlin

    fun main(args: Array<String>) {
        //Map에 항목 추가하기
        val name = mutableMapOf<String,Double>("ka" to 0.5)
        name.put("Kim",5.5)
        name.put("Jo",3.5)
    
    
        println(name.toString())
    
    }

```



`코틀린 컬렉션 요약`

|컬렉션 타입|요소의 순서가 있는가?|요소가 고유한가?|저장 단위|해체 선언 지원?|
|---|---|---|---|---|
|List|Yes|NO|요소|YES|
|Set|NO|Yes(요소)|요소|NO|
|Map|NO|Yes(키)|항목(키와 값의 쌍)|NO|

