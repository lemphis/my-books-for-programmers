# 71. 필요 없는 검사 예외 사용은 피하라

꼭 필요한 곳에만 사용한다면 검사 예외는 프로그램의 안정성을 높여주지만, 남용하면 쓰기 고통스러운 API를 낳는다.  
API 호출자가 예외 상황에서 복구할 방법이 없다면 비검사 예외를 던지자.  
복구가 가능하고 호출자가 그 처리를 해주길 바란다면, 우선 옵셔널을 반환해도 될지 고민하자.  
옵셔널만으로는 상황을 처리하기에 충분한 정보를 제공할 수 없을 때만 검사 예외를 던지자.
