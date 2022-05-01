
Flowable is the way of working with the subscriber and publisher mismatch problem. it queues the the events  to be processed at a steady pack. Backpressure is the mechnism that the subscriber uses to notify the producer to emit at the pace the subscriber can consume.
```java
Flowable.range(1, 999_999_999)
		.map(MyItem::new)
		.observeOn(Schedulers.io())
		.subscribe(myItem -> {

sleep(50);
System.out.println("Received MyItem " + myItem.id);});
sleep(Long.MAX_VALUE);
```

Note that a Flowable is not subscribed by an Observer but by
an org.reactivestreams.Subscriber : the subscribe() method of a
Flowable accepts Subscriber .

You should know [[when to use Flowable]]

Flowable doesn't use observable and instead uses a [[subscriber]] which can request  how many request it can accept.

Backlinks:-
 1) [[Custom Flowable]]
 2) [[Converting  Flowable and Observable]]
 3) [[BackPressure Strategy]]
 4) [[Flowable  creation using generate]]