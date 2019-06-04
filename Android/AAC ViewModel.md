## AAC ViewModel

ViewModel 클래스는 UI 관련 데이터를 고려한 방식으로 설계되었다. 화면 회전과 같은 configuration change에도 ViewModel클래스는 데이터를 보존할 수 있다.

안드로이드 프레임웍은 UI컨트롤러들(액티비티나 프래그먼트)의 생애주기를 관리한다. 프레임웍은 유저의 특정 액션이나 기기의 이벤트에 의해 UI컨트롤러를 destroy 하거나 re-create하곤 한다. 만약 시스템이 UI컨트롤러를 destroy나 re-create하면 일시적인 UI-Data는 손실된다. 예를 들어 유저의 목록을 띄우는 액티비가 있다고 하면, 액티비티가 설정변경(언어,화면회전)으로 인해 re-created될 때 액티비티는 재생성되며 다시 유저의 목록 데이터를 가져와야만 한다. 간단한 데이터라면 onSavedInstanceState()에 저장하고 onCreaet의 번들 파라미터로 넘겨줄 수 있지만 이러한 방식은 serialized 될 수 있는 작은 양의 데이터일때만 가능하다.(유저 데이터나 비트맵들은 불가능)

또 다른 문제는, UI-Controller는 일정 시간이 소요되는 비동기 콜을 자주 요청해야한다는 사실이다. UI-Controller는 이러한 콜을 관리해야하며 destroyed되고 난 이후에 시스템에게 자원을 반환을 해야만 메모리 누수를 피할 수 있다. 이러한 방식은 많은 유지보수가 필요하며, configuration changed가 일어날때 재생성된 객체가 이미 있음에도, 재발행되는 이슈가 생길 수 있다.

UI-Controller는 UI Data를 보여주고, 유저의 행위에 응답하고, 권한확인과 같은 OS와 통신을 하는것이 주요한 작업들이다. 또한 데이터베이스나 네트워크로부터 데이터를 로드하는 작업또한 요구되며, 클래스에 큰 부하가 될 것이다. 작업이 몰린 UI-Controller는 테스트도 쉽지 않다. UI-Controller의 과도한 책임은 결국 단일 클래스 ViewModel을 탄생하게 만들었다.

AAC는 UI-Controller를 위해 ViewModel 클래스를 제공하며, UI를 위해 데이터를 준비하는 책임이 있다. ViewModel클래스는 Configuration Changed가 발생해도 데이터를 자동으로 보존할 수 있으며 다음 액티비티나 프래그먼트에서도 사용할 수 있게 된다.

또한 액티비티가 re-created되더라도 처음에 생성되었던 똑같은 ViewModel과 작용하게 된다. Owner 액티비티가 소멸되면 안드로이드 프레임웤은 ViewModel의 onCleared() 메소드를 호출하여 리소스를 반환하도록 한다.(ViewModel클래스는 절대 view,lifeCycle,context를 참조하고 있는 클래스를 reference하지 않도록 한다. )

ViewModel은 뷰나 LifecyclerOwners의 특정 인스턴스보다 오래 지속되도록 설계되었다. 따라서 view나 lifecyclerOwner와 상관없이 테스트가 더 용이하다는 의미이다. ViewModel은 Livedata같은 LifiecyclerObserver를 포함할 수 있지만 Livedata와 같은 LifeCycle Observable 은 observe할 수 없다. 만약 ViewModel이 Context가 필요하다면 AndroidViewModel을 extends할 수 있으며 생성자로 Application을 받게 된다.





