## Using Flowable.generate

Before you use Flowable.generate() , consider making your source Iterable
instead and passing it to Flowable.fromIterable() .
Flowable.fromIterable() respects backpressure and might be easier to use for many
cases. Otherwise, Flowable.generate() is your next best option if you need something
more specific.

The simplest overload for Flowable.generate() accepts just
Consumer with emmitter T and assumes that there is no state maintained between emissions. This can be helpful in creating a backpressure-aware random integer generator,

``` java
Flowable.generate(emitter ->
		emitter.onNext(ThreadLocalRandom.current()
				.nextInt(min, max))
			);
```

Using Flowable.generate() , invoking multiple onNext() operators within
Consumer< Emitter< T >> results in IllegalStateException . The downstream only
needs it to invoke onNext() once, so it can make the repeated calls, as required, to
maintain flow. It also emits onError() in the event that an exception occurs.

we can also maintain state as :-
``` java
Flowable.generate(() -> new AtomicInteger(upperBound + 1),
					(state, emitter) -> {
					int current = state.decrementAndGet();
					emitter.onNext(current);
				if (current == lowerBound) {
						emitter.onComplete();
					}
}

```