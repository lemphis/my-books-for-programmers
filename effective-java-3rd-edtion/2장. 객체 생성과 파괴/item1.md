# 1. 생성자 대신 정적 팩터리 메서드를 고려하라

class는 client에게 public constructor 대신 static factory method를 제공할 수 있다.  
이 방식에는 장점과 단점 모두 존재한다.

## 장점

1. 이름을 가질 수 있다
    - constructor의 parameter는 반환할 객체의 특성을 제대로 설명할 수 없다.
    - static factory method는 이름만 잘 지어주면 반환될 객체의 특성을 쉽게 묘사할 수 있다.

2. 호출될 때마다 새 인스턴스를 생성하지 않아도 된다
    - 인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있다.
    - ex) `Boolean.valueOf(boolean)`
    - `flyweight pattern`이 이와 비슷한 기법이다.
    - 반복되는 요청에 같은 객체를 반환하는 식으로 static factory method 방식의 클래스는 언제 어느 인스턴스를 살아 있게 할지를 철저히 통제할 수 있다.
        - 인스턴스를 통제하면 `singleton` 또는 `non-instantiable` 로 만들 수 있으며, `동일성`을 보장한다. (a == b)

3. 반환 타입의 하위 타입 객체를 생성할 수 있다.
    - 반환할 객체의 타입을 자유롭게 선택할 수 있는 엄청난 유연성을 가질 수 있다.
    - 구현 클래스를 공개하지 않고 객체를 반환할 수 있어 API를 작게 유지할 수 있다.

4. 입력 매개변수에 따라 다른 클래스의 객체를 반환할 수 있다.
    - `EnumSet` 클래스의 경우 생성자 없이 static factory method만 제공하는데, 원소의 수에 따라 두 가지 하위 클래스 중 하나를 반환한다.
      ```java
      public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
        Enum<?>[] universe = getUniverse(elementType);
        if (universe == null)
            throw new ClassCastException(elementType + " not an enum");
       
        if (universe.length <= 64)
            return new RegularEnumSet<>(elementType, universe);
        else
            return new JumboEnumSet<>(elementType, universe);
      }
      ```

5. static factory method를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다
    - ?