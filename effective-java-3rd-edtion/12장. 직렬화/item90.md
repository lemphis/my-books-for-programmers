# 90. 직렬화된 인스턴스 대신 직렬화 프록시 사용을 검토하라

Serializable을 구현하기로 결정한 순간 언어의 정상 메커니즘인 생성자 이외의 방법으로 인스턴스를 생성할 수 있게 된다.  
버그와 보안 문제가 일어날 가능성이 커진다는 뜻이다.  
하지만 이 위험을 크게 줄여줄 기법이 하나 있다.  
바로 직렬화 프록시 패턴(serialization proxy pattern)이다.

직렬화 프록시 패턴에는 한계가 두 가지 있다.

- 클라이언트가 멋대로 확장할 수 있는 클래스에 적용 불가
- 객체 그래프에 순환이 있는 클래스에 적용 불가

제 3자가 확장할 수 없는 클래스라면 가능한 한 직렬화 프록시 패턴을 사용하자.  
이 패턴이 아마도 중요한 불변식을 안정적으로 직렬화해 주는 가장 쉬운 방법일 것이다.
