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

- 코틀린은 주 생성자(primary constructor)가 있고, 하나 이상의 보조 생성자(secondary constructor)을 가질 수 있다. 주 생성자는 Class의 헤더에 속하고, 클래스 명 다음에 쓴다.(optional한 parameter와 함께)
- **Visibility Modifiers ** 를 위해 private을 붙이는 경우가 아니면 `constructor` 키워드는 생략할 수 있다. (생성자는 기본적으로 public이다.)

```kotlin
class Person constructor(name: String) {/**/}
class Person (name: String) {/**/}
```

- 주 생성자에는 어떠한 코드블럭도 들어갈 수 없으며, 초기화에 관한 코드는 `init` 에서 작성한다.
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

- 비-추상 클래스(그냥 일반 클래스)가 주 생성자와 보조 생성자 둘 다 없다면, 암묵적으로 빈 생성자가 만들어진다. 기본적으로 public으로 만들어지게 되는데, 생성자를 private으로 만들고 싶으면 비어있는 생성자에 private을 붙이면 된다. 

```kotlin
class Person private constructor() { /**/ }
```

- 클래스를 만들 때 생성자의 모든 파라미터가 default value가 있다면, JVM은 default value를 가지고 있으면서 파라미터가 하나도 없는 생성자를 추가로 만든다. 

```kotlin
class Customer(val customerName: String = "")
```



## Inheritance

- 모든 코틀린 클래스는 `Any` 라는 슈퍼 클래스를 선언 없이 디폴트로 상속 받는다. 
- `Any` 는 `equals()`, `hashCode()`, `toString()` 을 가지고 있다. 따라서 모든 클래스는 override할 수 있다.
- 자식 클래스가 만약 주 생성자가 있다면, 자식 클래스의 생성자를 이용하여 부모 클래스를 선언과 동시에 초기화 해야 한다.

```kotlin
open class Base(p: Int)

class Derived(p: Int): Base(p)
```

- 자식 클래스가 만약 주 생성자 없이 보조 생성자(들)만 있다면, `super` 키워드를 사용하여 부모 타입을 같이 초기화 해야 한다. 혹은 같은 역할을 하는 다른 생성자에게 위임을 할 수도 있다.

```kotlin
class CustomView : View {
    constructor(context: Context) : super(context)

    constructor(context: Context, attrs: AttributeSet) : super(context, attrs)
}
```

- 자식 클래스가 생성자가 없어도 부모 클래스를 상속받는 순간 초기화를 같이 해야 한다.

### Overriding a method

- 코틀린은 명시적으로 표현하는 언어이다. 따라서 override가 가능한 함수들은 명시적으로 `open` 으로 명시해야 하고, `override` 또한 마찬가지이다.

```kotlin
open class Shape {
    open fun draw() { /*..*/ }
    fun fill() {/*..*/}
}

class Circle : Shape() {
    override fun draw() {

    }
}
```

- 위 코드에서 `open` 이 붙지 않은 fill() 메소드를 자식 클래스에서 똑같이 만들 수 없다.( `override` 를 붙이든 안붙이든)
- `override` 로 명시된 함수는 해당 클래스를 상속한 하위 클래스에서 오버라이딩이 가능하므로, 이를 막고싶다면 `final` 을 붙인다.

### Overriding Properties

- overriding property 역시 overriding method 와 비슷하게 사용한다.

```kotlin
open class Shape {
    open val vertexCount: Int = 0
}

class Rectangle : Shape() {
    /** 두가지 초기화 방법 모두 가능 **/
    override val vertexCount: Int
        get() = super.vertexCount

    override val vertexCount: Int = 4
}
```

- `val` property 를 `var` 로 override하는것은 가능하지만 그 반대는 허용되지 않는다. 이유는 `val` 프로퍼티는 본질적으로 get() 메소드에 의해 초기화가 되며, `var` 는 추가로 set도 가능하기 때문이다.

```kotlin
interface Shape {
    val vertexCount: Int
}

class Rectangle(override val vertexCount: Int = 4) : Shape //Always has 4 value

class Polygon : Shape {
    override val vertexCount: Int = 5 //can be set to any number later
}
```

### Derived class Initialization order

- 자식 클래스의 객체를 생성하는 동안, 자식 클래스의 초기화 로직이 실행되기 전에 부모 클래스의 초기화가 가장 먼저 진행된다.

```kotlin
open class Base(val name: String) {

    init {
        println("Initializing Base")
    }

    open val size: Int =
            name.length.also { println("Initializing size in Base: $it") }
}

class Derived(
        name: String,
        val lastName: String
) : Base(name.capitalize().also { println("Argument for Base: $it") }) {

    init {
        println("Initializing Derived")
    }

    override val size: Int = (super.size + lastName.length).also { println("Initializing size in Derived: $it") }
}

fun main() {
    val d = Derived("hello", "word")
}

/*
Argument For Base: hello
Initialize Base
Initializing size in Base: 5
Initializing Derived
Initializing size in Derived: 9
 */
```

- 위 코드의 실행결과를 보면, 자식 클래스의 overrided된 프로퍼티는 부모 클래스의 생성자가 실행되기 전까지는 초기화 되지 않음을 알 수 있다.
- (확실친 않은데)재정의가 가능한, 다시 말해서 `open` 이 붙은 프로퍼티는 부모 클래스의 생성자나, `init` 블록이나 프로퍼티 초기화 블록에서는 사용하지 않도록 한다

### Calling the superclass implementation

- (자바와 마찬가지로) 자식 클래스는 `super` 키워드를 사용하여 부모 클래스의 프로퍼티나 함수를 호출할 수 있다.

```kotlin
open class Rectangle {
    open fun draw() { println("Drawing a rectangle") }
    open val borderColor: String get() = "black"
}

class FilledRectagnle : Rectangle() {
    override fun draw() {
        super.draw()
        println("Filled the Rectangle")
    }
    
    val fillColor: String get() = super.borderColor
}
```

### Overriding rules

- 코틀린에서 상속을 구현할 때는 다음의 규칙을 따른다. 
	- 만약 똑같은 멤버를 가진 클래스(인터페이스)들을 여러개를 상속할 경우, 각 상속된 클래스들을 모두 구현해야 한다.
	- 어떤 타입을 상속받았는지 명시하기 위해 꺽쇠괄호 안에 타입을 명시한다.(`super<Base>` )



## Absrtact classes

- 클래스 혹은 클래스의 멤버들은 `abstrac` 키워드를 가질 수 있으며 클래스 안에서 구현체를 가지지 않는다.

- 추상 클래스도 Non-abstract open 클래스를 상속받아서 override할 수 있다.

```kotlin
open class Polygon {
    open fun draw() {}
}

abstract class Rectangle : Polygon() {
    override abstract fun draw()
}
```



## Companion object

- 객체 생성 없이 클래스 내부에 접근할 수 있는 함수를 작성할때(팩토리 메서드 같은) `object` 키워드의 멤버로 선언할 수 있다.
- 더 나아가서, 클래스 이름만으로(객체 생성 없이) 접근하고 싶은 함수를 만들고자 한다면 `companion object`멤버로 사용하여 만들 수 있다.
