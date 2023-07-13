# **Decorators and forwarding, `call`/`apply`**

## **I. Decorator**

A decorator is a special function that takes another function, and returns a function where it could be modified.

It is applied to a function when it's passed as an argument to a decorator function, resulting in a "wrapper".

The syntax is:

```js
let normalFunction = decorator(normalFunction);
```

### **Transparent Caching**

One use case of a decorator function is transparent caching where we store its result to bypass recalculation of the function.

This is useful if we want to skip recalculation of repeatedly called functions, especially if it's compute heavy.

```js
function computeFunction(x) {
	// there can be compute heavy code
	alert(`Compute: ${x} + 1`);
	return x + 1;
}

function storeCache(func) {
	let cache = new Map();

	return function (x) {
		if (cache.has(x)) {
			alert(`Retrieved from cache: ${cache.get(x)}`); // retrieve value from map
		} else {
			let result = func(x); // store result as value
			cache.set(x, result); // store key-value pair
			return alert(`Computed result: ${result}`);
		}
	};
}

computeFunction = storeCache(computeFunction);

computeFunction(1); // (1) Compute: 1 + 1, (2) Computed Result: 2
computeFunction(1); // Retrieve from cache: 2
```

### **Function properties**

The decorated function is a new, separate function with its own context and set of properties.

This means that if the nested function had any properties, it doesn't exist if it's ran inside another function unless we store it by using a closure.

## **II. `this` context on wrapper**

### **A. Object Methods**

When running object methods, the value of `this` is the object that is used to call the function.

If we try to use an object method inside a different object, it the value of `this` will refer to the current object. This means the available properties to be accessed are different.

For instance, when running an object method inside a function, the context of `this` becomes the current function. Since the property that it's trying to access doesn't exist in the current function, it returns `undefined`:

```js
let mathematics = {
	operand() {
		return 2;
	},

	doubleNumber(x) {
		alert(`Compute: ${x} * ${this.operand()}`);
		return x * this.operand();
	},
};

function storeCache(func) {
	let cache = new Map();
	return function (x) {
		if (cache.has(x)) {
			let retrieve = `Cached Result: ${cache.get(x)}`;
			return retrieve;
		} else {
			let result = func(x); // error: context of "this" is the current function
			cache.set(x, result);
			return `Computed Result: ${result}`;
		}
	};
}
alert(mathematics.doubleNumber(2)); // works

// lose context of "this"
mathematics.doubleNumber = storeCache(mathematics.doubleNumber);
alert(mathematics.doubleNumber(2)); // undefined
```

The object method context is also lost when passed as a value inside a variable, so the current context becomes the variable:

```js
let doubleNumber = mathematics.multiply;
doubleNumber(2); // undefined
```

### **Functions**

The context of `this` inside a nested function will also be global context.

1. To access the nested function, we need to create a function by assigning it with the decorator function and the arguments we want to use.
2. For the nested function to work, it needs to return a function.
3. Use the decorated function to be able to run the nested function.

```js
function delay(func, time) {
	return function () {
		setTimeout(() => {
			func.apply(this, arguments);
		}, time);
	};
}

function sayHello(name) {
	alert(`Hello, ${name}!`);
}

let sayHello1000 = delay(sayHello, 1000);

sayHello1000("John"); // Hello, John!
```

## **III. `call`**

The `call` method calls the function with the given `this` value with the arguments provided individually.

The provided object which will be the context of `this`, allows us to access it's own specific properties even if it's called outside of its own context.

### **Parameters**

When using the `call` method, the `object` is specified first before providing the arguments that is native to the function that it's called with.

The syntax is:

```js
func.call(object, arg1, arg2, ...argN);
```

The only difference between a function with and without a `call` method is that the `this` context is taken from a different environment. The arguments are passed like a normal function:

```js
// does the same thing
func(1, 2);
func.call(object, 1, 2);
```

### **Using `call`**

For instance, since we need to run the object method `call` with a function, we can run a nested function so that we won't need to keep appending the `call` method for future function calls:

```js
let object1 = {
	arg1: 2,
	arg2: 2,
};

let object2 = {
	arg1: 4,
	arg2: 4,
};

function sum(object) {
	function nested() {
		alert(`${this.arg1} + ${this.arg2} = ${this.arg1 + this.arg2}`);
	}
	return nested.call(object);
}

sum(object1); // 2 + 2 = 4
sum(object2); // 4 + 4 = 8
```

In the case of function wrapper, we should use `call` to access the object:

```js
function storeCache(func) {
	let cache = new Map();
	return function (x) {
		if (cache.has(x)) {
			return `Cached Result: ${cache.get(x)}`;
		} else {
			// original context is kept
			let result = func.call(this, x);
			cache.set(x, result);
			return `Computed Result: ${result}`;
		}
	};
}

mathematics.doubleNumber = storeCache(mathematics.doubleNumber);
alert(mathematics.doubleNumber(2)); // run function
alert(mathematics.doubleNumber(2)); // retrieve from cache
```

### **Arguments are provided individually**

To pass multiple arguments to the call() method without providing them individually, you can use the `arguments` object to pass the arguments as an array.

A use case for this technique would be storing the arguments as a single value for use as a key in a map.

For instance, if you want to cache the results of a function call with multiple arguments, you can store the arguments as a single key in a map and retrieve the cached result when the same arguments are passed to the function again.

A hashing function can be used to convert the array of arguments into a string that can be compared with the original arguments to determine if they match.

```js
let mathematics = {
	sum(x, y, z) {
		alert(`Compute: ${x} + ${y} + ${z}`);
		return x + y + z;
	},
};

function storeCache(func, hash) {
	let cache = new Map();
	return function () {
		let key = hash(arguments);
		if (cache.has(key)) {
			cache;
			return `Cached Result: ${cache.get(key)}`;
		} else {
			let result = func.call(this, ...arguments);
			cache.set(key, result);
			return `Computed Result: ${result}`;
		}
	};
}

function hash(args) {
	return args[0] + "," + args[1];
}

mathematics.sum = storeCache(mathematics.sum, hash);

alert(mathematics.sum(2, 2, 2)); // (1) Compute: 2 + 2 + 2, (2) Computed Result: 6
alert(mathematics.sum(2, 2, 2)); // Cached Result: 6
```

## **IV. `apply`**

The `apply` method also calls the function with the given `this` value, where it can only accept arrays or array-like objects as arguments.

The syntax is:

```js
func.apply(object, args);
```

### **`apply` vs. rest parameters**

The `call()` method with rest parameters can only accept iterables, while the `apply()` method can accept both iterables and array-likes.

## **V. Call forwarding**

In JavaScript, call forwarding involves redirecting a function call with its arguments and context to another function using the `call()` or `apply()` methods.

For the forwarding function to work properly, the parameter list of the forwarded function must be provided as arguments:

```js
function outer() {
	function sum(a, b) {
		return a + b;
	}

	// "sum" is forwarded to "forwardSum" using call()
	return function forwardSum(a, b) {
		return sum.call(this, a, b);
	};
}

let sum = outer();
alert(sum(2, 2));
```

## **VI. Method borrowing**

Method borrowing allows the use of one object's method on another object temporarily using the `call` or `apply` method. This allows for code reuse without duplicating the method.

The borrowed method still belongs to the original object and is not permanently added to the object it is borrowed for.

```js
let obj1 = {
	sum(a, b) {
		return a + b;
	},
};
let obj2 = {};

function borrowMethod(obj1, obj2, methodName, ...args) {
	// Borrow the method from obj1 and call it on obj2
	return obj1[methodName].call(obj2, ...args);
}

alert(borrowMethod(obj1, obj2, "sum", 2, 2)); // 4
alert("sum" in obj2); // false
```

### **Built-in methods**

We can also implement method borrowing on built-in methods by converting the target object into a format that is compatible with the function we are calling.

By using method borrowing, we can retain the functionality of the array-like `arguments` such as index access while also using array methods.

```js
function hash() {
	alert(arguments[0]);
	return [].join.call(arguments);
}
```

If we converted the `arguments` object directly into an array we would have permanently modified its behaviour and lost index access behavior.
