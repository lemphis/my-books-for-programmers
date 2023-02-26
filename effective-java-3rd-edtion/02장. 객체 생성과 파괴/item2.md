# 2. 생성자에 매개변수가 많다면 빌더를 고려하라

static factory method와 생성자에는 공통된 제약이 하나 있다. 선택적 매개변수가 많을 때 적절히 대응하기 어렵다는 것이다.  
점층적 생성자 패턴(telescoping constructor pattern) 또는 자바빈즈 패턴(JavaBeans pattern) 등을 사용할 수 있지만, 점층적 생성자 패턴의 안정성과 자바빈즈 패턴의 가독성을
겸비한 `빌더 패턴(Builder pattern)`이 있다.

빌더 패턴은 파이썬과 코틀린 등의 언어에 있는 명명된 선택적 매개변수(named optional parameters)를 흉내 낸 것이다.

빌더 패턴은 객체를 immutable하게 생성할 수 있으며 생성자에 동일한 타입의 parameter가 있을 때 발생할 수 있는 실수를 원천적으로 방지한다.
빌더 패턴은 계층적으로 설계된 클래스와 함께 사용하기에 좋다.

```java
public abstract class Pizza {
	public enum Topping {HAM, MUSHROOM, ONION, PEPPER, SAUSAGE}

	final Set<Topping> toppings;

	// 추상 클래스는 추상 Builder를 가진다. 서브 클래스에서 이를 구체 Builder로 구현한다.
	abstract static class Builder<T extends Builder<T>> {
		EnumSet<Topping> toppings = EnumSet.noneOf(Topping.class);

		public T addTopping(Topping topping) {
			toppings.add(Objects.requireNonNull(topping));
			return self();
		}

		abstract Pizza build();

		// 하위 클래스는 이 메서드를 overriding하여 this를 반환하도록 해야 한다.
		protected abstract T self();
	}

	Pizza(Builder<?> builder) {
		toppings = builder.toppings.clone();
	}
}

public class NyPizza extends Pizza {
	public enum Size {SMALL, MEDIUM, LARGE}

	private final Size size;

	public static class Builder extends Pizza.Builder<Builder> {
		private final Size size;

		public Builder(Size size) {
			this.size = Objects.requireNonNull(size);
		}

		@Override
		public NyPizza build() {
			return new NyPizza(this);
		}

		@Override
		protected Builder self() {
			return this;
		}
	}

	private NyPizza(Builder builder) {
		super(builder);
		size = builder.size;
	}
}

public class Calzone extends Pizza {
	private final boolean sauceInside;

	public static class Builder extends Pizza.Builder<Builder> {
		private boolean sauceInside = false;

		public Builder sauceInside() {
			sauceInside = true;
			return this;
		}

		@Override
		public Calzone build() {
			return new Calzone(this);
		}

		@Override
		protected Builder self() {
			return this;
		}
	}

	private Calzone(Builder builder) {
		super(builder);
		sauceInside = builder.sauceInside;
	}
}
```

빌더 패턴에 장점만 있는 것은 아니다. 객체를 생성하려면, 그에 앞서 빌더부터 만들어야 한다. 또한 점층적 생성자 패턴보다는 코드가 장황하여 매개변수가 4개 이상은 되어야 값어치를 한다.  
**하지만 API는 시간이 지날수록 매개변수가 많아지는 경향이 있음을 명심하자.**