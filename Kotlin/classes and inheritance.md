## Classes

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
- **Visibility Modifiers ** 를 위해 private을 붙이는 경우가 아니면 `constructor` 키워드는 생략할 수 있다. (생성자는 기본적으로 public이다.)

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

- 코틀린은 주 생성자로부터 property를 선언하면서 초기화할 수 있는 간결한 구문을 제공한다. 이때 val 과 var 모두 사용할 수 있다.

```kotlin
class Person(val name: String, val age:Int, var address: String) {/*.../*/}
```

- 만약 주 생성자가 Visibility Modifiers를 붙일 경우 constructor 키워드를 앞에 써주면 되고, constructor 키워드는 생략할 수 없다.



### Secondary Constructor

- 클래스는 보조 생성자를 가질 수 있으며, 클래스 body 내에서 `constructor` 키워드를 사용한다. 
- 만약 주 생성자가 있는 상황이라면, 보조 생성자는 선언할 때 `this` 키워드를 통해 주 생성자로 위임해야 한다. 

```kotlin
class Person(name: String) {
    val children = mutableListOf<Person>()
    constructor(name: String, parent: Person): this(name) {
        parent.children.add(this)
    }
}
```

- `init` 블럭은 효과적으로 주 생성자의 일부가 된다. 그러므로 보조 생성자가 주 생성자에게 위임을 하게 될 때 `init` 블럭이 먼저 실행되고, 보조 생성자의 코드가 실행된다. 설령, 주 생성자가 없는 클래스라 할지라도 명시적으로 위임은 동작하고 여전히 `init` 블럭은 실행된다.

```kotlin
class Person {
    constructor(name: String) {
        println("constructor")
    }

    init {
        println("Person Class")
    }
}
/*
Person Class
constructor
 */
```

- 비-추상 클래스가 주 생성자와 보조 생성자 둘 다 없다면, 명시적으로 빈 생성자가 만들어진다. 기본적으로 public으로 만들어지게 되는데, 생성자를 private으로 만들고 싶으면 비어있는 생성자에 private을 붙이면 된다. 

```kotlin
class Person private constructor() { /**/ }
```

- 클래스를 만들 때 생성자의 모든 파라미터가 default value가 있다면, JVM은 default value를 가지고 있으면서 파라미터가 하나도 없는 생성자를 추가로 만든다. 

```kotlin
class Customer(val customerName: String = "")
```

