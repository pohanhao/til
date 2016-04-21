#Singleton

```java
public class Singleton {
  	private static Singleton instance = new Singleton();
  	private Singleton() {
  	}
  
  	public Singleton getInstance() {
    	return instance;
  	}
}
```

```java
public class Singleton {
  
  	private static class Holder {
    	private static Singleton instance = new Singleton();
  	}
  
  	private Singleton() {
  	}
  
  	public Singleton getInstance() {
    	return Holder.singleton;
  	}
}
```

```java
public enum Singleton {
    INSTANCE;
    private String name;
    public String getName(){
        return name;
    }
    public void setName(String name){
        this.name = name;
    }
}
```

**Reference**

1. http://www.tekbroaden.com/singleton-java.html
2. https://en.wikipedia.org/wiki/Initialization-on-demand_holder_idiom
