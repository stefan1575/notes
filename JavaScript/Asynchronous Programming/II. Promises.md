# **Promises**

The promise object is a proxy for a value that is only known when an asynchronous operation has completed or failed. A promise is considered "settled" when it is finally evaluated.

- `<state>` - The initial state is `"pending"`
  - `"fulfilled"` - when `resolve` is called
  - `"rejected"` - when `reject` is called
- `<value>` - The initial state is `undefined`
  - `value` - the passed value of the called `resolve`.
  - `Error` - the passed error object of the called `reject`.

## **Executor**

The promise constructor takes a function called `executor` which takes two callbacks as arguments that determines the `<value>` of the promise object.

- `resolve(value)` - returns a `value` when called.
- `reject(Error)` - returns an `Error` object when called.

The executor is called immediately upon creation of the constructor.

This means that the code in the initial creation of the promise is ran synchronously, only the settlement of the promise is queued as a microtask.

If we want to guarantee asynchronous execution, we should run it outside of an executor:

```js
console.log(`Synchronous: #1`);

new Promise((resolve, reject) => {
	console.log(`Synchronous: #2`);
	resolve();
}).then(() => console.log(`Asynchronous: #4`));

console.log(`Synchronous: #3`);
```

## **I. Promise chaining - `then`**

When an asynchronous operation is evaluated, another subsequent operation is usually performed.

To run code after an asynchronous operation has completed, we can create another promise object that runs after the previous promise has been settled.

The following methods create a new promise object and run when the previous promise object is fulfilled, where its `<value>` gets passed as an identity function.

- `then` - executes when the previous promise has `fulfilled` state.
- `catch` - executes when the previous promise has `rejected` state.
- `finally` - executes when the previous promise has settled.
  - Since it doesn't have any arguments, it doesn't get the outcome of the previous handler.
  - The previous `<value>` is passed to the next suitable handler unless an error is thrown.
    - If an error is thrown its value gets passed to the nearest `catch` block.
  - The return value is silently ignored.

Results must be always returned, otherwise we can't track the previous chained `<value>`.

### **Returning promises**

If we need to chain another subsequent asynchronous operation we can return a promise instead of a value.

It is recommended to make an asynchronous action should always return a promise for it to be extendable.

### **Chaining after macrotasks**

If the asynchronous action involves macrotasks, we should resolve the promise when the macrotask finishes executing using `resolve(value)`.

## **II. Error Handling - `catch`**

When a promise is rejected, the control flow goes to the nearest `catch` block.

### **Rethrowing**

If a caught error is rethrown in a `catch` block, the control just goes to the next `catch` in the chain.

```js
new Promise((resolve, reject) => {
	// Promise is rejected
})
	.catch(function (Error) {
		// error is rethrown
	})
	.then(() => {
		// skipped
	})
	.catch(() => {
		// the error is resolved here
	});
```

### **Window: `unhandledrejection`**

The `unhandledrejection` event occurs when a Promise that has no rejection handler is rejected. The error is sent to the global scope.

## **III. Promise concurrency methods**

The following methods take an iterable of promises and execute all of them in parallel.

- `Promise.all()` - fulfills when all promises fulfill, rejects when any promise rejects.
  - Returns an array of values or the `reason` of the first rejected promise.
- `Promise.allSettled()` - fulfills when all promises settle, which returns an array of outcome objects with the following properties:
  - `status` - a string, either `"fulfilled"` or `"rejected"`.
  - `value/reason` - the value that the promise was fulfilled/rejected with.
- `Promise.race()` - returns the first settled promise.
- `Promise.any()` - returns the first fulfilled promise.
  - if all promises are rejected, returns `AggregateError`.
