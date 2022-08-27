# Learning-RxJS

## **Table of contents**
* [What is Observable](#what-is-an-observable)
* [What is Subscription](#what-is-a-subscription)
* [Operators](#operators)
* [Pipe](#pipe)
* [Operators catagories](#operators-catagories)
  * [Creation](#creation-operators)
  * [Combination](#combination-operators)
  * [Error handling](#error-handling-operators)
  * [Filtering](#filtering-operators)
  * [Multicasting](#multicasting-operators)
  * [Transformation](#transformation-operators)
* [Operators with common behavior](#operators-with-common-behavior)

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
## Operators
* Operators offer a way to manipulate values from a source, returning an observable for the transformed values.

* if you want to transform emitted values from an observable, you can use `map`:
```javascript
import { of } from 'rxjs';
import { map } from 'rxjs/operators';
/*
 *  'of' allows you to deliver values in a sequence
 *  In this case, it will emit 1,2,3,4,5 in order.
 */
const dataSource = of(1, 2, 3, 4, 5);

// subscribe to our source observable
const subscription = dataSource
  .pipe(
    // add 1 to each emitted value
    map(value => value + 1)
  )
  // log: 2, 3, 4, 5, 6
  .subscribe(value => console.log(value));
```
* if you want to filter for specific values, you can use `filter`:
```javascript
import { of } from 'rxjs';
import { filter } from 'rxjs/operators';

const dataSource = of(1, 2, 3, 4, 5);

// subscribe to our source observable
const subscription = dataSource
  .pipe(
    // only accept values 2 or greater
    filter(value => value >= 2)
  )
  // log: 2, 3, 4, 5
  .subscribe(value => console.log(value));
```
* if there is a problem you need to solve, it's more than likely there is an operator for that.

## Pipe
* The `pipe` function is the assembly line from your observable data source through your operators.
* source data can pass through a `pipe`-line of operators where you can manipulate, filter, and transform the data to fit your use case
* for example, a typeahead solution built with observables may use a group of operators to optimize both the request and display process:
```javascript
// observable of values from a text box, pipe chains operators together
inputValue
  .pipe(
    // wait for a 200ms pause
    debounceTime(200),
    // if the value is the same, ignore
    distinctUntilChanged(),
    // if an updated value comes through while request is still active cancel previous request and 'switch' to new observable
    switchMap(searchTerm => typeaheadApi.search(searchTerm))
  )
  // create a subscription
  .subscribe(results => {
    // update the dom
  });
 ```
 
## Operators catagories
### Creation Operators
* The most commonly used creation operators: `of`, `from` and `fromEvent`
### Combination operators
* The most commonly used combination operators: `combineLatest`, `concat`, `merge`, `startWith` and `withLatestFrom`
### Error handling operators
* The most commonly used error handling operators: `catchError`
### Filtering operators
* The most commonly used filtering operators: `debounceTime`, `distinctUntillChanged`, `filter`, `take` and `takeUntill`
### Multicasting operators
* The most commonly used multicasting operators: `shareReplay`
### Transformation operators
* The most commonly used transformation operators: `concatMap`, `map`, `mergeMap`, `scan` and `switchMap`

## Operators with common behavior
* Switch based operators: `switchAll`, `switchMap`, `switchMapTo`
* Concatation based operators: `concat`, `concatAll`, `concatMap`, `concatMapTo`
* Merge based operators: `merge`, `mergeMap`, `mergeAll` 
