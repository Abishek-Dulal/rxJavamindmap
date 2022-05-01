
# Creating new Operator 
<div style ="color:red; font-weight:bold"> Do this as  a last resort effort</div>

Implementing your own ObservableOperator (as well as FlowableOperator ) is more
involved than creating an ObservableTransformer (or FlowableTransformer ,
correspondingly). Instead of composing a series of existing operators, you intercept the
onNext() , onComplete() , onError() , and onSubscribe() calls from the upstream by
implementing your own Observer instead. This Observer will then logically pass
the onNext() , onComplete() , and onError() events to the downstream Observer in a
way that fulfills the desired operation.

Custom doonEmpty implementation
``` java
private static <T> ObservableOperator<T, T> doOnEmpty(Action action) {
	return new ObservableOperator<T, T>() {
		   
		   @Override
	       public Observer<? super T> apply(Observer<? super T> observer){
					return new DisposableObserver<T>() {
								boolean isEmpty = true;
                                
                                @Override
								public void onNext(T value) {
										isEmpty = false;
										observer.onNext(value);
								}
								
								@Override
								public void onError(Throwable t) {
									observer.onError(t);
								}
								
								@Override
								public void onComplete() {
									if (isEmpty) {
										try {
										action.run();
									} catch (Exception e) {
										onError(e);
										return;
									}
								}
								observer.onComplete();
							}
						};
					}
				};
}
```

so the code which can be used as is:-
``` java
  Observable.range(1,5)
            .lift(doOnEmpty(()->{print()}))
            .subscribe();

```


Custom toList implementation.
``` java

private static <T> ObservableOperator<List<T>, T> myToList() {
	return observer -> new DisposableObserver<T>() {
					ArrayList<T> list = new ArrayList<>();
					
					@Override
					public void onNext(T value) {
						//add to List, but don't pass anything downstream
						list.add(value);
					}
	
					@Override
					public void onError(Throwable t) {
							observer.onError(t);
					}
	
					@Override
					public void onComplete() {
						observer.onNext(list); //push List downstream
						observer.onComplete();
					}
			};
}

```

should create Flowable counterpart to the customOperator as well.
