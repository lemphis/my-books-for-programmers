# 6. 불필요한 객체 생성을 피하라

**Pattern**은 입력받은 정규표현식에 해당하는 FSM(finite state machine)을 만들기 때문에 인스턴스 생성 비용이 높다.  
성능을 개선하려면 필요한 정규표현식을 표현하는 (불변인) Pattern 인스턴스를 클래스 초기화 과정에서 직접 생성해 캐싱해두고, 나중에 isRomanNumeral 메서드가 호출될 때마다 캐싱해둔 인스턴스를
재사용한다.

- AS-IS

  ```java
  static boolean isRomanNumeral(String s) {
      return s.matches("^(?=.)M*(C[MD]|D?C{0,3})" + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
  }
  ```

- TO-BE

  ```java
  private static final Pattern ROMAN = Pattern.compile("^(?=.)M*(C[MD]|D?C{0,3})" + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");

  static boolean isRomanNumeral(String s) {
      return ROMAN.matcher(s).matches();
  }
  ```

isRomanNumeral 메서드가 처음 호출될 때 필드를 초기화하는 지연 초기화(lazy initialization)로 불필요한 초기화를 없앨 수 있으나 권하지 않는다.  
지연 초기화는 코드를 복잡하게 만드는데, 성능은 크게 개선되지 않을 때가 많다.

boxed primitives를 사용하면 불필요한 객체를 생성하게 되는 경우가 있다. boxed 대신 primitives를 선호하자.

```java
public static long sum() {
    Long sum = 0L; // boxed primitives
    for (long i = 0; i<= Integer.MAX_VALUE; i++)
		sum += i; // jesus...

    return sum;
}
```
