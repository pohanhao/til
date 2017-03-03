# Abstract Factory pattern

```java
public interface OfficeFactory {

    Word createWord();

    // omitted
    // Excel createExcel();
}
```

```java
class WinWord implements Word {

    WinWord() {
        System.out.println("this is windows word.");
    }
}
```

```java
class MacWord implements Word {

    MacWord() {
        System.out.println("this is mac word.");
    }
}
```

```java
public class WinFactory implements OfficeFactory {

    @Override
    public Word createWord() {
        return new WinWord();
    }
}
```

```java
public class MacFactory implements OfficeFactory {

    @Override
    public Word createWord() {
        return new MacWord();
    }
}
```

```java
public class AbstractFactoryDemo {

    public static void main(String[] args) {
        OfficeFactory winFactory = new WinFactory();
        winFactory.createWord();

        OfficeFactory macFactory = new MacFactory();
        macFactory.createWord();
    }
}
```

**Reference**

1. https://openhome.cc/Gossip/DesignPattern/AbstractFactory.htm
