# Observable
## Observable types (flow)
 ### Cold Observables
 ### Hot Observables
## Observable types (form)
### Single
### MayBe
### Completeable
## Observable types (function)
### range
### interval
### future
### empty
### never 
### error
### defer
### fromCalleable

# Observable Testing
## Blocking
### Blocking Subscriber
### Blocking First
### Blocking Last
### Blocking Get
### Blocking Iterable
### Blocking ForEach
### Blocking Next
### BlockingMostRecent
## TestSchedular
## TestObserver and TestSubscriber
# Operator
## Action Operator
### a) doOnNext and doAfterError
### b) doOnComplete  and doOnError
### c) doOnEach
### d) doOnTerminate  and doAfterTerminate()
### e) doOnEach 
### f) doOnSubscribe and doOnDispose
### g) doFinally
### h) doSuccess

## Collection Operator
### a) toList()
### b) toSortedList()
### c) toMap and MultiMap
### d) collect

## Conditional Operator
### a) takeWhile
### b) skipwhile
### c) takeUntil
### d) skipUntil
### e) defaultIfEmpty
### e) switchifEmpty

## Error Recovery Operator
### a) OnErrorReturnItem
### b) onErrorReturn
### c) onErrorResumeWith
### d) retry

## Reducing Operator
### a) count
### b) reduce
#### a) all
#### b) any
#### c) empty
#### d) contains
#### e) SequenceEqual


## Suppressing Operator
### a) filter
### b) take
### c) skip
### d) distinct
### e) distinctUntilChanged
### f) elementAt


## Transformation Operator
### a) map
### b) cast
### c) startWithItem
### d) Sorted
### e) Scan


## Utility Operator
### a) delay
### b) repeat
### c) single 
### d) timestamp
### e) timeInterval


## Observable Combining Operator
### a) Concat or ConcatWith
### b) ConcatMap 
### c) Ambiguous
### d) Zipping
### e) CombineLatest
### f) withLatestFrom
### g) groupBy
### h) flatmap
### i) merge or mergewith
# Concurrency
## observeOn
## subscribeOn
## unsubscribeOn
## Parrallelisation using flat map  
## Schedulers
 ### computation
 ### Io
 ### New Thread
 ### Trampoline
 ### Executor Service
 ### Single
## Flowable
### BackPressure Strategy
#### BackPressureDrop
#### BackPressureLatest
#### BackPressureBuffer
### Convert Flowable and observable
### CustomFlowable
### Flowable generate
### Subscriber
### BackPressure
#### Buffering
##### Fixed sized 
##### Time based 
##### Boundary based 
#### Windowing
##### Fixed sized 
##### Time based 
##### Boundary based 
#### Throttling
#####   last (sample)
#####   first
#####  withTimeOut ( debounce ) 

# Custom rxjava data
 ## ObservableOperator
 ## FlowableOperator
 ## ObservableTransforemer
 ## FlowableTransforemer
 
# Disposable
## Resource Observable
## Composite disposable
## Set Cancelable and disposable

# Multicasting
 ##  replay
 ##  cache
 ## connect
 ## refcount and share
 ## autoconnect
## Subject
### async
### unicast
### replay
### Behaviour

 



 







 
 



 