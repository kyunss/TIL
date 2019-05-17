## Subject

Subject는 옵저버나 옵저버블처럼 행동하는 Rx의 구현체에서 사용 가능한 일종의 교각 혹은 프록시라고 볼 수 있는데, 그 이유는 주제는 옵저버이기 때문에 하나 이상의 옵저버블을 구독할 수 있으며 동시에 옵저버블이기도 하므로, 이벤트를 하나 하나 거치면서 재배출하고 관찰하며 새로운 항목들을 배출할 수도 있다. 

하나의 Subject는 하나의 옵저버블을 구독하면서(Cold Observable일 경우) 옵저버블이 항목들을 배출시키도록 동작한다. 그 결과로 Cold Observable을 Hot Observable로 만들기도 한다. 

#### Subject의 종류

4종류의 Subject가 있으며, 각각의 Subject는 특정 상황에 맞도록 설계되었다. 그렇기 때문에 모든 상황에서 아무 Subject를 임의대로 사용할 수 없다.

##### ReplaySubject

ReplaySubject는 옵저버가 구독을 시작한 시점과 관계 없이, 소스 옵저버블이 배출한 모든 항목들을 모든 옵저버에게 배출한다. 몇 개의 생성자 오버로드를 통해, 재생 버퍼의 크기가 할당량보다 커지거나 처음 emit이후 일정 시간이 초과한 경우엔 오래된 항목들을 제거한다. 만약 ReplaySubject를 옵저버로 사용할 경우, 멀티 쓰레드 환경에서 ReplaySubject의 onNext함수가 불리지 않도록 주의해야 한다.(혹은 다른 On 계열 Method) 이 부분은 동시에 콜이 발생하므로, 옵저버블에 관한 계약을 자칫 위반할 수 있어서 ReplaySubject의 결과와 알림 중 어떤걸 Replay할지 애매하기 때문이다.

![image-20190517104620669](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190517104620669.png)



##### AsyncSubject

AsyncSubject는 소스 옵저버블로부터 배출된 마지막 값(만)을 배출하고, 소스 옵저버블의 완료된 이후에야 AsyncSubject가 동작한다.

![image-20190516215336909](/Users/danielkwak/Library/Application Support/typora-user-images/image-20190516215336909.png)

또한 AsyncSubject는 맨 마지막 값을 뒤 이어 오는 옵저버에 전달하는데, 만약 소스 옵저버블이 오류로 인해 종료될 경우 AsyncSubject는 아무 항목도 배출하지 않고 오류를 그대로 전달한다.