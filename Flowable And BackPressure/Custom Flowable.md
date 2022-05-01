
# Creating Flowable

``` java
// this is sub optimaal this puts data in the ubounded queue and memory unSufficient
Flowable<Integer> source = Flowable.create(emitter -> {
		for (int i = 0; i <= 1000; i++) {
			if (emitter.isCancelled()) {
			return;
			}
			emitter.onNext(i);
        }
		emitter.onComplete();
	}, BackpressureStrategy.BUFFER);

source.observeOn(Schedulers.io())
	  .subscribe(System.out::println);
sleep(1000);

```


<table style="border: 5px solid green">
 <thead>
  <tr>
  <th>BackPressure Strategy</th>
  <th>Description</th>
  </tr>
 </thead>
<tbody>
 <tr>
  <td> Missing</td>
  <td> no Backpressure startegy , use onBackPressure XXX() operations for this.</td>
 </tr>
 <tr>
  <td> Buffer</td>
  <td> Buffers in queue can , cause out of  memory exception</td>
 </tr>
 <tr>
  <td> Error</td>
  <td>  Generates Back PressureException when subscriber cannot keep up</td>
 </tr>
 <tr>
  <td> Drop</td>
  <td>  drops emmission data </td>
 </tr>
 <tr>
  <td> Latest</td>
  <td> Just takes the latest data</td>
 </tr>
<tbody>
</table>

