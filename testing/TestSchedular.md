
# TestSchedular 
 Use this to manipulate time for test.
```java
TestScheduler testScheduler = new TestScheduler();

TestObserver<Long> testObserver = new TestObserver<>();

Observable<Long> minuteTicker =Observable.interval(1, TimeUnit.MINUTES,testScheduler);

minuteTicker.subscribe(testObserver);
```

``` java

//Fast forward by 30 seconds
testScheduler.advanceTimeBy(30, TimeUnit.SECONDS);
testObserver.assertValueCount(0);

//Fast forward to 70 seconds after subscription
testScheduler.advanceTimeTo(70, TimeUnit.SECONDS);
testObserver.assertValueCount(1);

//Fast Forward to 90 minutes after subscription
testScheduler.advanceTimeTo(90, TimeUnit.MINUTES);
testObserver.assertValueCount(90);

```

In summary, use TestScheduler when you need to represent time elapsing virtually, but
note that this is not a thread-safe Scheduler and should not be used with actual
concurrency. A common pitfall is that a complicated flow that uses many operators and
schedulers is not easily configurable for using TestScheduler . In this case, you can use
RxJavaPlugins.setComputationScheduler() and similar methods that override the
standard Scheduler and inject TestScheduler in its place.