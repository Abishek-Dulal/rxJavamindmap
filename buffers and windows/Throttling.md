
# Throttle 
```java 
Observable<String> source1 = Observable.interval(100, TimeUnit.MILLISECONDS)
								.map(i -> (i + 1) * 100) //map to elapsed time
								.map(i -> "SOURCE 1: " + i)
								.take(10);
Observable<String> source2 = Observable.interval(300, TimeUnit.MILLISECONDS)
								.map(i -> (i + 1) * 300) //map to elapsed time
								.map(i -> "SOURCE 2: " + i)
								.take(3);
Observable<String> source3 = Observable.interval(2000, TimeUnit.MILLISECONDS)
								.map(i -> (i + 1) * 2000) //map to elapsed time
								.map(i -> "SOURCE 3: " + i)
								.take(2);
//combining observables for rapid firing.
Observable.concat(source1, source2, source3)
		  .subscribe(System.out::println);
```

## throttle last
The throttleLast() operator (another name for the same operator is sample() ) emits
only the last item at a fixed time interval.

```java
Observable.concat(source1, source2, source3)
		  .throttleLast(1, TimeUnit.SECONDS)
		 .subscribe(System.out::println);
```
``` 
// last item in a timeframe
SOURCE 1: 900
SOURCE 2: 900
SOURCE 3: 2000
```

## throttle first
 as as throttlelast but gets the first result as output.

## throttleWithTimeOut or debounce
When emissions are firing rapidly, it does not emit anything until there is a period of silence, and then it pushes the last emission forward.

The throttleWithTimout() operator (also called debounce() ) accepts time-interval
arguments that specify how long a period of inactivity (which means no emissions are
coming from the source) must be before the last emission can be pushed forward

``` java

Observable.concat(source1, source2, source3)
			.throttleWithTimeout(1, TimeUnit.SECONDS)
			.subscribe(System.out::println);
```

``` 
SOURCE 2: 900
SOURCE 3: 2000
SOURCE 3: 4000
```

The 900 emission from source2 was the last emission before source3 started since
source3 does not push its first emission for 2 seconds, which gave more than the needed 1- second period of silence for the 900 to be fired.
