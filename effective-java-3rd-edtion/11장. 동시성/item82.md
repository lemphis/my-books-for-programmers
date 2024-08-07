# 82. 스레드 안전성 수준을 문서화하라

한 메서드를 여러 스레드가 동시에 호출할 때 그 메서드가 어떻게 동작하느냐는 해당 클래스와 이를 사용하는 클라이언트 사이의 중요한 계약과 같다.

API 문서에 synchronized 한정자가 보이는 메서드는 스레드 안전하다는 이야기를 들었을지 모르겠다.  
하지만 이 말은 몇 가지 면에서 틀렸다.  
자바독이 기본 옵션에서 생성한 API 문서에는 synchronized 한정자가 포함되지 않는다.  
메서드 선언에 synchronized 한정자를 선언할지는 구현 이슈일 뿐 API에 속하지 않는다.

synchronized 유무로 스레드 안전성을 알 수 있다는 주장은 '스레드 안전성은 모 아니면 도'라는 오해에 뿌리를 둔 것이다.  
하지만 스레드 안전성에도 수준이 나뉜다.  
**멀티스레드 환경에서도 API를 안전하게 사용하게 하려면 클래스가 지원하는 스레드 안전성 수준을 정확히 명시해야 한다.**  
다음 목록은 스레드 안전성이 높은 순으로 나열한 것이다.

- 불변(immutable)
  - 이 클래스의 인스턴스는 마치 상수와 같아서 외부 동기화가 필요 없음
  - String, Long, BigInteger 등
- 무조건적 스레드 안전(unconditionally thread-safe)
  - 이 클래스의 인스턴스는 수정될 수 있으나, 내부에서 충실히 동기화하여 별도의 외부 동기화 없이 동시에 사용해도 안전함
  - AtomicLong, ConcurrentHashMap 등
- 조건부 스레드 안전(conditionally thread-safe)
  - 무조건적 스레드 안전과 같으나, 일부 메서드는 동시에 사용하려면 외부 동기화가 필요
  - Collections.synchronized 래퍼 메서드가 반환한 컬렉션 등 (이 컬렉션들이 반환한 반복자는 외부에서 동기화해야 함)
- 스레드 안전하지 않음(not thread-safe)
  - 이 클래스의 인스턴스는 수정될 수 있음
  - 동시에 사용하려면 각각의(혹은 일련의) 메서드 호출을 클라이언트가 선택한 외부 동기화 메커니즘으로 감싸야 함
  - ArrayList, HashMap 등
- 스레드 적대적(thread-hostile)
  - 이 클래스는 모든 메서드 호출을 외부 동기화로 감싸더라도 멀티스레드 환경에서 안전하지 않음
  - 이 수준의 클래스는 일반적으로 정적 데이터를 아무 동기화 없이 수정함
  - 스레드 적대적으로 밝혀진 클래스나 메서드는 일반적으로 문제를 고쳐 재배포하거나 사용 자제(deprecated) API로 지정

조건부 스레드 안전한 클래스는 주의해서 문서화해야 한다.  
어떤 순서로 호출할 때 외부 동기화가 필요한지, 그리고 그 순서로 호출하려면 어떤 락 혹은 (드물게) 락들을 얻어야 하는지 알려줘야 한다.

모든 클래스가 자신의 스레드 안전성 정보를 명확히 문서화해야 한다.  
정확한 언어로 명확히 설명하거나 스레드 안전성 애너테이션을 사용할 수 있다. (@Immutable, @ThreadSafe, @NotThreadSafe)  
무조건적 스레드 안전 클래스에서는 synchronized 메서드가 아닌 비공개 락 객체를 사용하자.  
이렇게 해야 클라이언트나 하위 클래스에서 동기화 메커니즘을 깨뜨리는 걸 예방할 수 있고, 필요하다면 다음에 더 정교한 동시성을 제어 메커니즘으로 재구현할 여지가 생긴다.
