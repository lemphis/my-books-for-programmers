# 30. 이왕이면 제네릭 메서드로 만들라

클라이언트에서 입력 매개변수와 반환값을 명시적으로 형변환해야 하는 메서드보다 제네릭 메서드가 더 안전하며 사용하기 쉽다.  
타입과 마찬가지로, 메서드도 형변환 없이 사용할 수 있는 편이 좋으며, 대부분의 경우 그렇게 하려면 제네릭 메서드가 되어야 한다.  
형변환을 해줘야 하는 기존 메서드는 제네릭하게 만들자.
