# 45. 스트림은 주의해서 사용하라

스트림 API는 다량의 데이터 처리 작업을(순차적이든 병렬적이든) 돕고자 자바 8에 추가되었다.  
이 API가 제공하는 추상 개념 중 핵심은 두 가지다.

1. 스트림(stream)
   - 데이터 원소의 유한 혹은 무한 시퀀스(sequence)를 뜻함
2. 스트림 파이프라인(stream pipeline)
   - 원소들로 수행하는 연산 단계를 표현하는 개념

스트림 파이프라인은 소스 스트림에서 시작해 종단 연산(terminal operation)으로 끝나며, 그 사이에 하나 이상의 중간 연산(intermediate operation)이 있을 수 있다.  
각 중간 연산은 스트림을 어떠한 방식으로 변환(transform)한다.

스트림 파이프라인은 지연 평가(lazy evaluation)된다.  
평가는 종단 연산이 호출될 때 이뤄지며, 종단 연산에 쓰이지 않는 데이터 원소는 계산에 쓰이지 않는다.

스트림 API는 메서드 연쇄를 지원하는 플루언트 API(fluent API)다.  
파이프라인 하나를 구성하는 모든 호출을 연결하여 단 하나의 표현식으로 완성할 수 있다.

기본적으로 스트림 파이프라인은 순차적으로 수행된다.  
파이프라인을 병렬로 실행하려면 파이프라인을 구성하는 스트림 중 하나에서 parallel 메서드를 호출해주기만 하면 되나, 효과를 볼 수 있는 상황은 많지 않다.

스트림 API는 다재다능하여 사실상 어떠한 계산이라도 해낼 수 있다.  
스트림을 제대로 사용하면 프로그램이 짧고 깔끔해지지만, 잘못 사용하면 읽기 어렵고 유지보수도 힘들어진다.

스트림 파이프라인은 되풀이되는 계산을 함수 객체(주로 람다나 메서드 참조)로 표현한다.
하지만 함수 객체로는 할 수 없지만 코드 블록으로는 할 수 있는 일들이 있다.

- 코드 블록에서는 범위 안의 지역번수를 읽고 수정할 수 있으나 람다에서는 final 또는 effectively final인 변수만 읽을 수 있고, 지역번수를 수정하는 것은 불가능하다.
- 코드 블록에서는 return 문을 사용해 메서드에서 빠져나가거나, break나 continue 문으로 블록 바깥의 반복문을 종료하거나 반복을 한 번 건너뛸 수 있고 메서드 선언에 명시된 checked exception을 던질 수 있다. 하지만 람다로는 이 중 어떤 것도 할 수 없다.

계산 로직에서 위와 같은 일들을 수행해야 한다면 스트림과는 맞지 않는 것이다.  
반대로 다음 일들에는 스트림이 아주 안성맞춤이다.

- 원소들의 시퀀스를 일관되게 변환한다.
- 원소들의 시퀀스를 필터링한다.
- 원소들의 시퀀스를 하나의 연산을 사용해 결합한다(더하기, 연결하기, 최솟값 구하기 등)
- 원소들의 시퀀스를 컬렉션에 모은다(아마도 공통된 속성을 기준으로 묶어가며)
- 원소들의 시퀀스에서 특정 조건을 만족하는 원소를 찾는다.

스트림을 사용해야 멋지게 처리할 수 있는 일들이 있고, 반복 방식이 더 알맞은 일도 있다.  
그리고 수많은 작업들이 이 둘을 조합했을 때 가장 멋지게 해결된다.  
스트림과 반복 중 어느 쪽이 나은지 확신하기 어렵다면 둘 다 해보고 더 나은 쪽을 택하라.
