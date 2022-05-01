
# SIngle
``` java

interface SingleObserver<T> {
 void onSubscribe(@NonNull Disposable d);
 void onSuccess(T value);
 void onError(@NonNull Throwable error);
}

```

replaces onComplete with onSucess. has no onNext

can be converted back to Observable as:-
```java
   Single.just("ant").toObservable()

```

can be converted to single like
```java
  Observable.just("ant").first()

```

# Maybe

```java
public interface MaybeObserver<T> {
 void onSubscribe(@NonNull Disposable d);
 void onSuccess(T value);
 void onError@NonNull Throwable e);
 void onComplete();
}
```


can be coverted from Observable with firstElement
```java
Observable<String> source =Observable.just("Alpha", "Beta");
source.firstElement()
	.subscribe(s -> System.out.println("RECEIVED " + s),
	e -> System.out.println("Error captured: " + e),
		() -> System.out.println("Done!")
    );

```
Output is:-
```
RECEIVED Alpha

```

Maybe output can be either onSucess or onComplete both not both.

``` java
Maybe<Integer> source = Maybe.just(100);
source.subscribe(s -> System.out.println("Process 1: " + s),
				e -> System.out.println("Error captured: " + e),
				() -> System.out.println("Process 1 done!"));
//no emission
Maybe<Integer> empty = Maybe.empty();
empty.subscribe(s -> System.out.println("Process 2: " + s),
				e -> System.out.println("Error captured: " + e),
				() -> System.out.println("Process 2 done!"));

```

output for maybe 

```  
process 1: 100
process 2: done!

```

but for Observable just

```
process 1:100
process 1:done

process 2:done

```


# Completable
 for the completion of  event it .with no events being down except completion .
``` java
interface CompletableObserver<T> {
 void onSubscribe@NonNull Disposable d);
 void onComplete();
 void onError(@NonNull Throwable error);
}

```

use either:-
1) Completable.complete()  
2) Completable.fromRunnable()