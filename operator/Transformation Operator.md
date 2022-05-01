
<h1 style="color:green;">   Transformation Operator </h1>
## a) map
## b) cast
## c) startWithItem
```java
Observable<String> menu =
Observable.just("Coffee", "Tea", "Espresso", "Latte");
//print menu
menu.startWithItem("COFFEE SHOP MENU")
.subscribe(System.out::println);

```

 we also have start with array
## d) Sorted
## e) Scan
``` java
Observable.just(5, 3, 7)
.scan((accumulator, i) -> accumulator + i)
.subscribe(s -> System.out.println("Received: " + s));
```
```
Received: 5
Received: 8
Received: 15
```
Rolling sums
