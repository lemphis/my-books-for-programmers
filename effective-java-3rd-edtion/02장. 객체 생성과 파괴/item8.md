# 8. finalizer와 cleaner 사용을 피하라

`finalizer`와 `cleaner`는 C++의 `destructor`와 다르다.  
finalizer와 cleaner가 실행되기까지 얼마나 걸릴지 알 수 없다.  
finalizer와 cleaner는 심각한 성능 문제도 동반한다. finalizer가 garbage collector의 효율을 떨어뜨린다.  
finalizer를 사용한 클래스는 finalizer 공격에 노출되어 심각한 보안 문제를 일으킬 수 있다.  
대안은 `AutoCloseable`을 구현하고, 클라이언트에서 인스턴스를 다 사용하고 나면 close 메서드를 호출하면 된다. 이 경우 `try-with-resources` 구문을 사용해 자동으로 close 메서드가 호출되도록 할 수 있다.
