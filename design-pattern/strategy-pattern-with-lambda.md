#Strategy Pattern with Lambda

```java
@FunctionalInterface
public interface OperationStrategy<T> {
	
    T compute(T x, T y);
    
}
```
```java
public enum Operation implements OperationStrategy<Double> {
    ADD((x, y) -> x + y),
    SUBTRACT((x, y) -> x - y),
    MULTIPLY((x, y) -> x * y),
    DIVIDE((x, y) -> x / y),
    MAX(Double::max);
    
    private OperationStrategy<Double> operationStrategy;

    Operation(final OperationStrategy<Double> operationStrategy) {
        this.operationStrategy = operationStrategy;
    }

    @Override
    public Double compute(Double x, Double y) {
        return operationStrategy.compute(x, y);
    }
}
```
**Reference**

1. https://alextheedom.wordpress.com/strategy-pattern-implemented-as-an-enum-using-lambdas/
