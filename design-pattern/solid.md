# S.O.L.I.D Design principle

### Single Responsibility Principle

> "One class should have one and only one responsibility"

[example](#srp-example)

### Open Closed Principle

> "Software components should be open for extension, but closed for modification"

### Liskov’s Substitution Principle

> "Derived types must be completely substitutable for their base types"

### Interface Segregation Principle

> "Clients should not be forced to implement unnecessary methods which they will not use"

[example](#isp-example)

### Dependency Inversion Principle

> "Depend on abstractions, not on concretions"

[example](#dip-example)

---

#### SRP Example

```java
public class AlarmApplication {

    public void trigger() {
        sendEmail("from", toAddresses);
    }

    private void sendEmail(String from, List<String> toAddresses) {
        // some logic
    }
}
```

refactor to this

```java
public class AlarmApplication {
    private EmailService emailService = new EmailService();

    public void trigger() {
        sendMessage("from", toAddresses);
    }

    private void sendMessage(String from, List<String> toAddresses) {
        emailService.send(from, toAddresses);
    }
}
```

```java
public class EmailService {
    
    public void send(String from, List<String> toAddresses) {
        // some logic
    }
}
```

#### ISP Example
 
```java
public interface Behavior {

    void fly();

    void run();

    void swim();
}
```

```java
public abstract class Animal implements Behavior {
  // some logic
}
```

```java
public class Bird extends Animal {
    @Override
    public void fly() {
        System.out.println("I can fly.");
    }
    @Override
    public void run() {
        throw new IllegalAccessError("I can't run");
    }
    @Override
    public void swim() {
        throw new IllegalAccessError("I can't swim");
    }
}
```

refactor to this

```java
public abstract class Animal {
  // some logic
}
```

```java
public interface FlyBehavior {

    void fly();
}
```

```java
public class Bird extends Animal implements FlyBehavior {
    @Override
    public void fly() {
        System.out.println("I can fly.");
    }
}
```

#### DIP Example

```java
public interface MessageService {
    void send(String from, List<String> toAddresses);
}
```

```java
public class EmailService implements MessageService {
    
    public void send(String from, List<String> toAddresses) {
        // some logic
    }
}
```

```java
public class InstantMessageService implements MessageService {
    
    public void send(String from, List<String> toAddresses) {
        // some logic
    }
}
```

```java
public class AlarmApplication {
    
    private MessageService messageService;
    
    public AlarmApplication(MessageService messageService) {
        this.messageService = messageService;
    }

    public void trigger() {
        messageService.send("from", toAddresses);
    }
}
```

**Reference**

1. https://dzone.com/articles/the-solid-principles-in-real-life
2. http://howtodoinjava.com/best-practices/5-class-design-principles-solid-in-java/
