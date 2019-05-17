## Single

RxJava는 옵저버블과 유사한 Single을 제공한다.

Single역시 옵저버블의 한 형태이지만, 옵저버블처럼 존재하지 않는 곳에서부터 무한대까지 임의의 연속된 값들을 배출하는것과는 달리, 항상 한 가지 값 또는 오류 알림 둘 중 하나만 배출한다. 

따라서 Single을 구독할 때는 Observable을 구독할때처럼(onNext, onError, onComplete)대신에, 다음 두 메서드만 사용하게 된다.

- onSuccess : Single은 자신이 배출하는 하나의 값을 이 메서드를 통해 전달한다.
- onError : Single 항목을 배출할 수 없을 때 Throwable객체를 전달한다. 

Single은 이 메서드 중 하나만, 그리고 한 번만 호출된다. 메서드가 호출되면 Single의 생명주기도 끝나고 구독이 종료된다.

아래는 ReactiveX.io에서 가져온 Single의 기본 마블 다이어그램이니 한 번 살펴보자.

![image-20190516173016345](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190516173016345.png)

Single 역시 다양한 연산자를 제공하며, 이 중 어떤 연산자들은 Observable과 Single을 섞어서 사용할 수 있도록 Observable 영역과 Single영역을 연결하는 인터페이스 역할을 수행한다. Concat 과 ConcatWith만 살펴보자.

- Concat, ConcatWith : 여러개의 single이 배출한 항목들을 Observable이 배출하는 형태로 변환한다.

![image-20190516173324934](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190516173324934.png)



