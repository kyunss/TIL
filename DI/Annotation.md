## Dagger2의 Annotation

- @MapKey

  key타입 어노테이션을 정의할 수 있다. 현재 String과 Enum타입이 맵을 바인딩을 할 수 있다. unwrapValue 값이 false이면 어노테이션 객체 자체가 키 타입이 되고, true이면 인터페이스 안에 정의한 value값이 key가 된다.
