Observable.interval is a concurrent  observable as it does not block the main thread.

we can make other observer to not block in main thread by using  **subscribeOn**

We can achieve this using the subscribeOn() operator, which suggests to the source to
fire emissions on a specified Scheduler (separate thread). In this case, let's use
Schedulers.computation() , which pools a fixed number of threads appropriate for
computation operations. It will provide a thread to push emissions for each Observer .
When onComplete() is called, the thread will be given back to Scheduler so it can be
reused elsewhere

``` kotlin

Observable.just("Alpha", "Beta", "Gamma")
.subscribeOn(Schedulers.computation())
.map(s -> intenseCalculation((s)))
.subscribe(System.out::println);

Observable.range(1, 3)
.subscribeOn(Schedulers.computation())
.map(s -> intenseCalculation((s)))
.subscribe(System.out::println);

sleep(20000);

```

``` txt
 1
 2
 Alpha
 3
 Beta
 Gamma
```


RxJava Operator can work with  observers in different threads and combine  them  safely.

```kotlin
Observable<String> source1 =Observable.just("Alpha", "Beta", "Gamma")
							.subscribeOn(Schedulers.computation())
							.map(s -> intenseCalculation((s)));

Observable<Integer> source2 =Observable.range(1, 3)
							.subscribeOn(Schedulers.computation())
							.map(s -> intenseCalculation((s)));

Observable.zip(source1, source2, (s, i) -> s + "-" + i)
.subscribe(System.out::println);

sleep(20000);


```

<div style ="color:yellow">
Being able to split and combine observables emitting on different threads is a powerful feature that eliminates the pain points of callbacks. When you start making reactive applications concurrent, a subtle complication can creep in. By default, a non-concurrent application will have one thread doing all the work from the source to the final Observer . But having multiple threads can cause emissions to be produced faster than the Observer can consume. For instance, the zip() operator may have one source producing emissions faster than the other. This can overwhelm the program and memory can run out as backlogged emissions are cached by certain operators. When you are working with a high volume of emissions (more than 10,000) and leveraging concurrency,you will likely want to use Flowable instead of Observable
</div>

So instead of using sleep .  we can use **blockingSubscribe** .
keeping an application alive based on the life cycle of a finite Observable subscription is a valid case to use a blocking operator.

actual scheduling is done with[[Schedulers]]


# Understanding susbscribeOn

The subscribeOn() operator suggests to the source Observable which Scheduler to use
and how to execute operations on one of its threads. If that source is not already tied to a
particular Scheduler , it will use the specified Scheduler .

It is important to note that the subscribeOn() operator has no practical effect with certain
sources (and keeps a worker thread unnecessarily on standby until that operation
terminates). This might be because an Observable already uses a Scheduler . For
example, Observable.interval() will use Schedulers.computation() and will
ignore any subscribeOn() .Instead the observable  thread usage type must be specified at the start.

On Multiple subscribeOn the first one used is always has the priority .


# Understanding unSubscribeOn
Disposing of an Observable can be an expensive (in terms of the time it takes) operation, depending on the nature of the source. For instance, if the Observable emits the results of a database query using RxJava-JDBC, ( https://github.com/davidmoten/rxjava-jdbc ), it can be expensive to dispose of because it needs to shut down the JDBC resources it is using.
```java
Disposable d = Observable.interval(1, TimeUnit.SECONDS)
				.doOnDispose(() -> System.out.println("Disposing on thread "+ 
                                 Thread.currentThread().getName()))
				.unsubscribeOn(Schedulers.io())
				.subscribe(i -> System.out.println("Received " + i));

sleep(3000);
d.dispose();
sleep(3000);
```

You can use multiple unsubscribeOn() calls if you want to target
specific parts of the Observable chain to be disposed of with different
schedulers. Everything downstream (after) of an unsubscribeOn() will
be disposed of until another unsubscribeOn() is encountered, which
will own the next upstream segment.


Backlinks:-
1) [[Observe On]]
2) [[Parrallelization]]