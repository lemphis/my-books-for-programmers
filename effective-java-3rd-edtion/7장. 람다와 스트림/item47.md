# 47. 반환 타입으로는 스트림보다 컬렉션이 낫다

스트림은 반복(iteration)을 지원하지 않는다.  
Stream 인터페이스는 Iterable 인터페이스가 정의한 추상 메서드를 전부 포함할 뿐만 아니라 Iterable 인터페이스가 정의한 방식대로 동작한다.  
그럼에도 foe-each로 스트림을 반복할 수 없는 이유는 Stream이 Iterable을 확장(extend)하지 않았기 때문이다.

API를 스트림만 반환하도록 작성해 놓으면 반환된 스트림을 for-each로 반복하길 원하는 사용자는 불만을 토로할 것이다.  
반대로, API가 Iterable만 반환하면 이를 스트림 파이프라인에서 처리하려는 프로그래머가 성을 낼 것이다.  
자바는 위 두 가지 케이스를 위한 어댑터를 제공하지 않으나, 손쉽게 구현 가능하다.

```java
import java.util.stream.StreamSupport;

public class Demo {
    // Iterable<E> to Stream<E>
    public static <E> Stream<E> streamOf(Iterable<E> iterable) {
        return StreamSupport.stream(iterable.spliterator(), false);
    }

    // Stream<E> to Iterable<E>
    public static <E> Iterable<E> iterableOf(Stream<E> stream) {
        return stream::iterator;
    }
}
```

작성한 메서드의 반환 값이 오직 스프림 파이프라인에서만 쓰일 걸 안다면 마음 놓고 스트림을 반환하자.  
반대로 반환된 객체들이 반복문에서만 쓰일 걸 안다면 Iterable을 반환하자.  
하지만 **공개 API를 작성할 때는 스트림 파이프라인을 사용하는 사람과 반복문에서 쓰려는 사람 모두를 배려해야 한다.**

Collection 인터페이스는 Iterable의 하위 타입이고 stream 메서드도 제공하니 반복과 스트림을 동시에 지원한다.  
따라서 **원소 시퀀스를 반환하는 공개 API의 반환 타입에는 Collection이나 그 하위 타입을 쓰는 게 일반적으로 최선이다.**  
반환하는 시퀀스의 크기가 메모리에 올려도 안전할 만큼 작다면 ArrayList나 HashSet 같은 표준 컬렉션 구현체를 반환하는 게 최선일 수 있다.  
하지만 **단지 컬렉션을 반환한다는 이유로 덩치 큰 시퀀스를 메모리에 올려서는 안 된다.**

아래는 입력 리스트의 모든 연속된 부분 리스트를 스트림으로 반환하는 예시다.

```java
import java.util.Collections;
import java.util.List;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class SubLists {
    public static <E> Stream<List<E>> of(List<E> list) {
        return Stream.concat(Stream.of(Collections.emptyList()), prefixes(list).flatMap(SubLists::suffixes));
    }

    public static <E> Stream<List<E>> prefixes(List<E> list) {
        return IntStream.rangeClosed(1, list.size()).mapToObj(end -> list.subList(0, end));
    }

    public static <E> Stream<List<E>> suffixes(List<E> list) {
        return IntStream.range(0, list.size()).mapToObj(start -> list.subList(start, list.size()));
    }
}
```