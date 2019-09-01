## Coroutine

### Definition(Wiki)

> Coroutine은 프로그램의 실행이 중지 되었다가 재개됨에 따라서 비선점형 멀티 태스킹을위한 subroutines들을 일반화하는 프로그램의 구성 요소이다. 
>
> Coroutine은 cooperative task, exeption, infinity list 등의 작업을 하는데 적합하다.



### Comparison With Thread(Wiki)

- 코루틴과 쓰레드는 매우 유사하지만, 코루틴은 **Cooperative Multitasked** 이고, 쓰레드는 **Preemptive Multitasked** 방식이다. 
- 다시 말해, 코루틴은 **concurrency** 는 제공하지만, **parallism** 은 아니다. 
	- 동시성이란, 하나의 프로그램에서 동시에 두개 이상의 작업을 할 수 있다면 Concurrency Program이라고 부른다.
	- 동시에 실행되는것처럼 보이지만, **동시가 아닐 수 있다.**
	- OS의 CPU-time-Slicing을 이용하여 각 작업이 작업을 실행하고, 대기상태로 전환되는 이점을 이용한 것으로, 하나의 작업을 수행한 후에, 이 작업이 대기상태가 된다면 OS는 두번째 작업에 할당하여 처리하게끔 한다.
	- 병렬성이란, 문자 그대로 작업의 일부 혹은 여러 작업을 동시에 실행하는 것으로, 물리적으로 하나의 코어가 하나의 작업에 할당이 된다.
	- 따라서 병렬성을 만족하기 위해선 본질적으로 하드웨어의 멀티코어 인프라가 수반되어야 한다.
	- Single Core에서는 Concurrency는 만족할 수 있지만 Parallism을 만족할 수는 없다.
- 동시성과 병렬성의 차이를 살펴보자.
	- 동시성은 두 작업이 겹치는 시간에 시작,실행,완료 될 수 있는 시점이고, 병렬성은 멀티코어 환경에서 동시에 실행된다.
	- 동시성은 독립적인 프로세스들을 실행하는 구성이며, 병렬성은 가능한 관련된 계산의 동시 실행이다.
	- 어플리케이션이 동시성을 만족하지만 병렬성은 아니다의 뜻은 동시에 여러 작업은 가능하지만, 동시에 두개 이상의 작업이 실행되는것은 아님을 뜻한다. 
- 코투틴이 쓰레드에 비해 갖는 장점은 hard-realtime에서 사용할 수 있다는 점(코루틴의 전환 간에는 System Call이나 Blocking Call이 필요치 않다)이고 뮤텍스나 세마포어 같은 동기화 선점에 관한 도구들이 필요하지도 않다. `Critical Section` 을 보호하기 위해 별다른 OS의 지원이 없어도 된다.
- 선점 방식으로 예약된 쓰레드를 통해 코루틴을 구현할 수는 있지만, hard-realtime에 대한 적합성이나 context-Switching에 있어서 장점이 희석되게 된다.
- 쓰레드는 preemptive-scheduled 방식이지만 코틀린은 그렇지 않다. 쓰레드는 언제든지 re-scheduled될 수 있고 concurrently하게 실행 될 수 있으므로 locking에 주의해야 하지만 코루틴이 rescheduled되는 경우는 오직 프로그램 안에서의 특정 포인트 뿐이며, concurrently하게 실행되지 않는다. 따라서 locking을 피할 수 있다.(이러한 장점으로 이벤트 드리븐의 비동기 프로그래밍에 유용하다.)



### Coroutines for asynchronou programming and more

- 서버-사이드 혹은 모바일이나 데스크톱 프로그램을 만들면서, 유저의 관점에서 끊김없는 유동성을 제공함과 동시에 필요에 따라 확장할 수 있어야 한다. 
- 코틀린은 언어레벨에서 **Coroutine Support** 를 지원하고, 대부분의 기능을 코틀린의 철학에 따라 라이브러리에 위임함으로써 유연하게 프로그래밍을 할 수 있다.
- 추가로, Coroutine은 비동기 프로그래밍을 가능하게 할 뿐 아니라, concurrency나 actor같은 다양한 기능도 제공한다.



### Coroutine Basic

- 코틀인은 표준 라이브러리를 통해 최소한의 저수준 API를 제공하며, 다양한 라이브러리를 통해 Coroutine을 유용하게 사용할 수 있다.
- 비슷한 기능을 가진 많은 다른 언어들과는 달리, `async` 나 `await` 는 코틀린의 키워드가 아니며, 표준 라이브러리에도 들어있지 않다.
- 더욱이, 코틀린의 `suspend function` 은 



### Reference

- https://howtodoinjava.com/java/multi-threading/concurrency-vs-parallelism/
- https://en.wikipedia.org/wiki/Coroutine#Comparison_with_threads
