# Weak, Soft, and Phantom References in Java

## SoftReference
Soft reference objects are cleared at the discretion of the garbage collector in response to memory demand. 
Soft references are most often used to implement memory-sensitive caches. 
All soft references to softly reachable objects are guaranteed to have been cleared before the virtual machine throws an *OutOfMemoryError*

```java
@Test
public void softReference() {
    Object referent = new Object();
    SoftReference<Object> softReference = new SoftReference<Object>(referent);

    assertNotNull(softReference.get());

    referent = null;
    System.gc();
    assertNotNull(softReference.get());
}
```

## WeakReference 
Weak reference objects do not prevent their referents from being made finalizable, finalized, and then reclaimed. 
Weak references are most often used to implement canonicalizing mappings. 
(Here, Canonicalizing mappings means mapping only reachable object instances.)

```java
@Test
public void weakReference() {
    Object referent = new Object();
    WeakReference<Object> weakReference = new WeakReference<Object>(referent);

    assertSame(referent, weakReference.get());

    referent = null;
    System.gc();
    assertNull(weakReference.get());
}
```

## PhantomReference 
Phantom reference objects are enqueued after the collector determines that their referents may otherwise be reclaimed. 
Phantom references are most often used for scheduling pre-mortem cleanup actions in a more flexible way than is possible with the Java finalization mechanism. 
Unlike soft and weak references, phantom references are not automatically cleared by the garbage collector as they are enqueued. 
An object that is reachable via phantom references will remain so until all such references are cleared or themselves become unreachable.

```java
public class LargeObjectFinalizer extends PhantomReference<Object> {

    public LargeObjectFinalizer(
            Object referent, ReferenceQueue<? super Object> q) {
        super(referent, q);
    }

    public void finalizeResources() {
        // free resources
        System.out.println("clearing ...");
    }
}
```

```java
@Test
public void phantomReference() {
    ReferenceQueue<Object> referenceQueue = new ReferenceQueue<>();
    List<LargeObjectFinalizer> references = new ArrayList<>();
    List<Object> largeObjects = new ArrayList<>();

    for (int i = 0; i < 3; ++i) {
        Object largeObject = new Object();
        largeObjects.add(largeObject);
        references.add(new LargeObjectFinalizer(largeObject, referenceQueue));
    }

    largeObjects = null;
    System.gc();

    Reference<?> referenceFromQueue;
    for (PhantomReference<Object> reference : references) {
        System.out.println(reference.isEnqueued());
    }

    while ((referenceFromQueue = referenceQueue.poll()) != null) {
        ((LargeObjectFinalizer)referenceFromQueue).finalizeResources();
        referenceFromQueue.clear();
    }
}
```
and output : 
```
true
true
true
clearing ...
clearing ...
clearing ...
```

---

**So in brief**: 
1. Soft references try to keep the reference. 
2. Weak references don’t try to keep the reference. 
3. Phantom references don’t free the reference until cleared.

**Reference**

1. https://dzone.com/articles/weak-soft-and-phantom-references-in-java-and-why-they-matter
2. https://github.com/uberto/memoryReferences
3. https://www.baeldung.com/java-phantom-reference
