### Operator

Reactive X 를 지원하는 언어 별 구현체들은 다양한 연산자들을 제공하는데, 이 중에는 공통적으로 제공되는 연산자도 있지만, 반대로 특정 구현체에서만 제공되는 연산자들도 존재한다. 또한 언어별 구현체들은 이미 언어에서 제공하는 메서드 이름과 유사한 형태로 연산자의 네이밍 컨벤션을 유지한다. 

#### 연산자 체인

거의 모든 연산자들은 옵저버블 상에서 동작하고, 옵저버블을 리턴한다. 이러한 접근 방법은 연산자들을 연달아 호출할 수 있는 연산자 체인을 제공한다. 연산자 체인에서 각 연산자는 이전 연산자가 리턴한 옵저버블을 기반으로 동작하며, 동작에 따라 옵저버블을 변경한다.

따라서 옵저버블 연산자 체인은 원본 옵저버블과 독립적으로 실행될 수 없고, 순서대로 실행되어야 한다. 

자주 쓰이는 연산자들 몇 개를 알아보자.

#### Observable 생성

새로운 옵저버블을 만드는 연산자

- Create : 직접적인 코드 구현을 통해 옵저버 메소드를 호출하여 옵저버블을 생성한다.
- From : 다른 객체나 자료 구조를 옵저버블로 변환한다.
- Just : 객체 하나 또는 객체 집합을 옵저버블로 변환한다. 변환된 옵저버블은 원본 객체들을 발행한다.

#### Observable 변환

옵저버블이 배출한 항목들을 변환하는 연산자

- flatMap : 하나의 옵저버블이 발행하는 각 이벤트에 대해 지정된 함수를 적용하여 여러개의 옵저버블로 변환하고, 아이템들의 배출을 차례차례 줄 세워 하나의 옵저버블로 반환하여 전달한다. 

  ![image-20190516145220803](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190516145220803.png)

- map : 옵저버블에서 발행한 각 이벤트에 함수를 적용한 결과를 반환한다.

  ![image-20190516150240285](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190516150240285.png)

#### Observable 필터링

옵저버블에서 선택적으로 이벤트를 배출하는 연산자

- Filter : 테스트 조건을 만족하는 이벤트 항목들만 반환한다.

  ![image-20190516150544109](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190516150544109.png)

- Take : 옵저버블이 배출한 처음 n개의 항목들만 배출한다. Skip 함수는 반대로 처음 n개의 항목들은 건너띈다.

![image-20190516150719171](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190516150719171.png)

#### Observable 결합

- Merge : 복수의 옵저버블들이 배출하는 항목들을 머지시켜 하나의 옵저버블로 만든다. MergeDelayError 함수는 아래 그림처럼 이벤트 스트림을 모두 머지한 뒤에, 옵저버에서 onError를 알린다.

![image-20190516152414998](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190516152414998.png)

- zip : 여러 옵저버블들이 배출한 항목들을 결합하고, 하나의 단일 이벤트 스트림으로 만들어서 실행결과를 배출한다. zip함수는 엄격한 순서가 적용되므로, 새로운 옵저버블은 옵저버블1 + 옵저버블2 = 옵저버블12 이다. 

![image-20190516153923058](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190516153923058.png)

#### Observable 유틸리티 연산자 

- timeOut : source 옵저버블을 그대로 내보내지만, 특정 기간동안이 지나는 동안 발생하는 이벤트가 없으면 onError를 발생시킨다.

![image-20190516154512837](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190516154512837.png)

#### 수학과 집계 연산자

- Concat : 여러 옵저버블의 출력을 연결하여 하나의 옵저버블을 발행한다. 첫번째 옵저버블에서 방출되는 모든 항목은 두 번째 옵저버블에서 방출된 항목보다 먼저 방출된다.

![image-20190516164356249](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190516164356249.png)

- Reduce : 옵저버블의 각 항목에 함수를 적용하고, 함수를 연산한 후 최종 결과만 반환한다. 

![image-20190516164617204](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190516164617204.png)

