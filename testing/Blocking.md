
# Blocking Subscriber
To test the code for  we can use a blocking subscriber.
``` java
AtomicInteger hitCount = new AtomicInteger();
Observable<Long> source = Observable
						 .interval(1, TimeUnit.SECONDS)
						 .take(5);

source.blockingSubscribe(i -> hitCount.incrementAndGet());
assertTrue(hitCount.get() == 5);
```

## Blocking Operators
<div style="color:red">Use with extreame caution</div>

## BlockingFirst
Blocking gets the first value  and returns immediatly

Looking at blockingFirst() , you may be tempted to use it in
production code to save a result statefully and refer to it later. Try not to
do that! While there are certain cases where you might be able to justify it
(such as saving emissions into a HashMap for expensive computations and
lookups), blocking operators can easily be abused. If you need to persist
values, try to use replay() and other reactive caching strategies so that
you can easily change their behavior and concurrency policies down the
road. Blocking often makes your code less flexible and undermines the
benefits of Rx.

## BlockingGet
Equivalent code for  Single and Maybe

## BlockingLast
 avoid with infinite sources
## Blocking Iterable
One of the most interesting blocking operators is blockingIterable() . Rather than
returning a single emission like our previous examples, it provides the emissions as they
become available through iterable . The Iterator provided by
the Iterable keeps blocking the iterating thread until the next emission is available,
and the iteration ends when onComplete() is called

```java

Observable<String> source =Observable.just("Alpha", "Beta", "Gamma","Delta", 
                                            "Zeta");

Iterable<String> allLengthFive = source.filter(s -> s.length() == 5)
                               .blockingIterable();

for (String s: allLengthFive) {
    assertTrue(s.length() == 5);
}
```

Unlike C#, note that Java's for-each construct does not handle cancellation, breaking, or
disposal. You can work around this by iterating the Iterator from the iterable inside try-
finally . In the finally block, cast the Iterator to a disposable so that you can call its
dispose() method.

## BlockingForEach
``` java
Observable<String> source =Observable.just("Alpha", "Beta", "Gamma", "Delta", 
                                 "Zeta");
source
     .filter(s -> s.length() == 5)
	 .blockingForEach(s -> assertTrue(s.length() == 5));
```

## BlockingNext
blockingNext() will return an iterable and block each iterator's next() request until the
next value is provided. Emissions that occur after the last fulfilled next() request and
before the current next() are ignored. In the following code example, we have a source
that emits every microsecond (1/1,000 th of a millisecond). Note that the iterable returned
from blockingNext() ignores previous values that it missed:
``` java
Observable<Long> source = Observable.interval(1, TimeUnit.MICROSECONDS)
                                    .take(1000);

Iterable<Long> iterable = source.blockingNext();

for (Long i: iterable) {
	System.out.println(i);
}

```

## BlockingMostRecent
blockingMostRecent() is similar to blockingLatest() , but it reconsumes the latest
value repeatedly for every next() call from the iterator, even if already consumed. It also
requires a defaultValue argument so that it has something to return if no value is emitted
yet. In the following example, we use blockingMostRecent() against an Observable
emitting every 10 milliseconds, and the default value is -1 :
```java
Observable<Long> source =Observable.interval(10, TimeUnit.MILLISECONDS)
                            .take(5);
Iterable<Long> iterable = source.blockingMostRecent(-1L);
for (Long i: iterable) {
     System.out.println(i);
}
```
