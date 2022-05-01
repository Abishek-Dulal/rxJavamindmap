<h1 style="color:green;">    Conditional Operator </h1>
## a) takeWhile
## b) skipwhile
## c) takeUntil
## d) skipUntil

## e) defaultIfEmpty
```java
 Objservable.just("a","b")
    .filter(i-> i.lenth>2)
    .defaultIfEmpty("apple")
    .subscribe(::pritln)

```

## f) SwitchIfEmpty
```java
 Objservable.just("a","b")
    .filter(i-> i.lenth>2)
    .switchIfEmpty(Observable.just("Zeta", "Eta", "Theta"))
    .subscribe(::pritln)

```