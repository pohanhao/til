# Abstract Factory pattern

create an interface or abstract class for factory
```java
public interface OfficeFactory {

    Word createWord();

    // omitted
    // Excel createExcel();
}
```

create concrete class
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

create concrete factory 
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

demo: display content by platform factory
```java
public class AbstractFactoryDemo {

    public static void main(String[] args) {
        OfficeFactory winFactory = new WinFactory();
        Word winWord = winFactory.createWord();

        OfficeFactory macFactory = new MacFactory();
        Word macWord = macFactory.createWord();
    }
}
```

**Reference**

1. https://openhome.cc/Gossip/DesignPattern/AbstractFactory.htm
