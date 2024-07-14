# 16. public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라

클래스가 public이면 필드는 반드시 private로 설정하고 접근자 메서드를 사용하자.  
클래스가 package-private이거나 private 중첩 클래스인 경우는 필드를 노출하는 것이 더 나을 수 있다.
