# 38. 확장할 수 있는 열거 타입이 필요하면 인터페이스를 사용하라

열거 타입은 거의 모든 상황에서 타입 안전 열거 패턴(typesafe enum pattern)보다 우수하다.  
단, 예외가 하나 있으니, 타입 안전 열거 패턴은 확장할 수 있으나 열거 타입은 그럴 수 없다는 점이다.

사실 대부분의 상황에서 열거 타입을 확장하는 건 좋지 않은 생각이다.

- 확장한 타입의 원소는 기반 타입의 원소로 취급하지만 그 반대는 성립하지 않는다면 논리적으로 이상함
- 기반 타입과 확장된 타입들의 원소 모두를 순회할 방법도 마땅치 않음
- 확장성을 높이려면 고려할 요소가 늘어나 설계와 구현이 더 복잡해짐

하지만 확장할 수 있는 열거 타입이 어울리는 쓰임이 최소한 하나는 있다.  
ex) 연산 코드 (operation code 또는 opcode)  
이럴 때는 인터페이스와 그 인터페이스를 구현하는 기본 열거 타입을 함께 사용해 같은 효과를 낼 수 있다.  
이렇게 하면 클라이언트는 이 인터페이스를 구현해 자신만의 열거 타입(혹은 다른 타입)을 만들 수 있다.

```java
public interface Operation {
    double apply(double x, double y);
}

public enum BasicOperation implements Operation {
    PLUS("+") {
        public double apply(double x, double y) {
            return x + y;
        }
    },
    MINUS("-") {
        public double apply(double x, double y) {
            return x - y;
        }
    },
    TIMES("*") {
        public double apply(double x, double y) {
            return x * y;
        }
    }, DIVIDE("/") {
        public double apply(double x, double y) {
            return x / y;
        }
    };

    private final String symbol;

    BasicOperation(String symbol) {
        this.symbol = symbol;
    }

    @Override
    public String toString() {
        return symbol;
    }
}

public enum ExtendedOperation implements Operation {
    EXP("^") {
        public double apply(double x, double y) {
            return Math.pow(x, y);
        }
    },
    REMAINDER("%") {
        public double apply(double x, double y) {
            return x % y;
        }
    };

    private final String symbol;

    BasicOperation(String symbol) {
        this.symbol = symbol;
    }

    @Override
    public String toString() {
        return symbol;
    }
}

public class Demo {
    public static void main(String[] args) {
        double x = Double.parseDouble(args[0]);
        double y = Double.parseDouble(args[1]);
        test(Arrays.asList(ExtendedOperation.values()), x, y);
    }

    private static void test(Collection<? extends Operation> opSet, double x, double y) {
        for (Operation op : opEnumType.getEnumConstants()) {
            System.out.printf("%f %s %f = %f%n", x, op, y, op.apply(x, y));
        }
    }
}
```