The Reactive pattern is based on a observer pattern and the data passed in based on streams in which some modification can be done.

to create a simple observable
```java
Observable.just("cat","dog")

```

proper factory for observable is as:

```kotlin
 Observable.create{
   emiiter ->
       emitter.onNext("Apha")
       emitter.onNext("beta")
       emitter.onComplete();
 }


```

so these observable can be chained like

```kotlin
 Observable
	 .just("cat","dog")
	 .map(String::length)
     .subscribe(observer)

```

Implementing Observer interface for subscribe. or just use lambdas.

``` java
public interface Observer<T> {
	void onSubscribe@NonNull Disposable d);
	void onNext(@NonNull T value);
	void onError(Throwable e);
	void onComplete();
}

```


We have  many types of observable as defined in [[Observable types (function)]]

or defined in types  as [[Observable types (form) (Single ...)]]

or based on flow as in [[Observable types  (Flow)  (Cold vs hot observables)]]