## 2. 자바와 비교해보는 코틀린(1)

### 2.1 기본 자료형

- 코틀린은 자바에서 사용하는 원시(primitive) 및 래퍼 타입을 코틀린 자료형으로 처리한다.(int->Int , byte->Byte ...)

- 코틀린은 컴파일 단계를 거치면서 원시 타입과 래퍼 타입 중 더 효율적인 타입으로 변환한다.

- 코틀린 타입 뒤에 붙어있는 느낌표(!)는 해당 타입이 널 허용 여부에 대한 정보를 포함하고 있지 않다는 의미이며, 플랫폼 타입이라고 부른다. (ex: Kotlin.Enum! , kotlin.String! ...)

- 코틀린에서 제공하는 문자열 템플릿을 사용하면, String.format과 달리 템플릿 문자열 내 직접 인자를 대입할 수 있다.

- 인자는 달러($)로 구분하고, 인자로 값이 아닌 표현식을 넣고 싶다면 표현식 부분을 중괄호로 구분하면 된다.

  ```kotlin
  val length: Int = 3000
  //"Length: 3000 meters" 값 할당 
  val lengthText: String = String.format("Length :%d meters",length)
  //Kotlin 방식
  val legnthText: String = "Length: $length meters"
  
  val text: String = "Lorem ipsum"
  //"TextInput: 4"할당
  val lengthText: String = "TextLength : ${text.length}"
  ```

- 배열 타입이 별도로 존재하는 자바와 달리, 코틀린에서는 타입을 인자로 갖는 Array클래스로 표현한다.

  ```kotlin
  val words: Array<String> = arrayOf("Lorem", "ipsum", "dolor", "sit")
  ```

  - arrayOf는 코틀린 표준 라이브러리 함수로, 입력받은 인자로 구성된 배열을 생성한다. 

  - 자바의 원시 타입을 코틀린의 배열 타입 인자로 사용할 수 없으므로, 원시 타입의 배열을 생성할 수 있는 코틀린의 표준 라이브러리를 제공한다.

    ```kotlin
    //Java
    int[] intArr = new Int[1,2,3,4,5]
    //Kotlin (Primitive array)
    val intArr: IntArray = intArrayOf(1,2,3,4,5)
    //Kotlin (Java wrapper type)
    val intArr: ArrayInt<Int> = arrayOf(1,2,3,4,5)
    ```

  - 코틀린으로 작성된 함수는, 가변인자에 배열을 전달하는 경우에만 스프레드 연산자를 사용한다. 

    ```kotlin
    fun foo(arr:Array<Int>) {
      ...
    }
    //가변인자를 받는 함수
    fun bar(vararg args: String){
      ...
    }
    val intArr: Array<Int> = arrayOf(1,2,3,4,5)
    foo(intArr) //배열을 바로 인자로 대입
    
    val stringArr: Array<String> = arrayOf("Lorem" , "Ipsum")
    bar(*stringArr) //스프레드 연산자 사용
    ```



### 2.2 컬렉션

- 코틀린에서는 자바에서 사용하는 컬렉션 클래스를 그대로 사용한다.

- 코틀린에서는 컬렉션 내 자료의 수정 가능 여부에 다라 컬렉션의 종류를 구분한다. 이는 인터페이스를 통해 사용 가능한 함수를 제한하는 방식으로 구현된다.

  - List는 수정이 불가능한 컬렉션이지만, MutableList는 수정이 가능한 컬렉션이다.

- 배열과 마찬가지로, 컬렉션을 쉽게 생성하는 코틀린 표준 라이브러리를 제공한다.

  - listOf(kotlin.collection.List) 와 setOf(kotlinl.collection.Set) 는 자료를 수정할 수 없지만, arrayListOf() , hashSetOf() 는 자료를 수정할 수 있는 컬렉션을 반환한다. 

    ```kotlin
    //자료를 수정할 수 없는 리스트 생성
    val immutableList: List<String> = listOf("Lorem" , "Ipsum")
    //컴파일 에러:자료를 수정을 위한 함수를 지원하지 않음
    mutableList.add("amet")
    
    //자료를 수정할 수 있는 리스트 생성
    val mutableList: MutableList<String> = arrayListOf("Lorem", "Ipsum")
    mutableList.add("test")
    
    val immutableMap: Map<String, Int> = mapOf(Pair("A" , 65) , Pair("B",66))
    val code: Int = immutableMap["A"]
    
    val mutableMap: HashMap<String, Int> = hashMapOf(Pair("A", 65),Pair("B", 66))
    mutableMap["C"] = 67 // 자료 변경 가능
    ```



### 2.3 클래스 및 인터페이스

- 코틀린에서 접근 제한자를 지정하지 않는 경우 __public__ 으로 간주한다.

- 코틀린에서는 클래스와 인터페이스의 메서드나 필드 구현없이 클래스를 선언할 수 있다.

  ```kotlin
  class Foo
  interface Bar
  ```

- 코틀린에서는 클래스의 인스턴스를 생성할 때 new 연산자를 사용하지 않는다. 

  ```kotlin
  val foo: Foo = Foo()
  val bar: Bar = Bar(1)
  ```

- 추상 클래스는 자바와 동일한 방법으로 선언하지만, 추상 클래스의 인스턴스를 생성하는 형태는 매우 다르다.

  ```java
  //Java의 추상클래스 선언 및 인스턴스 생성
  abstract class Foo{
    public abstract void bar();
  }
  Foo foo = new Foo () {
    @Override
    public void bar(){
      //메서드 구현
    }
  };
  ```

  ```kotlin
  //Kotlin의 추상클래스 선언 및 인스턴스 생성
  abstract class Foo{
    abstract fun bar()
  }
  //추상클래스 인스턴스 생성은 object: [생성자]형태로 선언
  val foo = object: Foo(){
    override fun bar(){
      //함수 구현
    }
  }
  ```

- 인터페이스를 선언하거나 인터페이스의 인스턴스를 만드는 방법은 추상 클래스와 매우 유사하다.

  ```kotlin
  interface Bar{
    fun baz()
  }
  
  //object: [인터페이스 이름] 형태로 선언
  val bar = object: Bar{
    override fun baz(){
      //함수구현
    }
  }
  ```

- 자바로 Model클래스를 작성할 경우 Getter,Setter등 보일러 플레이트 코드가 생성된다.

- 코틀린은 이러한 불편함을 개선하기 위해 프로퍼티를 사용한다.

  ```kotlin
  class Person{
    val name: String? = null //값을 읽을 수만 있는 프로퍼티
    var address: String? = null
  }
  ```

- property는 선언 시점에서 **초기화를** 해야 한다. 그렇지 않을 시 컴파일 에러가 발생한다.(단 **생성자**에서 프로퍼티의 값을 할당한다면 선언 시 값을 할당하지 않아도 된다.)

- 프로퍼티 선언 시점이나 생성자 호출 시점에 값을 할당할 수 없는 경우에 **lateinit** 키워드를 사용하여 이 프로퍼티의 값이 나중에 할당될 것임을 명시한다. 

- lateinit 키워드는 **var 프로퍼티에만** 사용할 수 있으며, 선언 시점에서 값 할당을 요구하는 **val 프로퍼티에는 쓸 수 없다.**

  ```kotlin
  class Person{
    val name: String? = null //val 프로퍼티는 항상 선언과 함께 값을 할당해야 한다.
    lateinit val address: String? //컴파일 에러가 발생하지 않는다.
  }
  ```

- 프로퍼티의 초깃값을 할당하는 시점에서 해당 프로퍼티의 타입을 추론할 수 있다면, 타입 선언을 생략할 수 있다. 

- 코틀린에서는 __internal__ 키워드가 새로 추가되었는데, 자바의 __default__ 와 유사하다.(패키지 단위 제한자)

- 자바에서는 같은 패키지에만 있으면 __default__ 에 의해 접근이 가능하지만, 해당 클래스가 포함된 포듈이 아닐지라도 패키지를 동일하게 맞추면 패키지 단위로 제한된 클래스에 접근할 수 있다. 

- 코틀린의 __internal__ 은 이러한 단점을 보완하여 __모듈__ 단위에서만 접근할 수 있다. 모듈의 범위는 

  - IntelliJ IDEA 모듈
  - Maven / Gradle 프로젝트
  - 하나의 Ant 태스크 내에서 함께 컴파일되는 파일들

- 코틀린의 생성자는 __init__ 키워드를 사용한다.

- 생성자에 인자가 필요한 경우 아래의 코드처럼 인자를 받을 수 있다.**(주 생성자)** 여기서 받은 인자는 __init__ 블록 내에서**만** 사용할 수 있다.

  ```kotlin
  class Foo(a: Int) {
    init {
      //생성자에서 수행할 작업
      Log.d("Foo" , "Number: $a")
    }
  }
  ```

- 이렇게 받은 생성자의 인자는 바로 __프로퍼티 할당__ 할 수 있으므로 추가로 프로퍼티를 선언하지 않아도 된다. 

- 다음은 인자로 받은 값을 사용하여 내부의 필드 및 프로퍼티에 할당하는 생성자 예제이다.

  ```java
  public class Foo {
    int a;
    char b;
    Foo ( int a , char b ) {
      this.a = a;
      this.b = b;
    }
  }
  ```

  ```kotlin
  class Foo(val a: Int , val b: Char)
  ```

  - 이처럼 생성자로 받은 데이터를 내부적으로 생성된 프로퍼티에 할당할 수 있게 된다.
  - 만약 __val__ ,__var__ 를 붙이지 않으면 __생성자__ 블럭에서만 쓸 수 있다.

- 주 생성자 이외에 다른 형태의 생성자가 필요한 경우, __constructor__ 키워드를 사용하여 추가 생성자를 선언할 수 있다. 추가 생성자를 선언할 때는 반드시 **주 생성자를 호출해야 한다.**

  ```kotlin
  class Foo(val a: Int , var b: Char) {
    //a의 값만 인자로 받는 추가 생성자
    constructor(a: Int): this(a,0)
    //두 인자의 값을 모두 0으로 저장하는 생성자
    constructor(): this(0,0)
  }
  ```

- 추가 생성자에서는 인자와 프로퍼티를 함께 선언할 수 없다. 프로퍼티 선언이 필요한 인자인 경우 반드시 주 생성자에서 이를 처리해야 한다. 

- 코틀린에서의 함수는 표기법만 다를 뿐 자바와 역할은 동일하다.

  - ```kotlin
    class Foo {
      //아무 값도 반환하지 않는 함수 
      fun foo(): Unit{
      }
      
      //정수 값을 반환하는 함수
      private fun bar() : Int {
      }
    }
    ```

  - 코틀린에서 특별한 값을 반환하지 않는 함수는 __함수 자체__ 를 의미하는 __Unit__타입을 반환하며, __Unit__ 타입을 반환하는 함수는 반환 타입을 생략할 수 있다.

- 코틀린에서는 클래스의 __extends__ 와 인터페이스의 __implements__ 를 구분하지 않고 __:__ 키워드를 사용한다.

  - 클래스의 경우 반드시 **부모 클래스의 생성자를 호출**해야 한다. 
  - 부모 클래스의 생성자가 여러 형태일 경우, 별도의 추가 생성자 선언부에서 부모 생성자를 호출할 수 있다.

- 자바의 @Override는 선택사항이였지만, 코틀린에서는 그런 모호함을 피하기 위해 __override__ 키워드를 무조건 붙여야 한다. 

- 자바의 final 키워드가 붙은 클래스나 메서드는 상속을 받지 못하거나 재정의를 할 수 없지만, 반대로 코틀린에서는 __open__ 키워드가 붙지 않은 클래스나 메서드를 상속받거나 재정의 할 수 없다. 

- __this__ 의 의미는 자바와 코틀린이 동일하게 사용되지만, __this__의 특성상 사용되는 범위가 달라지는 경우가 있다. 코틀린에서는 __this@{클래스이름}__ 으로 사용한다. 

- 자바에서는 __static__ 필드로 된 변수와 메서드는 객체를 생성하지 않고도 쓸 수 있지만 코틀린에서는 __static__ 을 지원하지 않는다. 따라서 __패키지 단위__ 로 선언해서 사용한다.

  ```kotlin
  //Foo.kt
  package foo.bar
  //값 FOO를 패키지 foo.bar에 선언한다.
  const val FOO = 123
  //함수 foo를 패키지 foo.bar에 선언한다.
  fun foo () {
    
  }
  class Foo {
    fun bar() {} //함수 bar은 Foo의 인스턴스를 생성해야 사용할 수 있다.
  }
  ```

  - 패키지 단위로 선언한 값이나 함수는 패키지에 종속되므로, __import__ 문에서도 __{패키지이름}.{값 or 함수}__ 의 형식으로 사용한다. 

  ```kotlin
  import foo.bar.FOO //패키지 값
  import foo.bar.foo //패키지 함수
  
  class Bar {
    fun bar () {
      val foo = FOO //패키지 값 FOO를 참조한다.
    }
    foo() // foo.bar 패키지 함수 foo()를 호출한다. 
  }
  ```

  - __동반객체(companion object)__ 를 사용하면 클래스 내 모든 멤버에 접근할 수 있으면서, 인스턴스 생성 없이 호출할 수 있는 함수를 작성할 수 있다.

    - __동반객체__ 란 클래스별로 하나씩 클래스의 인스턴스 생성 없이 사용할 수 있는 __object__ 를 정의할 수 있는데, 이를 __동반객체__ 라고 한다. 

    - ```kotlin
      //생성자의 접근제한자가 private이므로 외부에서 접근할 수 없다.
      class User private constructor(val name: String, val registerTime: Long) {
        companion object {
          //companion object는 클래스 내부에 존재하므로 private으로 선언된 생성자에 접근할 수 있다.
          fun create(name: String) : User {
            return User(name, System.currentTimeMillis())
          }
        }
      }
      ```

    - 동반 객체로 선언한 함수는 자바의 정적 메서드의 사용방법과 동일하므로, User.create("John")과 같이 쓸 수 있다.

- 코틀린에서는 __object__ 키워드를 사용하여 매우 간편하게 __싱글톤__ 패턴을 사용할 수 있다. 

  ```kotlin
  object Foo {
    val FOO = "foo"
    fun foo() {}
  }
  val fooValue = Foo.FOO
  Foo.foo()
  ```

