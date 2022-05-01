
# Understanding schedulars
Typically, in Java, you use an ExecutorService as a thread pool. However, RxJava
implements its own concurrency abstraction called Scheduler . This defines methods and
rules that an actual concurrency provider such as an ExecutorService or actor system
must obey. The construct flexibly renders RxJava non-opinionated regarding the source of
concurrency.

Many of the default Scheduler implementations can be found in the Schedulers static
factory class.

## a) Computation
Scheduler maintains a fixed number of threads based on processor availability for your Java session, making it appropriate for computational tasks. Computational tasks (such as math, algorithms, and complex logic) may utilize cores to their fullest extent.

``` java
Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
.subscribeOn(Schedulers.computation());

```

## b) I/O
I/O tasks, such as reading from and writing to databases, web requests, and disk storage
use little CPU power and often have idle time waiting for the data to be sent or received.
it maintains as many threads as there are tasks and dynamically grows the number of
threads, caches them, and discards the threads when they are not needed.But you have to be careful! As a rule of thumb, assume that each subscription results in anew thread.

## c) NewThread
The Schedulers.newThread() factory returns a Scheduler that does not pool threads at
all. It creates a new thread for each Observer and then destroys the thread when it is not
needed anymore. This is different to Schedulers.io() because it does not attempt to
persist and cache threads for reuse
<div style="color:red">Schedulers.newThread() can run amok in a
complex application (as can Schedulers.io() ) and create a high volume of threads,
which could crash the application.</div>

## d) Single
When you want to run tasks sequentially on a single thread, you can use
Schedulers.single() to create a Scheduler . This is backed by a single-threaded
implementation appropriate for event looping. It can also be helpful to isolate fragile, non-
thread-safe operations to a single thread.

## e) Trampoline
A Scheduler created by Schedulers.trampoline() is an interesting one. Most
probably, Its pattern is also borrowed for UI schedulers such as RxJavaFX and
RxAndroid. It is just like default scheduling on the immediate thread, but it prevents cases
of recursive scheduling where a task schedules a task while on the same thread. Instead of causing a stack overflow error, it allows the current task to finish first and to execute that new scheduled task only afterward.

## f) ExecutorService
for control of the thread pools and thread pool policy.
``` java
int numberOfThreads = 20;
ExecutorService executor = Executors.newFixedThreadPool(numberOfThreads);
Scheduler scheduler = Schedulers.from(executor);

Observable.just("Alpha", "Beta", "Gamma", "Delta", "Epsilon")
		  .subscribeOn(scheduler)
		  .doFinally(executor::shutdown)
		  .subscribe(System.out::println);
```
ExecutorService will likely keep your program alive indefinitely, so you have to manage
its disposal if its activity is supposed to be finite. If you want to support the life cycle of
only one Observable subscription, you need to call its shutdown() method. That is why,
in the preceding example, the shutdown() method is called after the processing terminates or is disposed of via the doFinally() operator



## Starting and Shutting down schedulers
Each default Scheduler is lazily instantiated.The Scheduler created by the
computation() , io() , newThread() , single() , or trampoline() factory method can
	be disposed of at any time by calling its shutdown() method.Alternatively, all created
schedulers can be disposed of by calling Schedulers.shutdown() . This stops all their
threads and forbids new tasks from coming in and throws an error if you try otherwise.
You can also call their start() method, or Schedulersers.start() , to reinitialize the
schedulers so that they can accept tasks again.

<div style ="color:blue" > 
On the server side,
however, Java EE-based applications (for example, servlets) may get
unloaded and reloaded and use a different classloader, causing the old
Scheduler instances to leak. To prevent this from occurring, the Servlet
should shut down the Schedulers in its destroy() method.
</div> 
