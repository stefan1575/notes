# **`async`/`await`**

The `async function` expression allows us to pause the execution of a function using the `await` operator and returns a promise.

If the return value is not a promise, automatically wrapped into a resolved promise.

The `await` operator pauses the execution of the `async function` until the specified promise gets its fulfillment value.

```js
async function f() {
	let promise = new Promise((resolve, reject) => {
		setTimeout(() => resolve("Async code"), 1000);
	});

	let value = await promise; // paused until settled
	console.log(value);
}

f();
```
