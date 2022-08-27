# Learning-RxJS

## What is an Observable?
* An Observable represents a stream or source of data that can arrive over time.
* Observables are cold or do not activate a producer until there is a subscription.

```javascript
// import the fromEvent operator
import { fromEvent } from 'rxjs';

// grab button reference
const button = document.getElementById('myButton');

// create an observable of button clicks
const myObservable = fromEvent(button, 'click');

```

## What is a Subscription?

* Subscriptions are what set everything in motion.
* can think of this like a faucet, have a stream of water ready to be tapped, someone just needs to turn the handle.
* In case of observable, that role belongs to the `subscriber`.

```javascript
// import the fromEvent operator
import { fromEvent } from 'rxjs';

// grab button reference
const button = document.getElementById('myButton');

// create an observable of button clicks
const myObservable = fromEvent(button, 'click');

// for now, let's just log the event on each click
const subscription = myObservable.subscribe(event => console.log(event));
```

* the subscribe method also accepts an object, map to handle the case of error or completion

```javascript
// instead of a function, we will pass an object with next, error, and complete methods
const subscription = myObservable.subscribe({
  // on successful emissions
  next: event => console.log(event),
  // on errors
  error: error => console.log(error),
  // called once on completion
  complete: () => console.log('complete!')
});
```

* each subscription will create a new execution context.
* calling `subscribe` a second time will create a new event listener.
```javascript
// addEventListener called
const subscription = myObservable.subscribe(event => console.log(event));

// addEventListener called again!
const secondSubscription = myObservable.subscribe(event => console.log(event));

// clean up with unsubscribe
subscription.unsubscribe();
secondSubscription.unsubscribe();
```
* Observable source emitting data to observers is a `push based model`.
