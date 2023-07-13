# **Function Binding**

## **I. Losing bound object methods**

When running an object method, the value of `this` isn't always bound to the object itself.

If we call object methods in a separate execution context from its object, the value of `this` will become the global object.

For instance, when a variable is assigned an object method, it directly references the function itself. The execution context no longer comes from the object but it is called as a standalone function:

```js
let user = {
	name: "John",
	sayHi() {
		alert(this);
		alert(`Hello, ${this.name}`);
	},
};

let globalThis = user.sayHi;
globalThis(); // (1) Window, (2) Hello, undefined
```

### **Built-in methods**

There are built-in methods like `setTimeout` that create a new execution context separate from its object.

As a result, those methods modify the default binding of an object to the global object:

```js
setTimeout(user.sayHi, 1000); // (1) Window, (2) Hello, undefined
```

## **II. Keeping the context of `this`**

### **Creating a wrapper function**

One solution to preserve the value of `this` when using object methods with `setTimeout` is to wrap the object method in a function wrapper.

This works because only the function wrapper itself is executed in the global execution context, while the contents of the object method are subject to the default binding behavior.

The contents of the wrapper function are subject to the default binding behavior, so an object method retains its `this` binding:

```js
setTimeout(function () {
	user.sayHi();
}, 1000); // (1) { ... }, (2) Hello, John
```

### **Non-blocking function**

Normally, when running code, it blocks the main thread of execution until it has completed running.

A non-blocking function like `setTimeout` allows the main thread of execution to continue while the function runs in the background, allowing other code to run concurrently.

This means that if the result of the function can change if the content is not immutable:

```js
let user = {
	name: "John",
	sayHi() {
		alert(this.name);
	},
};

setTimeout(() => user.sayHi(), 1000); // Username changed: Tom

user = {
	name: "Tom",
	sayHi() {
		alert("Username changed: " + this.name);
	},
};
```

## **III. `bind`**

The `bind` method creates a new function with a fixed `this` value.

The basic syntax is:

```js
func.bind(thisArg);
```

The `bind` method creating a new function means that it's not affected by changes to the original function:

```js
let user = {
	name: "John",
	sayHi() {
		alert(this.name);
	},
};

setTimeout(user.sayHi.bind(user), 1000); // John
user = {
	name: "Tom",
	sayHi() {
		alert("Username changed: " + this.name);
	},
};
```

#### **Binding all object methods**

If we have an object with methods that we plan to keep using, it is possible to bind them to their original object by using a loop:

```js
// binds all methods to its object
function bindAll(obj) {
	for (let key in obj) {
		if (typeof obj[key] === "function") {
			obj[key] = obj[key].bind(obj);
		}
	}
}
```

## **IV. Partial functions**

The `bind` method has a second parameter that allows us to prepend arguments to the bound function to the target function.

This means that it could be used as a _partial function_, where we create a new function by fixing some of the parameters of an existing function:

```js
function multiply(x, y) {
	return alert(x * y);
}

let double = multiply.bind(null, 2);
let triple = multiply.bind(null, 3);

double(3); // 6
triple(3); // 9
```

### **Partial functions without context**

The native `bind` method doesn't allow us to only use the arguments parameter since we need to provide the `this` context first.

If we want to create an implementation of a partial function without context, we need to pass the function and arguments to be bound as parameters in a higher-order function.

The returned function inside the higher-order function should make a function call in the context of the bound function and fixed arguments:

```js
function partial(func, ...argsBound) {
	return function (...args) {
		return func.call(this, ...argsBound, ...args);
	};
}
```
