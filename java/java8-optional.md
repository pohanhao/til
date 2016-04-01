# java8 - Optioanl

Optional is a new feature in java 8, Optional is a container object which is used to contain not-null objects. Optional object is used to represent null with absent value.

For example, 

```java
Map<String, String> nickNames = new HashMap<>();
nickNames.put("Justin", "caterpillar");
nickNames.put("Monica", "momor");
nickNames.put("Irene", "hamimi");
```
and execute the following code.

```java
String nickName = nickNames.get("Duke");
System.out.println(nickName);
```

You will find out your program be excuted normally and get the output with **null**, but you have no idea what is null mean in your result. That's because there is lot of meaning in null, including **EMPTY**, **NOT AVALIBLE** even **ERROR** in some case.

but in java-8, you can use **Optional** class to handle this problem.

```java
private static Optional<String> getNickName(String name) {
	return Optional.ofNullable(nickNames.get(name));
}
```

Now you can have the benefit of **FAIL-FAST** by using Optioanl class. Not only that, you can eliminate all null-check, you code will be simpler.

```java
Optional<String> nickOptional = getNickName("Duke");
System.out.println(nickOptional.orElse("Openhome Reader"));
```

**Reference**


1. http://openhome.cc/Gossip/Java/Optional.html
2. https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html
3. http://www.tutorialspoint.com/java8/java8_optional_class.htm