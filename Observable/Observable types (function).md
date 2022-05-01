# Observable types

## Observable.range

```kotlin
    Observable.range(3,10)
    .subscribe{println(it)}
```

Count start from 3  , 10 times increment to 13

## Observable.interval
``` java
     Observable<Long> seconds = Observable.interval(1, TimeUnit.SECONDS);
```
goes till infinity  it is a cold stream . if a hot observable is needed use publish.
```java
     Observable<Long> seconds = Observable.interval(1, TimeUnit.SECONDS).publish();

```

## Observable.future
 create observer form future objects
```java
  Observable.fromFuture(future);
```

## Observable.empty
create a observable that just completes.

## Observable.never
creates a observable that never completes.

# Observable.error
Creates a observable that sends an error message

## Observable.defer
to take into account for state changes
 for stateful observable behaviour 
```java
Observable<Integer> source = Observable.defer(() ->Observable.range(start, count));
source.subscribe(i -> System.out.println("Observer 1: " + i));
//modify count
count = 5;
source.subscribe(i -> System.out.println("Observer 2: " + i));
```

```

Observer 1: 1
Observer 1: 2
Observer 1: 3
// count is changed to 5 so the data emmission is also changed.
Observer 2: 1
Observer 2: 2 
Observer 2: 3
Observer 2: 4
Observer 2: 5

```


## Observable.fromCallable
for lazy emmission.
if the error occurs inside  Observable.just it is not passed down to stream but handled like a normal java program.

```java
Observable.fromCallable(() -> 1 / 0)
          .subscribe(i -> System.out.println("Received: " + i),
           e -> System.out.println("Error captured: " + e));
```
