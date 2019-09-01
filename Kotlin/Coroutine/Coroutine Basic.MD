## Coroutine

### Definition(Wiki)

> Coroutine은 프로그램의 실행이 중지 되었다가 재개됨에 따라서 비선점형 멀티 태스킹을위한 subroutines들을 일반화하는 프로그램의 구성 요소이다. 
>
> Coroutine은 cooperative task, exeption, infinity list 등의 작업을 하는데 적합하다.



### Comparison With Thread(Wik)

- 코루틴과 쓰레드는 매우 유사하지만, 코루틴은 **Cooperative Multitasked** 이고, 쓰레드는 **Preemptive Multitasked** 방식이다. 
- 다시 말해, 코루틴은 **concurrency** 는 제공하지만, **parallism** 은 아니다. 
	- 동시성:
	- 병렬성:



### Coroutines for asynchronou programming and more

- 서버-사이드 혹은 모바일이나 데스크톱 프로그램을 만들면서, 유저의 관점에서 끊김없는 유동성을 제공함과 동시에 필요에 따라 확장할 수 있어야 한다. 
- 코틀린은 언어레벨에서 **Coroutine Support** 를 지원하고, 대부분의 기능을 코틀린의 철학에 따라 라이브러리에 위임함으로써 유연하게 프로그래밍을 할 수 있다.
- 추가로, Coroutine은 비동기 프로그래밍을 가능하게 할 뿐 아니라, concurrency나 actor같은 다양한 기능도 제공한다.



### Coroutine Basic

- 코틀인은 표준 라이브러리를 통해 최소한의 저수준 API를 제공하며, 다양한 라이브러리를 통해 Coroutine을 유용하게 사용할 수 있다.
- 비슷한 기능을 가진 많은 다른 언어들과는 달리, `async` 나 `await` 는 코틀린의 키워드가 아니며, 표준 라이브러리에도 들어있지 않다.
- 더욱이, 코틀린의 `suspend function` 은 
