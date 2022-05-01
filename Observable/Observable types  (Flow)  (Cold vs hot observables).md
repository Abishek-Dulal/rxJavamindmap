# Cold observables

Every Subscribe recives all the message from the beginning of the stream.
```kotlin
    val observable = Observable.just("Alpha", "Beta", "Gamma")
    observable
        .map { it.length }
        .subscribe { s -> println(s) }
    Thread.sleep(200)
      observable
        .map { it.length }
        .subscribe { s -> println(s) }
```

Observable.interval is a cold stram


# Hot Observables.
Every Subscriber recieve message from the point of subscription.
``` kotlin
	val source = Observable.interval(1, 2, TimeUnit.SECONDS).publish()
    source.subscribe { println("ob 1 is $it") };
    val ob2 = source.subscribe { println("ob 2 is $it") };
    source.connect()
    Thread.sleep(5000)

    source.subscribe { println("ob 3 is $it") };
    ob2.dispose()


    Thread.sleep(5000)
```




