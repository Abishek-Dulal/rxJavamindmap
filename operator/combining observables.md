# Merging factories and operators
## a) merge  and mergewith
```java
 Observable.merge(src1,src2)
```

``` java
src1.mergeWith(src2)
```
no ordering gaurantee. 
 we also have mergeArray.

## b) flatmap

``` kotlin
    val just = Observable.just("apple", "cat")
    just
        .flatMap {Observable.just(it.split("a"))}
        .subscribe{
            print(it)
        };
```

## Concatenating factory and operators

## a) Concat or ConcatWith
 no infinite steam or infinite stream in last 
 
## b) ConcatMap 
   same as flatmap but with concat semantics.

## c) Ambiguous
   among the multiple stream takes the data from which one is the fastest
## d) Zipping
## e) CombineLatest
## f) withLatestFrom
does not care about positioning
## g) groupBy
   groupBy is mix of hot and coldobservable.
   may have some issue use with caution
   

  
   