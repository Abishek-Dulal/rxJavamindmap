
<h1 style="color:green;">  Reducing operator </h1>
## a) count
## b) reduce
  seed value should be immutable and we should have seed with value.
  seedwith or collect should be used for mutable data.

<h2 style="color:green;"> 4.1) Boolean Operator </h2>
## a) all
```java 
Observable.just(5, 3, 7, 11, 2, 14)
.all(i -> i < 10)
.subscribe(s -> System.out.println("Received: " + s))
```

```
 Recieved: false
```
## b) any
## c) empty
## d) contains
## e) SequenceEqual
