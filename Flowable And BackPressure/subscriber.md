
## Working with Flowable and subscriber.
All Observable factories work with flowable but with Flowable.interval we have.

```java
Flowable.interval(1, TimeUnit.MILLISECONDS)
		.observeOn(Schedulers.io())
		.map(i -> intenseCalculation(i))
		.subscribe(System.out::println,Throwable::printStackTrace);

sleep(Long.MAX_VALUE);
```

```
0
io.reactivex.exceptions.MissingBackpressureException:
Cant deliver value 128 due to lack of requests
at io.reactivex.internal.operators.flowable.FlowableInterval
```

we can use  OnBackPressure### operator to control this.

## Interface Subscriber

Instead of an Observer , Flowable uses a Subscriber to consume emissions and events at
the end of a Flowable chain. If you pass only lambda event arguments (and not an entire
Subscriber object), subscribe() does not return a Disposable , but rather
an org.reactivestreams.Subscription , which can be disposed of by calling
cancel() instead of dispose() . Subscription can also serve another purpose; it
communicates upstream how many items are wanted using its request() method.
Subscription can also be leveraged in the onSubscribe() method of Subscriber ,
which can call the request() method (and pass in the number of the requested elements)
the moment it is ready to receive emissions.

```java
Flowable.range(1, 1000)
		.doOnNext(s -> System.out.println("Source pushed " + s))
		.observeOn(Schedulers.io())
		.map(i -> intenseCalculation(i))
		.subscribe(s ->
					System.out.println("Subscriber received " + s),
					Throwable::printStackTrace,
					() -> System.out.println("Done!"));
```
 Default request for emmision is unbounded.

The quickest and easiest way to implement a Subscriber is to have the onSubscribe()
method call request(Long.MAX_VALUE) on Subscription , which essentially tells the
upstream give me everything now. Even though the operators preceding Subscriber will
request emissions at their own backpressured pace, no backpressure will exist between the last operator and the Subscriber . This is usually fine since the upstream operators
constrain the flow anyway.

```java
Flowable.range(1, 1000)
.doOnNext(s -> System.out.println("Source pushed " + s))
.observeOn(Schedulers.io())
.map(i -> intenseCalculation(i))
.subscribe(new Subscriber<Integer>() {
				@Override
				public void onSubscribe(Subscription subscription) {
				//can also be let say 40
					subscription.request(Long.MAX_VALUE);
				}
				
				@Override
				public void onNext(Integer s) {
					sleep(50);
					System.out.println("Subscriber received " + s);
				}
				@Override
				public void onError(Throwable e) {
					e.printStackTrace();
				}
				@Override
				public void onComplete() {
					System.out.println("Done!");
				}
});
```

