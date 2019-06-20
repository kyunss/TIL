### 2.4 자료/자료형의 확인 및 변환

- 코틀린에서는 원시타입과 객체 모두 __==__ 연산자를 사용하여 비교할 수 있다.

  ```kotlin
  val foo: Int = ...
  val bar: Int = ...
  //foo와 bar의 값을 비교한 값을 equals에 할당한다.
  val equals: Boolean = foo == bar 
  //의사코드는 아래와 같다.
  if(foo가 널이 아니라면){
    foo.equals(bar)의 결과 반환
  } else{
    bar == null 결과 반환
  }
  ```

  - 위 코드에서 볼 수 있듯이, __==__ 연산자는 비교하는 값의 널 여부를 함께 확인한다.
  - __객체 자체가__ 동일한지 확인할 경우 __===__ 연산자를 사용한다.

- 코틀린에서 자료형을 확인하는 연산자는 __is__ 이다. 

- 코틀린에서 형변환 연산자는 __as__ 이다. 

  - __is__ 를 통해 형변환이 가능한걸 확인하면, __스마트 캐스트__ 기능을 통해 캐스팅 없이 해당 자료형으로 사용할 수 있다.

### 2.5 흐름제어

+ 코틀린의 __if - else__ 문은 값을 반환할 수 있다.

  ```kotlin
  val age: Int = 25
  val ageRange: String = if (age >= 10 && age < 20) {
    "10대"
  } else if (...) {
    "20"
  } else {
    "기타"
  }
  
  //또한 자바의 3항 연산자를 코틀린의 if-else로 대체할 수 있다.
  val number: Int = 20
  val str: String = if(number%2 == 0) "Even" else "Odd"
  ```

+ 코틀린의 __when__ 문은 자바의 __switch__ 문을 대체한다. 자바는 __break;__ 를 사용하여 각 케이스를 구분하지만, 코틀린에서는 __중괄호__ 를 통해 구분한다. 

  ```kotlin
  val bags: Int = 1
  when(bags) {
    //각 케이스에 해당하는 값만 적는다.
    0 -> Log.d(TAG, ...)
    //여러개의 case는 쉼표로 구분
   	1,2 -> {
      ...
    }
    //default는 else 로 구분한다.
    else {
      ...
    }
  }
  ```

  + when문도 if-else문과 마찬가지로 값을 반환할 수 있다. 

  + 각 조건을 상수만 지정할 수 있었던 자바와 달리, 코틀린에서는 각 조건을 __표현식__ 으로 나타낼 수도 있다.

    ```kotlin
    val e: Exception = ... //	값 e에 여러 종류의 예외에 대입될 때
    when(e) {
      is IOExecption -> Log.d(...)
      is IllegalStateException -> Log.d(...)
    }
    
    val str: String = ... //값 str에 임의의 문자열이 대입될 때
    when (str) {
      str.startsWith('a') -> Log.d(...)
      str.startsWith('k') -> Log.d(...)
    }
    ```

+ 코틀린은 __for - each__ 형태만 지원하며, 반복자를 통해 접근하는 인자의 타입을 생략할 수 있다. 

  ```kotlin
  val names: List<String> = ...
  for( name in names ) { //리스트 names를 통해 name은 String으로 추론한다.
    Log.d("Name" , "name = " + name)
  }
  ```

  + for문 내에서 현재 항목의 인덱스는 __Collections.indices__ 프로퍼티를 사용하여 컬렉션의 항목에 접근한다.

    ```kotlin
    val names: List<String> = ..
    for( i in names.indicies) {
      Log.e("Name" , name=${names[i]})
    }
    ```

+ __범위(Range)__ 는 코틀린에만 있는 독특한 자료구조로, 특정 범위를 순환하거나 해당 범위 내에 특정 항목이 포함되어 있는지를 확인할 때 사용한다. __..__ 연산자를 사용한다.

  ```kotlin
  val myRange: IntRange = 0..10
  for(i in myRange) {
    ...
  }
  for( i in 0..10){
    ...
  }
  //연산자 대신 until 함수를 사용하면 가장 마지막 값을 포함하지 않는 범위를 생성
  val myRange: IntRagne = 0..3
  val MyRnage2: IntRange = 0 until 4
  ```

  + 범위 내 특정 항목이 있는지 알아보려면 __in__ 연산자를 사용한다.

    ```kotlin
    val myRange: IntRange = 0..10
    val foo: Boolean = 4 in myRange
    val foo: Boolean = 5 !in myRange
    ```

  + 항목들의 순서가 반대로 정렬된 범위를 생성하려면 __downTo()__ 함수를 사용한다. 기본적으로 1씩 감소시키며, __step()__ 함수를 사용하면 감소 폭을 변경할 수 있다. 

    ```kotlin
    for (i in 5 downTo 1 step 2){
      println(i) //531 출력
    }
    ```

### 2.6 제네릭

제네릭 혹은 제네릭 타입은 인자로 사용하는 타입에 따라 구체화되는 클래스나 인터페이스를 의미.

코틀린에서 제네릭 클래스는 __반드시 타입을 명시__ 해야 한다.

```kotlin 
//car class
open class Car {...}
//sedan class
open class Sedan: Car() {...}
//truck class
open class Truck: Car() {...}

//src로 받은 목록을 dest에 추가한다.
//자바의 ? super T -> in T , ? extends T -> out T
fun <T> append (dest: MutableList<in T>, src: List<out T>) {
  dest.addAll(src)
}
```



### 2.7 예외

- 코틀린에서의 예외는 자바와 거의 동일하다. __throw__ 키워드를 통해 예외를 발생시키며, 객체 생성시와 마찬가지로 new 키워드는 사용하지 않는다. 

- 코틀린의 __try-catch__ 문은 값을 반환할 수 있다.

  ```kotlin
  val valid: Boolean = try {
    //예외를 발생시킬 수 있는 코드들...
    
    //예외가 발생하지 않았다면 true 반환
    true
  } catch ( e: Exception) {
    //예외가 발생했을 때
    ...
    //false 반환
    false
  } finally {
    ...
  }
  ```

- 코틀린은 __Checked Exception__ 을 따로 검사하지 않는다. 즉 대부분의 예외를 try-catch 문으로 감싸 처리했던 자바와는 달리, 코틀린에서는 이를 선택적으로 할 수 있다.

  ```java
  public String readFromJson (String fileName) throws IOExecption {
  	...
  }
  public void process() {
    String json = null;
    try {
      json = readFromJson("foo.json")
    } catch( IOException e){
      ...
    }
  }
  ```

  위의 코드가

  ```kotlin
  fun readFromJson(fileName: String): String {
    ... //IOException may occur
  }
  fun process() {
    //don't have to use try-catch
    val json: String = readFromJson("foo.json")
  }
  ```

  

  