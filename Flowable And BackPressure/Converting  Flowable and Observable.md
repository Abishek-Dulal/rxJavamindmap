
## Converting Observable and Flowable.
``` java
Observable<Integer> source = Observable.range(1, 1000);
source.toFlowable(BackpressureStrategy.BUFFER)
		 .observeOn(Schedulers.io())
		.subscribe(System.out::println);
sleep(10000);
```

``` java
Flowable<Integer> integers =
Flowable.range(1, 1000)
		.subscribeOn(Schedulers.computation());

Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
		  .flatMap(s -> integers.map(i -> i + "-" + s)
		  .toObservable())
		  .subscribe(System.out::println);

sleep(5000);
```

If Observable  had many more than five emissions (for example, 1,000 or
10,000 emissions), then it would probably be better to turn it into a Flowable instead of
turning the flat-mapped Flowable into an Observable .

Even if you call toObservable() , the Flowable still leverages backpressure, but at the
point it becomes an Observable , the downstream will no longer be backpressured and will
request a Long.MAX_VALUE number of emissions. This may be fine as long as no more
intensive operations or concurrency changes happen downstream and the Flowable
operations upstream constrain the number of emissions.

