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
   - 이러한 유연함은 서비스 제공자 프레임워크를 만드는 근간이 된다.

## 단점

1. 상속을 하려면 public이나 protected 생성자가 필요하니 static factory method만 제공하면 하위 클래스를 만들 수 없다.

   - 컬렉션 프레임워크의 유틸리티 구현 클래스들은 상속할 수 없다.
   - 상속보다는 컴포지션을 사용하도록 유도하고 불변 타입으로 만들려면 이 제약을 지켜야 한다는 점에서 오히려 장점으로 받아들여질 수도 있다.

2. static factory method는 프로그래머가 찾기 어렵다.
   - 생성자처럼 API 설명에 명확히 드러나지 않으니 사용자는 static factory method 방식 클래스를 인스턴스화할 방법을 알아내야 한다.
   - static factory method에 흔히 사용하는 명명 방식을 따라 이름을 지어서 문제를 완화해야 한다.
     - `from`: 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드
     - `of`: 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드
     - `valueOf`: from과 of의 더 자세한 버전
     - `instance` 혹은 `getInstance`: (매개변수를 받는다면) 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다.
     - `create` 혹은 `newInstance`: instance 혹은 getInstance와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장한다.
     - `getType`: getInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 factory method를 정의할 때 쓴다. `Type`은 factory method가 반환할 객체의
       타입이다.
     - `newType`: newInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 factory method를 정의할 때 쓴다. `Type`은 factory method가 반환할 객체의
       타입이다.
     - `type`: getType과 newType의 간결한 버전
