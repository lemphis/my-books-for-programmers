# 33. 타입 안전 이종 컨테이너를 고려하라

제네릭은 Set\<E>, Map\<K, V> 등의 컬렉션과 ThreadLocal\<T>, AtomicReference\<T> 등 단일 원소 컨테이너에도 흔히 쓰인다.  
이런 모든 쓰임에서 매개변수화되는 대상은 (원소가 아닌) 컨테이너 자신이다.  
따라서 하나의 컨테이너에서 매개변수화할 수 있는 타입의 수가 제한된다.

하지만 더 유연한 수단이 필요할 때도 종종 있다.  
컨테이너 대신 키를 매개변수화한 다음, 컨테이너에 값을 넣거나 뺄 때 매개변수화한 키를 함께 제공하면 된다.  
이러한 설계 방식을 `타입 안전 이종 컨테이너 패턴(type safe heterogeneous container pattern)`이라 한다.

> 컴파일타임 타입 정보와 런타임 타입 정보를 알아내기 위해 메서드들이 주고받는 class 리터럴을 `타입 토큰(type token)`이라 한다.

```java
import java.util.HashMap;
import java.util.Objects;

public class Favorites {
    private Map<Class<?>, Object> favorites = new HashMap<>();

    public <T> void putFavorite(Class<T> type, T instance) {
        favorites.put(Objects.requireNonNull(type), instance);
    }

    public <T> T getFavorite(Class<T> type) {
        return type.cast(favorites.get(type));
    }

    public static void main(String[] args) {
        Favorites f = new Favorites();

        f.putFavorite(String.class, "Java");
        f.putFavorite(Integer.class, 0xcafebabe);
        f.putFavorite(Class.class, Favorites.class);

        String favoriteString = f.getFavorite(String.class);
        Integer favoriteInteger = f.getFavorite(Integer.class);
        Class<?> favoriteClass = f.getFavorite(Class.class);

        System.out.printf("%s %x %s%n", favoriteString, favoriteInteger, favoriteClass.getName());
    }
}
```

기대한 대로 이 프로그램은 Java cafebabe Favorites를 출력한다.  
비한정적 와일드카드 타입이라 맵에 아무것도 넣을 수 없다고 생각할 수 있지만, 사실은 그 반대다.  
와일드카드 타입이 중첩(nested)되었다는 점을 깨달아야 한다. 맵이 아니라 키가 와일드카드 타입인 것이다.  
이는 모든 키가 서로 다른 매개변수화 타입일 수 있다는 뜻으로, Class\<String>, Class\<Integer> 등이 될 수 있다.
