# 14. Comparable을 구현할지 고려하라

Comparable interface의 compareTo 메서드는 단순 동치성 비교에 더해 순서까지 비교 가능하며, 제네릭하다.  
Comparable을 구현했다는 것은 그 클래스의 인스턴스들에게는 자연적인 순서(natural order)가 있음을 뜻한다.

알파벳, 숫자, 연대 같이 순서가 명확한 값 클래스를 작성한다면 반드시 Comparable interface를 구현하자.

다음은 compareTo 메서드의 일반 규약이다.

> 이 객체와 주어진 객체의 순서를 비교한다.  
> 이 객체가 주어진 객체보다 작으면 음의 정수를, 같으면 0을, 크면 양의 정수를 반환한다.  
> 이 객체와 비교할 수 없는 타입의 객체가 주어지면 ClassCastException을 throw 한다.  
> 다음 설명에서 sgn(표현식) 표기는 수학에서 말하는 부호 함수(signum function)를 뜻하며, 표현식의 값이 음수, 0, 양수일 때 -1, 0, 1을 반환하도록 정의했다.
>
> - 모든 x, y에 대해 `sgn(x.compareTo(y)) == -sgn(y.compareTo(x))`여야 한다. (따라서 x.compareTo(y)는 y.compareTo(x)가 예외를 던질 때에 한해 예외를 던져야 한다)
> - 추이성을 보장해야 한다. 즉, `(x.compareTo(y) > 0 && y.compareTo(z))이면 x.compareTo(z) > 0`이다.
> - 모든 z에 대해 `x.compareTo(y) == 0이면 sgn(x.compareTo(z)) == sgn(y.compareTo(z))`이다.
> - `(x.compareTo(y) == 0) == (x.equals(y))`여야 한다. (필수 X, 권고 사항이나 꼭 지키는 게 좋음)

마지막 조건을 위반하면 정렬된 컬렉션에 객체를 넣으면 해당 컬렉션이 구현한 interface(Collection, Set, Map 등)에 정의된 동작과 엇박자를 낸다.  
위 interface들은 동치 비교 시 equals 메서드의 규약을 따른다고 되어 있지만 정렬된 컬렉션들은 equals 대신 compareTo를 사용한다.

Comparable을 구현하지 않은 필드나 표준이 아닌 순서로 비교해야 한다면 Comparator를 사용할 수 있으나 성능상 손실이 존재한다.
