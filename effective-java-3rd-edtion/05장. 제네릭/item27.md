# 27. 비검사 경고를 제거하라

모든 비검사 경고는 런타임에 ClassCastException을 일으킬 수 있는 잠재적 가능성을 뜻하니 최선을 다해 제거하자.

경고를 제거할 수는 없지만 타입 안전하다고 확신할 수 있다면 @SuppressWarnings("unchecked") 애너테이션을 달아 경고를 숨기자.  
@SuppressWarnings("unchecked") 애너테이션은 항상 가능한 한 좁은 범위에 적용해야 하며, 경고를 무시해도 안전한 이유를 항상 주석으로 남겨야 한다.
