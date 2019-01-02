# Command pattern

Encapsulate a request as a object, thereby let you to parameterize other objects with different requests, queue or log request, and support undo operation

```java
public class Light {

    public void on() {
        System.out.println("light on");
    }

    public void off() {
        System.out.println("light off");
    }
}
```

```java
public class AmazonEcho {

    private Light light;

    public AmazonEcho(Light light) {
        this.light = light;
    }

    public void lightOn() {
        light.on();
    }

    public void lightOff() {
        light.off();
    }
}

```

```java
public class RemoteControl {

    private Light light;

    public RemoteControl(Light light) {
        this.light = light;
    }

    public void lightOn() {
        light.on();
    }

    public void lightOff() {
        light.off();
    }
}
```

```java
public static void main(String[] args) {
    Light doorLight = new Light();

    RemoteControl remoteControl = new RemoteControl(doorLight);
    remoteControl.lightOn();

    AmazonEcho amazonEcho = new AmazonEcho(doorLight);
    amazonEcho.lightOff();
}
```

refactor to this

```java
@FunctionalInterface
public interface Command {

    void execute();
}
```

```java
public class RemoteControl {

    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void press() {
        command.execute();
    }
}
```

```java
public class AmazonEcho {

    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void alexa() {
        command.execute();
    }
}
```

```java
public static void main(String[] args) {
    Light doorLight = new Light();

    RemoteControl remoteControl = new RemoteControl();
    remoteControl.setCommand(new LightOnCommand(doorLight));
    remoteControl.press();

    AmazonEcho amazonEcho = new AmazonEcho();
    amazonEcho.setCommand(new LightOffCommand(doorLight));
    amazonEcho.alexa();
}
```

**Reference**

1. https://openhome.cc/Gossip/DesignPattern/CommandPattern.htm
2. http://teddy-chen-tw.blogspot.tw/2013/08/command-pattern.html
3. https://dotblogs.com.tw/pin0513/2010/03/14/14017
4. https://blog.jason.party/4/command-pattern
5. http://alvinalexander.com/java/java-command-design-pattern-in-java-examples
