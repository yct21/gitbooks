# Observable

Observable 是数据的生产者。

## Observable 的产生

> 当你有了一把锤子，看到的所有东西都是钉子

Observable 是 RxJS 中最重要的实体。根据实际的数据源的性质，可以用不同的方法将其
转化为 Observable。

- Event emitter
- Static data
- Generated data

```js
// Single value, synchronous
Rx.Observable.of(42).subscribe(console.log);

// Multiple value, synchronous
Rx.Observable.from([1, 2, 3]).subscribe(console.log);

const fortyTwo = new Promise((resolve, reject) => {
   setTimeout(() => {
      resolve(42);
   }, 5000);
});

// Single value, asynchronous
Rx.Observable.fromPromise(fortyTwo)
   .map(increment)
   .subscribe(console.log); //-> 43

// Multiple value, asynchronous
Rx.Observable.fromEvent(addEmitter, 'add', (a, b) => ({a: a, b: b}))
   .map(input -> input.a + input.b)
   .subscribe(console.log); //-> 5

addEmitter.emit('add', 2, 3);
```

## pull-based paradigm

Event emitter 在 JavaScript 中有很长的历史，和 RxJS 中的 stream 类似，它可以将
event 以一组序列的方式进行处理。和 stream 的不同点在于，Event emitter 对数据的处
理方法，既可以 pull base, 也可以 push base，取决于使用者。

此外，出现在 JavaScript 中不久的 Generator，则是一个 push base 的处理异步的方法。

RxJS 的 Observable 采用 push-based ntification。
