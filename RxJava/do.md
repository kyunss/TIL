## do

Observable 또한 일종의 생명주기를 가지는데, 데이터가 발행될 때, 데이터 발행이 완료됐을때, 데이터 발행이 실패했을때, Observable이 구독이 될 때 등등 각 행위에 대한 콜백을 등록할 수가 있는것이다. 이때 사용하는 do연산자은 언제 쓰이는지 알아보자.

![image-20190615230405051](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190615230405051.png)



- doOnNext() : onNext랑 헷갈릴 수가 있는데 onNext() 함수는 Observer가 가지는 콜백함수이다. Observable에서 발행한 데이터를 구독한 Observer에서 데이터를 받을때 호출되는 함수인것이다. doOnNext() 는 Observable의 콜백함수이며, 데이터가 발행이 될 때 마다 불리는 함수이다. 따라서 데이터가 발행되지 않으면 doOnNext()는 불리지 않는다. 따라서  doOnNext()는 주로 디버깅 용도나, 로깅 용도로 사용된다.

