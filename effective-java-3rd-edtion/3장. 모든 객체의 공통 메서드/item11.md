# 11. equals를 재정의하려거든 hashCode도 재정의하라

equals를 재정의한 모든 class에서 hashCode를 재정의해야 한다.

아래는 Object 명세에 정의된 hashCode 일반 규약이다.

> - equals 비교에 사용되는 정보가 변경되지 않았다면 애플리케이션이 실행되는 동안 그 객체의 hashCode 메서드는 몇 번을 호출해도 일관되게 항상 같은 값을 반환해야 한다.
> - equals가 두 객체를 같다고 판단했다면 두 객체의 hashCode는 똑같은 값을 반환해야 한다.
> - equals가 두 객체를 다르다고 판단했더라도 두 객체의 hashCode가 서로 다른 값을 반환할 필요는 없다.
