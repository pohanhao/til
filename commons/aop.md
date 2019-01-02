# Aspect Oriented Programming

## Guice Example

```java
@Retention(RetentionPolicy.RUNTIME) @Target(ElementType.METHOD)
@interface NotOnWeekends {} 
```

```java
public class RealBillingService implements BillingService {

  @NotOnWeekends
  public Receipt chargeOrder(PizzaOrder order, CreditCard creditCard) {
    ...
  }
}
```

```java
public class WeekendBlocker implements MethodInterceptor {
  public Object invoke(MethodInvocation invocation) throws Throwable {
    Calendar today = new GregorianCalendar();
    if (today.getDisplayName(DAY_OF_WEEK, LONG, ENGLISH).startsWith("S")) {
      throw new IllegalStateException(
          invocation.getMethod().getName() + " not allowed on weekends!");
    }
    return invocation.proceed();
  }
}
```
```java
public class NotOnWeekendsModule extends AbstractModule {
  protected void configure() {
    bindInterceptor(Matchers.any(), Matchers.annotatedWith(NotOnWeekends.class), 
        new WeekendBlocker());
  }
}
```

```
Exception in thread "main" java.lang.IllegalStateException: chargeOrder not allowed on weekends!
	at com.publicobject.pizza.WeekendBlocker.invoke(WeekendBlocker.java:65)
	at com.google.inject.internal.InterceptorStackCallback.intercept(...)
	at com.publicobject.pizza.RealBillingService$$EnhancerByGuice$$49ed77ce.chargeOrder(<generated>)
	at com.publicobject.pizza.WeekendExample.main(WeekendExample.java:47)
```

### Spring Example
```java
@Component
@Aspect
public class LoggingAspect {
    @Before("execution(* cc.openhome.model.AccountDAO.*(..))")
    public void before(JoinPoint joinPoint) {   
        Object target = joinPoint.getTarget();
        String methodName = joinPoint.getSignature().getName();
        Object[] args = joinPoint.getArgs();
        Logger.getLogger(target.getClass().getName())
              .info(String.format("%s.%s(%s)",
                target.getClass().getName(), methodName, Arrays.toString(args)));
    }
}
```

**Reference**

1. https://github.com/google/guice/wiki/AOP
2. https://openhome.cc/Gossip/SpringGossip/AOPConcept.html
3. https://openhome.cc/Gossip/Spring/AspectJ.html
4. https://openhome.cc/Gossip/Spring/SpringAOP.html
