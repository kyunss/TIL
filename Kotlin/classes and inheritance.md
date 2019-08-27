# Classes and Inheritance

### classes

- 코를린의 클래스는 class로 선언한다.

```kotlin
class Invoice { /*..*/ }
```

- 클래스는 Header(type parameter나 constructor) 나 Body로 이루어져 있고, 둘 다 생략할 수 있다.

```kotlin
class Empty
```



### Constructor

- 코틀린은 주 생성자(primary constructor)를 가질 수 있고, 하나 이상의 보조 생성자(secondary constructor)을 가진다. 주 생성자는 Class의 헤더에 속하고, 클래스 명 다음에 쓴다.(optional parameter와 함께)
- constructor``` 키워드는 생략할 수 있다. 

```kotlin
class Person constructor(name: String) {/**/}
class Person (name: String) {/**/}
```

- 주 생성자에는 어떠한 코드블럭도 들어갈 수 없으며, 초기화에 관한 코드는 `init` 에서 초기화 할 수 있다. 
- 인스턴스 초기화 중에, `init` 블록은 어디에든 끼여 들어갈 수 있으며 인스턴스와 함께 선언된 순서로 실행된다. 

```kotlin
class InitOrderDemo(name: String) {

    val firstProperty = "First Property : $name".also(::println)

    init {
        println("First Initialize Block that print : ${name}")
    }

    val secondProperdy = "Second Property: ${name.length}".also(::print)

    init {
        println("Second Initialize block that print : ${name.length}")
    }
}
/*
First property: hello
First initializer block that prints hello
Second property: 5
Second initializer block that prints 5
*/
```

- 주 생성자로 받은 파라미터는 `init` 블럭에서 쓰일 수 있고, 클래스의 프로퍼티를 초기화 할 때도 쓰일 수 있다.

```kotlin
class Person(name: String) {
    val firstProperty = name.toUpperCase()
}
```

