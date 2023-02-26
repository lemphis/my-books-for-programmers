# 10. equals는 일반 규약을 지켜 재정의하라

equals를 재정의하지 않아야 하는 상황

- 각 인스턴스가 본질적으로 고유하다. (동작하는 개체를 표현하는 클래스가 해당됨)
- 인스턴스의 논리적 동치성(logical equality)을 검사할 일이 없다.
- 상위 클래스에서 재정의한 equals가 하위 클래스에도 딱 들어맞는다.
- 클래스가 private 또는 package-private 이고 equals를 호출할 일이 없다.

equals를 재정의할 때 따라야 하는 규약

> equals 메서드는 동치관계(equivalence relation)를 구현하며, 다음을 만족한다.
> - 반사성(reflexivity): null이 아닌 모든 참조 값 x에 대해 x.equals(x)는 true
> - 대칭성(symmetry): null이 아닌 모든 참조 값 x, y에 대해 x.eqlaus(y)가 true면 y.equals(x)도 true
> - 추이성(transitivity): null이 아닌 모든 참조 값 x, y, z에 대해 x.equals(y)가 true이고 y.equals(z)가 true이면 x.equals(z)도 true
> - 일관성(consistency): null이 아닌 모든 참조 값 x, y에 대해 x.equals(y)를 반복해서 호출하면 항상 true이거나 항상 false
> - null 아님: null이 아닌 모든 참조 값 x에 대해 x.equals(null)은 false

equals 메서드 구현 방법

1. == 연산자를 이용하여 입력이 자기 자신의 참조인지 확인한다.
2. instanceof 연산자로 입력이 올바른 타입인지 확인한다.
3. 입력을 올바른 타입으로 형변환한다.
4. 입력 객체와 자기 자신의 대응되는 핵심 필드들이 모두 일치하는지 하나씩 검사한다.