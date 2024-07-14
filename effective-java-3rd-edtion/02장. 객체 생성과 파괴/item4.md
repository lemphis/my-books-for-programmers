# 4. 인스턴스화를 막으려거든 private 생성자를 사용하라

명시적으로 private 생성자를 추가하면 클래스의 인스턴스화를 막을 수 있다.  
하지만 조금 더 완벽하게 제한하려면 private 생성자 내에 **throw new AssertionError()** 를 추가하여 제어할 수 있다.  
클래스 내부에서 객체를 생성하는 실수를 방지하며 reflection을 통한 객체 생성을 막을 수 있다.

하지만 생성자가 존재하는데 호출할 수 없는 부분이 직관적이지 않으므로 적절한 주석을 달면 좋다.

```java
class UtilityClass {
	// 기본 생성자 방지 (인스턴스화 방지)
	private UtilityClass() {
		throw new AssertionError();
	}
}
```
