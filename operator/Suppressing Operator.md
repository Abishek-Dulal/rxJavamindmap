
<h1 style="color:green;">  Suppressing Operator </h1>
## a) filter
## b) take
```java
Observable.just("Alpha", "Beta", "Gamma")
.take(2)
.subscribe(s -> System.out.println("RECEIVED: " + s));
```

```
output
RECEIVED: Alpha
RECEIVED: Beta
```

take can also have a timer.

## c) skip
  skiplast

## d) distinct
use keySelector overloaded method to create a unique value.
## e) distinctUntilChanged
## f) elementAt
SIngleElement -> maybe
SingleElementorError -> SIngle
firstElement 
lastElement
