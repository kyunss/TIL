## View와 ViewGroup

![image-20191127010726805](/Users/daniel/Documents/TIL/TIL/Kotlin/image-20191127010726805.png)

- ViewGroup은 다른 View를 포함할 수 있는 특별한 View이다. (View를 상속하고 있음) 따라서 ConstraintLayout, LinearLayout 같은 레이아웃이나 ViewContainer의 기본 클래스가 된다. 
- ViewGroup은 ViewGroup.LayoutParams 에서 레이아웃의 속성들을 정의할 수 있음.
- View는 UI 위젯들이다. TextView, ImageView… CustomView를 만들기 위해서는 View를 상속받아서 만들어야 한다. 물론 ViewGroup도 상관없음
- ConstraintLayout Extension을 만들려다가 View Extension에 그냥 퉁쳐서 만들려다가 ViewGroup에만 쓸 수 있는 함수들(addView, removeView 같은 자식 뷰에 관한) 때문에 다시 한 번 짚고 넘어가게 됨.