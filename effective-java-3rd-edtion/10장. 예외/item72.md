# 72. 표준 예외를 사용하라

예외는 재사용하는 것이 좋으며, 자바 라이브러리는 대부분 API에서 쓰기에 충분한 수의 예외를 제공한다.

표준 예외를 재사용하면 얻는 게 많다.

- 만든 API를 다른 사람이 익히고 사용하기 쉬워짐
- 예외 클래스 수가 적을수록 메모리 사용량도 줄고 클래스를 적재하는 시간도 적게 걸림

아래는 널리 재사용되는 예외들이다.

| 예외                            | 주요 쓰임                                                                                                                                                   |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IllegalArgumentException        | 허용하지 않는 값이 인수로 건네졌을 때 (null은 따로 NullPointerException으로 처리) <br /> ex) 반복 횟수를 지정하는 메서드에 음수를 건넬 때                   |
| IllegalStateException           | 객체가 메서드를 수행하기에 적절하지 않은 상태일 때 (인수 값이 무엇이었든 어차피 실패할 상황일 때) <br /> ex) 제대로 초기화되지 않은 객체를 사용하려고 할 때 |
| NullPointerException            | null을 허용하지 않는 메서드에 null을 건넸을 때                                                                                                              |
| IndexOutOfBoundsException       | 인덱스가 범위를 넘어섰을 때                                                                                                                                 |
| ConcurrentModificationException | 허용하지 않는 동시 수정이 발견됐을 때                                                                                                                       |
| UnsupportedOperationException   | 호출한 메서드를 지원하지 않을 때<br /> ex) 원소를 넣을 수만 있는 List 구현체에 대고 누군가 remove 메서드를 호출할 때                                        |

Exception, RuntimeException, Throwable, Error는 직접 재사용하지 말자. (추상 클래스라고 생각하면 좋다)  
위 예외들은 다른 예외들의 상위 클래스이므로, 즉 여러 성격의 예외들을 포함하는 클래스이므로 안정적으로 테스트할 수 없다.
