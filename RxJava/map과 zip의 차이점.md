## map() 과 zip() 의 차이점

큰 틀에서 보면 map()과 zip모두 RxJava의 결합연산자이다. 하지만 본질적인 차이가 있는데, map()은 source Observable들이 들어오는 순서대로 발행하여 내보내지만, zip()은 source Observables들이 모두 발행이 되어서 데이터들을 합친 후 내보낸다는 점이다. 따라서 merge()는 보통 source Observables들이 같은 행위(이벤트)를 할 때 묶는 용도로 많이 쓴다. zip()은 반드시 source Observables들이 발행된 데이터들을 가지고 있어야 한다는 특징이 있다.

![image-20190607120128424](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190607120128424.png)

공식문서에서 따온 그림을 보면 각 source Observable에서 발행한 각각 데이터를 모아서 하나의 데이터로 합쳐서 발행해준다.

이때 RxJava에서의 zip()함수는 source Observables를 받아서 어떻게 처리할지에 대한 function도 같이 넘겨줘야 하는데 RxKotlin에서는 이를 사용하기 좀 더 쉽게 만들어 놓았다. 먼저 RxJava의 원형을 보자.

![image-20190607132546395](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190607132546395.png)

Source1, source2를 받아서 zipper함수에 넘겨주고 있다. 이때 자바의 BiFunction을 정의할 때 파라미터의 타입까지 다 지정해주어야만 한다. 

 RxKotlin의 zip을 살펴보자.

![image-20190607133843238](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190607133843238.png)

단순히 kotlin으로 컨버팅 이외에 Pair로 묶었음을 볼 수 있다. 따라서 사용할 때 it.first, it.second… 이렇게 참고하는것도 가능하다.

