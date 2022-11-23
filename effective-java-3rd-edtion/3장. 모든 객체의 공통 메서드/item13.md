# 13. clone 재정의는 주의해서 진행하라

`Cloneable` interface는 복제해도 되는 클래스임을 명시하는 용도의 mixin interface지만, 의도한 목적을 제대로 이루지 못했다.  
clone 메서드가 선언된 곳이 Cloneable이 아닌 Object이고, 심지어 protected이다. 그래서 Cloneable을 구현하는 것만으로는 외부 객체에서 clone 메서드를 호출할 수 없다.  
메서드 하나 없는 Cloneable은 Object의 clone 메서드의 동작 방식을 결정한다.

clone 메서드는 사실상 생성자와 같은 효과를 낸다.  
clone 메서드는 원본 객체에 아무런 해를 끼치지 않는 동시에 복제된 객체의 불변식을 보장해야 한다.

Cloneable을 이미 구현한 클래스를 확장한다면 어쩔 수 없이 clone을 잘 작동하도록 구현해야 한다.  
그렇지 않은 상황에서는 복사 생성자와 복사 팩터리가 더 나은 객체 복사 방식을 제공할 수 있다.