# Multicasting ,Replaying and cacheble
## a) connect
   time  when subscriber should connect is immportant.
## b) autoconnect 
 works like a  count down latch .
  autoconnect(0) for immediate firing
## c) refcount and share
disposes the observable when there is no subscriber and the stream starts over.
```kotlin
 Observable.just(1,2,3).share()
 Observable.just(1,2,3).publish().refcount();

```
## d) replay
use it in conjuction  with autoconnect.
donot use with refcount.
```java
replay(1)
replay(1,TimeUnit.Seconds)
```

## e) cache
cache all emissions indefinitely for the long term and do not need to control the subscription behavior to the source
cacheWithInitialCapacity(9)
**favor** replay  for finer control. only use cache if absolutely certain.


