




## Understanding ObserveOn
SubscribeOn moves up the chain and changes the schedular defined but ObserveOn only works on the downstream .
```java
Observable.just("WHISKEY/27653/TANGO","6555/BRAVO", "232352/5675675/FOXTROT")
		  .subscribeOn(Schedulers.io())
		  .flatMap(s -> Observable.fromArray(s.split("/")))
		  .doOnNext(s -> System.out.println("Split out " + s +" on thread " + 
                       Thread.currentThread().getName()))
		  .observeOn(Schedulers.computation()) //Happens on//computation scheduler
		  .filter(s -> s.matches("[0-9]+"))
		  .map(Integer::valueOf)
		  .reduce((total, next) -> total + next)
		  .doOnSuccess(i -> System.out.println("Calculated sum" + i + " on thread " 
                         + Thread.currentThread().getName()))
		  .observeOn(Schedulers.io()) //Switches back to I/O scheduler
		  .map(i -> i.toString())

```

Let's say you have a chain of Observable operators with two sets of operations, Operation
A and Operation B. Let's not worry what operators each one is using. If you do not have
any observeOn() between them, the operation will pass emissions strictly one at a time
from the source to Operation A, then to Operation B, and finally to the Observer . Even
with subscribeOn() , the source will not pass the next emission down the chain until the
current one is passed all the way to the Observer .

This changes when you introduce an observeOn() , and let's say that we put it between
Operation A and Operation B. After Operation A hands an emission to the observeOn()
operator, it will immediately start the next emission and not wait for the downstream to
finish the current one, including Operation B and the Observer . This means that the sourceand Operation A can produce emissions faster than Operation B and the Observer can consume them. This is a classic producer/consumer scenario where the producer is
producing emissions faster than the consumer can consume them. If this is the case,
unprocessed emissions will be queued in observeOn() until the downstream is able to
process them. But if you have a lot of emissions, you can potentially run into memory
issues.


