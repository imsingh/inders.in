---
layout: post
title: Learning RxJS by Creating Observables
subtitle: Deep dive into Observables
category: blog
comments: true
---

<div class="alert alert-primary" role="alert">
    This blog post was originally published in <a class="alert-link" href="https://www.zeolearn.com/magazine/learning-rxjs-by-creating-observables-introduction" target="_new">ZeoLearn Blog.</a>
</div>

**TL;DR:** In this article, you will learn how to implement custom observables via `Observable.create` method. Furthermore, you will also learn why Observables are lazy. Finally, you will re-create RxJS's `fromEvent` and `of` method to understand the library bit more.

## Introduction
RxJS is the Library for doing Reactive Functional Programming in JavaScript. It combines [Observer pattern](https://en.wikipedia.org/wiki/Observer_pattern) with [Iterator pattern](https://en.wikipedia.org/wiki/Iterator_pattern) and also adds Array's extras like `map`, `filter`, `reduce` and other similar methods on the top of it. Key thing is, you can do functional programming with Asynchronous events with RxJS.

> Think of RxJS as Lodash for events.
> -- RxJS Docs

Before you advance to the coding part. Following are some terms that you should know.

* `Observable` : It's basically a collection of events.
* `Observer` : Collection of callbacks, which listens to the value emitted by Observable.
* `Subscription` : It basically represents the invocation of Observable. You can unsubscribe to Observable via Subscription.

## Anatomy of an Observable
On API level, Observable is an Object with `subscribe` method on it. This `subscribe` method takes `observer` as an argument.

```js
{ 
    subscribe(observer);
}
```
This observer object has three methods: `next`, `error`, `complete`.

1. To emit a value, you can call the `observer.next` method with the value that you want to emit.
2. In case there is an error, you can emit that error via `observer.error`.
3. Finally, if everything is finished. You can call `observer.complete` method to complete the observable.

Following is an example of an `Observer`.

```js 
{
    next: x => console.log(x),
    error: e => console.log(e),
    complete: () => console.log('complete')
}
```

You can wrap this pattern around any Push API of the Browser.

### `setInterval` Example

Let's take the example of `setInterval`.

Take a look at following snippet.
```js
const timeId = setInterval(() => { console.log('setInterval'); }, 1000);

// Stoping the Interval
clearInterval(timeId);
```
You called `setInterval` with an anonymous function. `setInterval` will run this function every one second. It also returns a `timeId` for referencing this interval further. To stop the execution at any point, you have to call `clearInterval` with the `timeId` as shown above.

Now, let's wrap it around `Observable` as follows:

```js
function setIntervalObservable(time) {
    return {
        subscribe: (observer) => {
            const timeId = setInterval(() => { console.log('setInterval'); }, 1000);
            // Teardown logic here
            return {
                unsubscribe: () => {
                    clearInterval(timeId);
                }
            }
        }
    };
}
```

Let's understand this whole bunch of code.
The **`setIntervalObservable`** function takes `time` as input and returns an `Observable`. This Observable has `subscribe` method which takes `observer` as input. When you subscribe to that `Observable`, it fires the `setInterval`. This `subscribe` method also returns an object with `unsubscribe` method to stop the interval.

You can use this `Observable` like this:
```js
const interval$ = setIntervalObservable(1000);
const subscription = interval$
                    .subscribe({ next: () => console.log('interval') });

// Stoping Interval at some point
subscription.unsubscribe();
```

### Why Observables are lazy?

If you take a closer look at the following code:  
```js 
const interval$ = setIntervalObservable(1000);
``` 
You have created an Observable. But it is not going to do anything until you fire `subscribe` method. Because the actual work is done inside `subscribe` method in the Observable's implementation. That's why Observables are **`lazy`**.

## Observable.create

I was not completely honest with you when I said 
> Observable is an Object with `subscribe` method on it.

The Real implementation in RxJS is a bit more complex. I am not going to talk about that complexity here. But in order to use your custom Observable with RxJS's other methods. You need to use RxJS's `Observable.create` method for creating your custom Observables.

This `Observable.create` method takes `subscribe` function and returns an `Observable`.
```js
Observable.create(subscribe): Observable<any>;
```

This subscribe function is like a blueprint of the Observable where you would define the Observable. It takes `observer` object as an argument. 
Now let's create our `setInterval` Observable with `create` method.

```typescript
import { Observable } from 'rxjs';

function setIntervalObservable(time) {
    return Observable.create((observer) => {
        const timeId = setInterval(() => { console.log('setInterval'); }, 1000);

        // Teardown logic here
        return () => {
            clearInterval(timeId);
        }
    });
}

const interval$ = setIntervalbservable(1000)
                .subscribe(() => console.log('interval'));

// To unsubscribe from Observable
interval$.unsubscribe();
```

If you compare this code with one without `create` method, the major difference is that you have to pass `subscribe` function to `create` method. Also, you have to return a function inside `subscribe` method instead of an `object` with `unsubscribe` method. This is all handle by RxJS for us. 

## Case Study: Creating RxJS creation methods
In this section, you are going to implement very basic version of RxJS's `fromEvent` and `of` method. Idea is to give more insight about how these methods work and how you can create custom observables according to your own need. It's highly likely that the actual implementation is more complex. 
### Creating `fromEvent` method
`fromEvent` method in RxJS library has following signature:

```js
fromEvent(target:EventTargetLike, eventName:string)
:Observable<T>;
```
> Creates an `Observable` that emits `events` of a specific type coming from the given event `target`. -- RxJS Docs

So basically, with this method, you can listen to events on a specific DOM Node in *Observable way*. For example:

```js
import { fromEvent } from 'rxjs';

fromEvent(document.querySelector('#myButton', 'click'));
```

The above code creates an `Observable` which listens to `click` event on HTML element with `myButton` id.

#### Implementation of `fromEvent`
Let's implement it ourselves.

```js
import { Observable } from 'rxjs';

function fromEvent(dom, event) {
    return Observable.create((observer) => {
        const handler = (event) => { 
            observer.next(event); 
        };
        dom.addEventListener(event, handler);

        return () => {
            dom.removeEventListener(handler);
        }
    });
}
```

#### Explanation
It's fairly simple, in the `subscribe` function, you created a `handler` function which passes the values to subscriber via `object.next`. This handler is attached as an event listener for that specific DOM node and event. In the `unsubscribe` method, you are removing the event listener to clean up.

### Creating `of` method
`of` method has following signature:

```js
of(value):Observable<T>;
```
> Creates an `Observable` that emits some values you specify as arguments, immediately one after the other, and then emits a `complete` notification. -- RxJS Docs

Basically, it emits all the value that you have specified in arguments one by one and then completes. For example:
```js
import { of } from 'rxjs';

of(1,2,3);
```
The above code creates an Observable which will emit `1`,`2`,`3` and then completes.

#### Implementation of `of` method
```js
import { Observable } from 'rxjs';

function of(...values) {
    return Observable.create((observer) => {
        const handler = () => {
            values.forEach(val=> {
                observer.next(val);
            });
            observer.complete();
        };
        handler();
    });
}
```

#### Explanation
Inside `subscribe` method, you created a handler function, which forEach over all the inputs of `of` method and it fires `observer.next` with each input. Then it fires `observer.complete()` and finishes. And finally, we are calling `handler` function.

## Conclusion
In this post, you have learned how to create `Observable` via `create` method. Along with that, you have learned why Observables are lazy. In the end, you also did a case study on RxJS's `fromEvent` and `of` method.