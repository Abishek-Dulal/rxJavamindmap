# Using Compose  and transformers
When working with RxJava, you may find yourself wanting to reuse pieces of an
Observable or Flowable chain and somehow consolidate these operators into a new
operator. Good developers find opportunities to reuse code, and RxJava provides this
ability using ObservableTransformer and FlowableTransformer , which you can pass
to the compose() operator.

## Using Observable Transformer
use this to bunch up a list of operator to single operator
``` java
<T> ObservableTransformer<T, ImmutableList<T>> toImmutableList(){
    return new ObservableTransformer<T, ImmutableList<T>>() {
		@Override
		public ObservableSource<ImmutableList<T>> apply(Observable<T> up){
			return up.collect(ImmutableList::<T>builder,ImmutableList.Builder::add)
					 .map(ImmutableList.Builder::build)
					 .toObservable();
			 }
		};
}
```

can be used as  new operator in 

```java
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
		  .compose(toImmutableList())
		  .subscribe(System.out::println);

Observable.range(1, 10)
		  .compose(toImmutableList())
		  .subscribe(System.out::println);
```


## Using Flowable Transformer
Use Flowable Transformer for flowable data .


# Avoid a Shared state with transformers
first we create a state holder
``` java
static final class IndexedValue<T> {
	final int index;
	final T value;

	IndexedValue(int index, T value) {
		this.index = index;
		this.value = value;
	}

	@Override
	public String toString() {
		return index + " - " + value;
	}
}

```

``` java
static <T> ObservableTransformer<T,IndexedValue<T>> withIndex() {
		final AtomicInteger indexer = new AtomicInteger(-1);
         // this works like a closure and the state is shared
		return upstream -> upstream
		.map(v -> new IndexedValue<T>(indexer.incrementAndGet(), v));
}
```

``` java
Observable<IndexedValue<String>> indexedStrings =
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
		  .compose(withIndex());
//data
indexedStrings.subscribe(v ->System.out.println("Subscriber 1: " + v));
//data
indexedStrings.subscribe(v ->System.out.println("Subscriber 2: " + v));
```

``` 
Subscriber 1: 0  Alpha
Subscriber 1: 1  Beta
Subscriber 1: 2  Gamma
Subscriber 1: 3  Delta
Subscriber 1: 4  Epsilon
Subscriber 2: 5  Alpha
Subscriber 2: 6  Beta
Subscriber 2: 7  Gamma
Subscriber 2: 8  Delta
Subscriber 2: 9  Epsilon
```

The state is  wrong because the subscription is sharing state . we can instead do it like this.

``` java
static <T> ObservableTransformer<T,IndexedValue<T>> withIndex() {
		return upstream -> Observable.defer(() -> {
				AtomicInteger indexer = new AtomicInteger(-1);
		   return upstream.map(v -> new IndexedValue<T> 
                  (indexer.incrementAndGet(),v));
    });
}
```

# to binding
 Use to operater for casting into a custom type.
```java
Binding<String> binding = Observable.interval(1, TimeUnit.SECONDS)
							.map(i -> i.toString())
							.observeOn(JavaFxScheduler.platform())
							.to(JavaFxObserver::toBinding);

label.textProperty().bind(binding)
```




