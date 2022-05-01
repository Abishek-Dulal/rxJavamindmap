
## OnBackPressureXXX() operators.
If you have a Flowable that has no backpressure implementation (including ones derived
from Observable ), but you need to support backpressure, you can apply
BackpressureStrategy using onBackpressureXXX() operators. They have a few
configuration options.

### onBackPressureBuffer()
The onBackPressureBuffer() takes an existing Flowable that is assumed to not have
backpressure implemented and applies BackpressureStrategy.BUFFER at that point to
the downstream.
```java
Flowable.interval(1, TimeUnit.MILLISECONDS)
		.onBackpressureBuffer()
		.observeOn(Schedulers.io())
		.subscribe(i -> {
					sleep(5);
					System.out.println(i);
		});
sleep(5000);
```

``` java
 .onBackpressureBuffer(10,() -> System.out.println("overflow!"),
						BackpressureOverflowStrategy.DROP_LATEST)
```


<table style="border: 5px solid green">
 <thead>
  <tr>
  <th>BackPressure OverFlow Strategy</th>
  <th>Description</th>
  </tr>
 </thead>
<tbody>
 <tr>
  <td> Error</td>
  <td> no Backpressure startegy , use onBackPressure XXX() operations for this.</td>
 </tr>
 <tr>
  <td> Drop Latest</td>
  <td>  drops emmission data latest </td>
 </tr>
 <tr>
  <td> Drop Oldest</td>
  <td> drops emmission data oldest</td>
 </tr>
<tbody>
</table>


### OnBackPressureLatest
A slight variant of onBackpressureBuffer() is the onBackPressureLatest() operator.
This retains the latest value from the source while the downstream is busy. Once the
downstream is free to process more, it provides the latest value.

### OnBackPressureDrop
The onBackpressureDrop() operator simply discards emissions if the downstream is too
busy to process them