# Proxy pattern
In proxy pattern, a class represents functionality of another class. This type of design pattern comes under structural pattern.
In proxy pattern, we create object having original object to interface its functionality to outer world.

```java
public interface IHello {
    public void hello(String name);
}
```


```java
public class HelloSpeaker implements IHello {
    public void hello(String name) {
        System.out.println("Hello, " + name); 
    }
}
```

```java
public class HelloProxy implements IHello { 
    private Logger logger = 
            Logger.getLogger(this.getClass().getName());
    
    private IHello helloObject; 

    public HelloProxy(IHello helloObject) { 
        this.helloObject = helloObject; 
    } 

    public void hello(String name) { 
        log("hello method starts....");      

        helloObject.hello(name);
        
        log("hello method ends...."); 
    } 
    
    private void log(String msg) {
        logger.log(Level.INFO, msg);
    }
}
```

```java
public class ProxyDemo {
    public static void main(String[] args) {
        IHello proxy = 
            new HelloProxy(new HelloSpeaker());
        proxy.hello("Justin");
    }
} 
```


**Reference**
1. https://openhome.cc/Gossip/SpringGossip/FromProxyToAOP.html
2. https://www.tutorialspoint.com/design_pattern/proxy_pattern.htm
