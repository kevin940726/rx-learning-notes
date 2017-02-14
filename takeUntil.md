## takeUntil
Use `takeUntil` for cancellation.

We create a button in html with the markup: `<button id="btn">click</button>`.

Every click of the button represent an async call, we want to only emit the latest call, while cancel the other ones.

[jsBin](http://jsbin.com/paliqe/edit?js,console,output)

```js
const source = Rx.Observable.fromEvent(document.getElementById('btn'), 'click')
  .scan(sum => sum + 1, 0)
// We count the click event to show how many times the button is clicked.

const observable = source
  .mergeMap(i => Rx.Observable.of(i)
    .delay(1000)
    // Here we simulate the async call with a delay of 1 second.
    .takeUntil(source)
    // Take the emit value until another click event happened,
    // in other words, cancel the previous async call when another click arrived.
  )

const subscribe = observable.subscribe(val => console.log(val));
// Print result to the console, in this case:
// (click)...1...(click)...2...(click)..(click)...4
```
