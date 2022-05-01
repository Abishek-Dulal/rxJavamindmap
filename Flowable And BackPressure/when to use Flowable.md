
## When to use the Flowable and BackPressure

a) Use an observable if 
 1) Very few observations sub 1000 or the emmissions are  intermittent or  far apart
 2) If the data usage is synchronous  and  has limited usage of concurrency. It includes simple usage of subscribeOn at Observable.
 3) When dealing with Ui event use switching , throttling and windowing etc.

b) Use an Flowable if:-
  1) Dealing with emission greater than 10,000 emmissions in a regulated manner.
  2) IO operations that support blocking while returning  results , which is how many IO sources works.
