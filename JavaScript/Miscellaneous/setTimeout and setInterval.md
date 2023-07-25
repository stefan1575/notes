# **Scheduling: `setTimeout` and `setInterval`**

The following methods allow us to run functions _after_ a specific amount of time:

- `setTimeout` - Executes a function after a fixed time delay.
- `setInterval` - Repeatedly executes a function with a fixed time delay after each call.

## **I. `setTimeout`**

Providing the `delay` and `funcParam` is optional. The syntax is:

```js
let timerId = setTimeout(func, [delay], [funcParam1], [funcParam2], ...);
```

- `func` - The function or string of code to execute. Strings of code is only used for historical reasons.
- `delay` - The delay before the code is ran in milliseconds(ms), by default the value is `0`.
- `funcParam` - Arguments to be passed by `func`.

For instance, we can output a string after one second:

```js
function sayHi(message, person) {
	return alert(`${message}, ${person}`);
}

setTimeout(sayHi, 1000, "Hi", "John"); // Hi, John
```

Since `setTimeout` expects a reference to a function, we shouldn't immediately execute it.

When passing a function with brackets `"()"`, the result of execution is passed:

```js
setTimeout(sayHi(), 1000); // undefined
setTimeout(sayHi, 1000); // Hi, John
```

### **Canceling: `clearTimeout`**

A call to `setTimeout` returns a "timer identifier" that can be used to cancel the execution.

The syntax is:

```js
let timerId = setTimeout(...);
clearTimeout(timerId);
```

For instance, although the function specified to be ran is cancelled, `setTimeout` returns a "timer identifier" which is separate from the function to be delayed:

```js
let timerId = setTimeout(() => alert("Never happens"), 1000);
alert(timerId); // timer identifier (number);

clearTimeout(timerId);
alert(timerId); // the value is stil retained;
```

## **II. `setInterval`**

The `setInterval` method has the same syntax as `setTimeout`. The only difference is that it runs repeatedly after the given `delay`:

```js
let timerId = setInterval(func, [delay], [funcParam1], [funcParam2], ...);
```

To stop further calls, use `clearInterval(timerId)`:

```js
let timerId = setInterval(() => alert("2s has elapsed"), 2000);

// 5 seconds has elapsed
setTimeout(() => {
	clearInterval(timerId);
	alert("5s has elapsed in total");
}, 5000);
```

## **III. Nested `setTimeout`**

There are two ways to run something at an interval of time, `setInterval` and a _nested `setTimeout`_:

```js
// setInterval
let interval = setInterval(() => alert("2s has elapsed"), 2000);

// nested setTimeout
let timerId = setTimeout(function interval() {
	alert("2s has elapsed");
	nestedTimeout = setTimeout(interval, 2000);
}, 2000);
```

The difference is that we impose a set of conditions for the nested `setTimeout`, which may execute differently depending on the results of the current one.

For instance, a confirm prompt keeps running every 5 seconds until the the `confirm` prompt is cancelled:

```js
function kickUser() {
	let timeOut = 5000;
	let timerId = setTimeout(statusUpdate, timeOut);

	function statusUpdate() {
		let userStatus = confirm("Are you still active");
		if (userStatus) {
			kickUser();
		} else {
			alert("User has been kicked out of this session.");
			clearTimeout(timerId);
		}
	}
}

kickUser();
```

## **IV. Interval delay precision**

Using a nested `setTimeout` allows us to set the delay more accurately than `setInterval`.

### **A. `setInterval`**

The real delay in `setInterval` is less than the code since the time taken by the execution of the function consumes part of the interval.

There is a consequence of this behavior. In edge cases, if a functions execution takes longer than the specified delay, it runs immediately if the time is up.

### **B. Nested `setTimeout`**

Unlike `setInterval` a nested `setTimeout` waits for the function to finish executing before the internal scheduler starts counting.

## **V. Garbage collection**

When a function is passed inside `setInterval` or `setTimeout`, an internal reference is created and saved in the scheduler. This prevents is from become garbage collected.

Since an internal reference is stored by the function, outer variables are stored in memory.

To prevent this we should always clear the scheduled function:

```js
// an internal reference is stored in memory
let timerId = setTimeout(func, 100);

// reference is cleared and garbage collected
clearTimeout(timerId);
```

## **VI. Setting the delay to `0`**

When setting the delay of either `setTimeout` or `setInterval` to zero, the scheduler will only invoke the code with a delay of zero after the **script** finishes executing.

For instance, the invoked `setTimeout` with zero delay goes after the code finishes executing even if it is called first:

```js
function zeroDelay() {
	return setTimeout(() => alert("world"));
}

zeroDelay(); // (2) world
alert("hello"); // (1) hello
```

After 5 nested timers, the interval is forced to be _at least_ 4 milliseconds.

This limitation comes from historical reasons. For server side JavaScript, this limitation does not exist.

## **Summary**

- `setTimeout` - executes a function once after the specified delay.
- `setInterval` - executes a function repeatedly specified by the delay.
- To cancel the execution, use `clearTimeout` or `clearInterval` with the timer identifier.
- Using a nested `setTimeout` allows us to impose a set of conditions and is more precise than
- Zero delay scheduling is used to schedule the call immediately after the script is complete.
- After 5 nested `setTimeout` or for `setInterval` calls, the interval is forced to be at least 4ms.

Note that all scheduling methods do not _guarantee_ the exact delay.
