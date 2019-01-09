# Fault Tolerance Microservice

## Use Timeout
Prevents blocked threads

```java
URL url = new URL("http://acme-books.com/special-offers");
URLConnection connection= url.openConnection();
connection.connect();
InputStream inputStream = connection.getInputStream();
// Read response from stream
```

```java
URL url = new URL("http://acme-books.com/special-offers");
URLConnection connection= url.openConnection();
connection.setConnectTimeout(100);
connection.setReadTimeout(500);
connection.connect();
InputStream inputStream = connection.getInputStream();
// Read response from stream
```

### Circuit Breakers
Calls to broken services failfast, Offloads broken services

### Bulkheads
Isolates components, Prevents cascading

```java
Semaphore bulkhead = new Semaphore(2);
Offers protectedGetOffers() {
  if (bulkhead.tryAcquire(0, TimeUnit.SECONDS)) {
    try {
      returnspecialOffers.getOffers();
    } finally {
      bulkhead.release();
    }
  } else {
    throw new RejectedByBulkheadException();
  }
}
```

### Thread Pool Handovers
Thread Pool Handovers, Calling threads can always walk away, Generic timeouts

```java
ExecutorService executor = new ThreadPoolExecutor(3, 3, 1, TimeUnit.MINUTES, new SynchronousQueue<>());
Offers protectedGetOffers() {
try {
    Future<Offers> future = executor.submit(specialOffers::getOffers);
    return future.get(1, TimeUnit.SECONDS);
} catch (RejectedExecutionExceptione) {
    throw new RejectedByBulkheadException();
} catch (TimeoutExceptione) {
    throw new ServiceCallTimeoutException();
  }
}
```

### Monitor Service Calls
Timeout rate, Rejected call rate, Short circuit rate, Failure/success rate, Response times

**Reference**

1. https://www.youtube.com/watch?v=pKO33eMwXRs
2. https://www.slideshare.net/KristofferErlandsson/building-fault-tolerant-microservices
3. https://www.youtube.com/watch?v=T9MPDmw6MNI
4. https://www.slideshare.net/ufried/patterns-of-resilience
