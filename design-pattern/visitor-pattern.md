# Visitor Pattern

```java
public interface Element {

    void accept(Visitor visitor);
}

```

```java
public class ClothesElement implements Element {

    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

```java
public interface Visitor {

    void visit(ClothesElement element);
}

```

```java
public class CheckerboardVisitor implements Visitor {

    @Override
    public void visit(ClothesElement element) {
        System.out.println("啊，好像棋盤似的");
    }
}

```

```java
public class ManuscriptPaperVisitor implements Visitor {

    @Override
    public void visit(ClothesElement element) {
        System.out.println("我看倒有點像稿紙");
    }
}

```

```java
public class MungBeanCakeVisitor implements Visitor {

    @Override
    public void visit(ClothesElement element) {
        System.out.println("真像一塊塊綠豆糕");
    }
}
```

```java
public class VisitorMain {

    public static void main(String[] args) {
        Element element = new ClothesElement();
        element.accept(new CheckerboardVisitor());
        element.accept(new ManuscriptPaperVisitor());
        element.accept(new MungBeanCakeVisitor());
    }
}
```

Output:
```
啊，好像棋盤似的
我看倒有點像稿紙
真像一塊塊綠豆糕
```
