## 회사에서 사용하고 있는 RxJava Extension

- applyOf : Observable의 데이터를 저장하거나, 어딘가에 적용을 하고 Observable 원래 데이터를 그대로 반환할 경우(다른데서 써야 할 경우)

```kotlin
fun <T> Observable<T>.applyOf(mapper: (T) -> Unit): Observable<T> {
  return map {
    //적용할 함수에 Observable<T> 의 데이터를 넣어준다. mapper는 람다 표현식으로 사용할 수 있다.
    mapper(it)
    //처음 들어온 Observable<T>를 그대로 반환한다.
    it 
  }
}

//example
private val isEdited = channel.ofViewAction().ofType<LoginViewAction.OnIdTextChanged>()
	.applyOf{ refinedData.accout.id = it.id }
	.share()
```

여기서 mapper는 Observable의 데이터를 받아서 처리할 함수이고, it은 Observable<T>의 원 데이터가 된다. 풀어서 쓰자면 `fun doSomething(it: Observable<T>) { } `  가 될 것이다. mapper를 람다표현식으로 쓸 수 있는 이유는 함수를 호출할 때 대입하는 인자 중 마지막 인자가 함수 타입이라서 외부에 선언할 수 있고, 함수가 **단 하나의 함수 타입 매개변수를 가질 경우** 인자 대입을 위한 소괄호도 생략이 가능하다. 또한 **함수의 매개변수가 하나일 경우** 파라미터를 생략하고 it으로 쓸 수 있기 때문이다.

