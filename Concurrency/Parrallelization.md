
# Parellelization
A common question about RxJava is how to achieve parallelization, or emitting multiple items concurrently from an Observable. Of course, this definition breaks the Observable Contract which states that onNext() must be called sequentially and never concurrently by more than one thread at a time.

``` java 
Observable.range(1, 10)
.flatMap(i -> Observable.just(i)
			  .subscribeOn(Schedulers.computation())
			  .map(i2 -> intenseCalculation(i2)))
.subscribe(i -> System.out.println("Received " + i +" " + LocalTime.now() + " on 
               thread " +Thread.currentThread().getName()));
sleep(20000);

```

<div style ="color:blue" > 
flatMap() allows only one thread out of it at a time to push emissions
downstream, which maintains that the Observable contract demanding
emissions stays sequential. A neat little behavior with flatMap() is that it
does not use excessive synchronization or blocking to accomplish this. If a
thread is already pushing an emission out of flatMap() downstream
toward Observer , any threads also waiting to push emissions will simply
leave their emissions for that occupying thread to take ownership of
</div>


If there are eight cores, we can key each
emission to a number in the range from 0 through 7 . This way, eight instances of
GroupedObservable are created that cleanly divide the emissions into eight streams.
Now, we can cycle through these eight numbers and assign them as a key to each emission.None of the GroupedObservable instances is impacted by subscribeOn() (it will emit on the source's thread with the exception of the cached emissions), so we need to use observeOn() to parallelize them instead


```java 
int coreCount = Runtime.getRuntime().availableProcessors();
AtomicInteger assigner = new AtomicInteger(0);
Observable.range(1, 10)
		  .groupBy(i -> assigner.incrementAndGet() % coreCount)
								.flatMap(grp -> grp.observeOn(Schedulers.io())
								.map(i2 -> intenseCalculation(i2)))
		  .subscribe(i -> System.out.println("Received " + i +" " + LocalTime.now()  
                           + " on thread " +
```

if available use some parallel construct.
