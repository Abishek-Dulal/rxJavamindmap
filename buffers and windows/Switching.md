
# Switching
it emits the latest Observable derived from the latest emission and disposes of any previous observables that were processing.In other words, it allows you to cancel an emitting Observable andswitch to a new one, thereby preventing stale or redundant processing.

``` java
	Observable<String> items = Observable.just("Alpha", "Beta","Gamma", "Delta", 
                            "Epsilon", "Zeta", "Eta", "Theta", "Iota");

   Observable<String> processStrings =
                              items.concatMap(s -> Observable.just(s)
                             .delay(randomSleepTime(), TimeUnit.MILLISECONDS));

   Observable.interval(5, TimeUnit.SECONDS)
	            //every five secong it switches the observable in process string
             .switchMap(i -> processStrings.doOnDispose(() ->
                           System.out.println("Disposing! Starting next")))
                           .subscribe(System.out::println);

```

For switchMap() to work effectively, the thread pushing emissions into switchMap()
cannot be occupied while doing the work inside switchMap() . This means that you may
have to use observeOn() or subscribeOn() inside switchMap() to do work on a
different thread. If the operations inside switchMap() are expensive to stop (for instance, a
database query using RxJava-JDBC), you might want to use unsubscribeOn() as well to
keep the triggering thread from becoming occupied with the disposal.

A neat trick you can do to cancel work within switchMap() (without providing new work
immediately) is to conditionally yield Observable.empty()