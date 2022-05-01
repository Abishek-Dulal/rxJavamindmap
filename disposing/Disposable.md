# Disposable

```java
public interface Disposable {
 void dispose();
 boolean isDisposed();
}

```

Use disposable interface to dispose of data.
```java
	   disposable.dispose()
```

Custom  Observer can be used to dispose values:-

```java
Observer<Integer> myObserver = new Observer<Integer>() {
	private Disposable disposable;
		
	@Override
	public void onSubscribe(Disposable disposable) {
		this.disposable = disposable;
	}

	@Override
	public void onNext(Integer value) {
		//has access to Disposable
	}

	@Override
	public void onError(Throwable e) {
		//has access to Disposable
	 }

	@Override
	public void onComplete() {
		//has access to Disposable
	 }
};
```


# Resource Observer

Automanage dispose data.
```java
	   ResourceObserver<Long> myObserver = new ResourceObserver<Long>(){
                 // implement methods
                           }
// Subscribe with different method
observable.subscribeWith(myObserver);


```

# Composite Disposable
 Use composite disposable to collect disposable data and remove them at once.



# Handling Disposabe in infinite  generation.


```java

  Observable.create(observableEmitter -> {
	try {
		for (int i = 0; i < 1000; i++) {
			while (!observableEmitter.isDisposed()) {
				observableEmitter.onNext(i);
			}
			//this is this should be done to remove resource  leaks.
			if (observableEmitter.isDisposed()) {
				return;
			}
		}
		observableEmitter.onComplete();
		} catch (Throwable e) {
			observableEmitter.onError(e);
		}
	});

```


**Rule 1: Always check isDisposed for infinite Resource**
**Rule 2: Use SetCanceable and SetDisposable to remove disposable **


```java
Observable<T> valuesOf(final ObservableValue<T> fxObservable) {
	return Observable.create(observableEmitter -> {
		observableEmitter.onNext(fxObservable.getValue());
		final ChangeListener<T> listener =
			(observableValue, prev, current) ->observableEmitter.onNext(current);
		fxObservable.addListener(listener);

		//removing observable on event
		observableEmitter.setCancellable(() ->
			fxObservable.removeListener(listener));
		});
}


```
	
