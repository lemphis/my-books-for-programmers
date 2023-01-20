# 32. 제네릭과 가변인수를 함께 쓸 때는 신중하라

메서드를 선언할 때 실체화 불가 타입으로 varargs 매개변수를 선언하면 컴파일러가 경고를 보낸다.  
가변인수 메서드를 호출할 때에도 varargs 매개변수가 실체화 불가 타입으로 추론되면, 그 호출에 대해서도 경고를 낸다.

매개변수화 타입의 변수가 타입이 다른 객체를 참조하면 heap pollution이 발생한다.

```java
class Demo {
    static void dangerous(List<String>... stringLists) {
        List<Integer> intList = List.of(42);
        Object[] objects = stringLists;
        objects[0] = intList; // heap pollution 발생
        String s = stringLists[0].get(0); // ClassCastException
    }
}
```

위 메서드에서는 형변환하는 곳이 보이지 않는데도 인수를 건네 호출하면 ClassCastException을 던진다.  
마지막 줄에 컴파일러가 생성한 (보이지 않는) 형변환이 숨어 있기 때문이다.  
이처럼 타입 안정성이 깨지니 **제네릭 varargs 배열 매개변수에 값을 저장하는 것은 안전하지 않다.**

그럼에도 제네릭 varargs 매개변수를 받는 메서드를 선언할 수 있게 한 이유는 실무에서 매우 유용하기 때문이다.  
예시로 `Arrays.asList(T... a)`, `Collections.addAll(Collection<? super T> c, T... elements)`, `EnumSet.of(E first, E... rest)`가 대표적이다.

Java 7에서 `@SafeVarargs` 애너테이션이 추가되었다. @SafeVarargs 애너테이션은 메서드 작성자가 그 메서드가 타입 안전함을 보장하는 장치다.  
가변인수 메서드를 호출할 때 varargs 매개변수를 담는 제네릭 배열이 만들어진다는 사실을 기억하자.  
메서드가 이 배열에 아무것도 저장하지 않고(매개변수들을 덮어쓰지 않고) 그 배열의 참조가 밖으로 노출되지 않는다면(신뢰할 수 없는 코드가 배열에 접근할 수 없다면) 타입 안전하다.

varargs 매개변수 배열에 아무것도 저장하지 않고도 타입 안전성을 깰 수도 있으니 주의해야 한다.

```java
import java.util.concurrent.ThreadLocalRandom;

class Demo {
    static <T> T[] pickTwo(T a, T b, T c) {
        switch (ThreadLocalRandom.current().nextInt(3)) {
            case 0:
                return toArray(a, b);
            case 1:
                return toArray(a, c);
            case 2:
                return toArray(b, c);
        }
        throw new AssertionError();
    }

    public static void main(String[] args) {
        String[] attributes = pickTwo("좋은", "빠른", "저렴한");
    }
}
```

아무런 문제가 없는 메서드이니 별다른 경고 없이 컴파일되지만 실행하면 ClassCastException을 던진다.
pickTwo 메서드의 리턴 타입은 항상 Object[]이다. pickTwo에 어떤 타입의 객체를 넘기더라도 담을 수 있는 가장 구체적인 타입이기 때문이다.  
pickTwo 메서드의 반환값을 attributes에 저장하기 위해 String[]으로 형변환하는 코드를 컴파일러가 자동 생성하는데, Object[]는 String[]의 하위 타입이 아니므로 이 형변환은 실패한다.  
