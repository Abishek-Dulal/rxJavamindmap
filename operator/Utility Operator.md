
<h2 style="color:green;">  Utilty Operator </h2>
## a) delay
- delaySubscription (dont know how this work)
## b) repeat
``` java
Observable.just("Alpha", "Beta", "Gamma")
.repeat(2)
.subscribe(s -> System.out.println("Received: " + s));
```
with no number infinite repeat
```
Received:Alpha
Received: Beta
Received: Gamma
Received: Alpha
Received: Beta
Received: Gamma
```
## c) single 
  - singleOrError
  - singleElement -> maybe
## d) timestamp
  addes timed wrapper data 
## e) timeInterval
   added timelapses between emission.