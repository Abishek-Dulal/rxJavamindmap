
# Windowing
windowing is same as buffering but it collect data in a observable.

## Fixed Size windowing
```java
Observable.range(1, 50)
		  .window(8)
			  .flatMapSingle(obs -> obs.reduce("", (total, next) ->total + 
                                    (total.equals("") ? "" : "|") + next))
			  .subscribe(System.out::println)
```
also supports skip  .

## Time based windowing
```java
Observable.interval(300, TimeUnit.MILLISECONDS)
		  .map(i -> (i + 1) * 300)
		  .window(1, TimeUnit.SECONDS)
          .flatMapSingle(obs -> obs.reduce("", (total, next) ->
                    total + (total.equals("") ? "" : "|") + next))
          .subscribe(System.out::println);
```

## Boundary Based window
```java
Observable<Long> cutOffs = Observable.interval(1, TimeUnit.SECONDS);
Observable.interval(300, TimeUnit.MILLISECONDS)
		  .map(i -> (i + 1) * 300)
          .window(cutOffs)
          .flatMapSingle(obs -> obs.reduce("", (total, next) ->
                         total + (total.equals("") ? "" : "|") + next))
          .subscribe(System.out::println);

sleep(5000);
```
