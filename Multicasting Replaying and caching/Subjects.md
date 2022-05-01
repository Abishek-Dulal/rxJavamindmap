
# Subject
Only use subject when all others obtion are exhausted.
Subject acts both as subscriber and obsrvable. It is used to consolidate data from multiple source and stream it to other
```java
Subject<String> subject = PublishSubject.create();
subject.map(String::length)
.subscribe(System.out::println);
subject.onNext("Alpha");
subject.onNext("Beta");
subject.onNext("Gamma");
subject.onComplete();
```
Imperative code .  
collect data from multiple source and  subscribe to one print statement
```kotlin
Observable<String> source1 =
Observable.interval(1, TimeUnit.SECONDS)
.map(l -> (l + 1) + " seconds");
Observable<String> source2 =
Observable.interval(300, TimeUnit.MILLISECONDS)
.map(l -> ((l + 1) * 300) + " milliseconds");
Subject<String> subject = PublishSubject.create();
subject.subscribe(System.out::println);
source1.subscribe(subject);
source2.subscribe(subject);
sleep(3000);
```
Observable to call onComplete() on Subject is going to cease the other Observable objects from pushing their emissions, and downstream cancellation requests are ignored.

**use a Subject for infinite, event-driven (that is, user action-driven) observables**

Subject are prone to error because they can pass arbitary value and predictablity of data is hard to reason about .
Take caution while using them.
(Subject is also hot)

## ToSerialized

the onSubscribe() , onNext() ,onError() , and onComplete() calls are not threadsafe!If you have multiple threads calling these four methods, emissions could start to overlap and break the Observable contract, which demands that emissions happen sequentially.to prevent we can use:-
``` java
Subject<String> subject =PublishSubject.<String>create().toSerialized();
```

# Behaviour Subject
There are a few other flavors of a Subject . One of them is the BehaviorSubject class. It behaves almost the same way as PublishSubject , but it also replays the last emitted item to each new Observer downstream

# ReplaySubject
The ReplaySubject class behaves similar to PublishSubject followed by a cache()
operator. It immediately captures emissions regardless of the presence of a downstream
Observer and optimizes the caching to occur inside the Subject

# AsyncSubject
The AsyncSubject class has a highly tailored, finite-specific behavior: it pushes only the
last value it receives, followed by an onComplete() event.



AsyncSubject resembles CompletableFuture from Java 8 as it performs a computation that you can choose to observe for completion and get the value. You can also imitate AsyncSubject using takeLast(1).replay(1) on an Observable . Try to use this approach
first before resorting to AsyncSubject .

## UnicastSubject
An interesting and possibly helpful kind of Subject is the UnicastSubject class. Like
any Subject , it can be used to observe and subscribe to the sources. In addition, it buffers
all the emissions it receives until an Observer subscribes to it, and then it releases all the
emissions to the Observer and clears its cache

 But there is one important property ofUnicastSubject ; it works with only one Observer and throws an error for any subsequent one.If you want to support more than one Observer and let them receive the live emissions without receiving the missed emissions, you can call publish() to create a single Observer proxy that multicasts to more than one Observer