
# Buffering
simple holds the values in a list .
we have three types of buffer in:-
 1) Fixed Size buffering
 2) Time base buffering
 3) Boundary based buffering

## 1) Fixed Size buffering:-
``` java
 buffer(8)
```
we can also use other collections as
```
 buffer(8,HashSet::new)
```
we can also have buffer size with skip.
```
buffer(8,2)
```
if skip is equal to count there is no effect.
if skip is more than count we can have  skip data at that position.
``` java
Observable.range(1, 10)
		.buffer(2, 3)
		.subscribe(System.out::println);
```

```
[1,2]
[4,5]
[7,8]
```

if skip size is less than count we can have rolling buffers.
``` java
Observable.range(1, 10)
			.buffer(3, 1)
			.subscribe(System.out::println);
```

```
[1, 2, 3]
[2, 3, 4]
[3, 4, 5]
[4, 5, 6]
[5, 6, 7]
[6, 7, 8]
[7, 8, 9]
[8, 9, 10]
[9, 10]
```

## 2) Time based Buffering
```java
Observable.interval(300, TimeUnit.MILLISECONDS)
		  .map(i -> (i + 1) * 300)
		  .buffer(1, TimeUnit.SECONDS)
		  .subscribe(System.out::println);
```

we also have skip on buffer.
```
Observable.interval(300, TimeUnit.MILLISECONDS)
.map(i -> (i + 1) * 300)
.buffer(1, TimeUnit.SECONDS, 2)
.subscribe(System.out::println);

```

Either the time is pased or the size is reached.
Time based buffer works on Schedulars computation.

```
[300, 600]
[900]
[1200, 1500]
[1800]
[2100, 2400]
[2700]
[3000, 3300]
```

## 3) Boundary Based Buffering
 Observables are bound by the output of the cutoff observable
```java
 Observable<Long> cutOffs = Observable.interval(1, TimeUnit.SECONDS);
Observable.interval(300, TimeUnit.MILLISECONDS)
		  .map(i -> (i + 1) * 300)
		  .buffer(cutOffs)
		  .subscribe(System.out::println);
```