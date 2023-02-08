# 44. 표준 함수형 인터페이스를 사용하라

자바가 람다를 지원하면서 API를 작성하는 모범 사례도 크게 바뀌었다.  
예컨대 상위 클래스의 기본 메서드를 재정의해 원하는 동작을 구현하는 템플릿 메서드 패턴의 매력이 크게 줄었다.  
이를 대체하는 현대적인 해법은 같은 효과의 함수 객체를 받는 정적 팩터리나 생성자를 제공하는 것이다.

|       인터페이스       |       함수 시그니처       |          예          |
|:-----------------:|:-------------------:|:-------------------:|
| UnaryOperator<T>  |    T apply(T t)     | String::toLowerCase |
| BinaryOperator<T> | T apply(T t1, T t2) |   BigInteger::add   |
| UnaryOperator<T>  |  boolean test(T t)  | Collection::isEmpty |
|   Predicate<T>    |    R apply(T t)     |   Arrays::asList    |
|    Supplier<T>    |       T get()       |    Instant::now     |
|    Consumer<T>    |  void accept(T t)   | System.out::println |