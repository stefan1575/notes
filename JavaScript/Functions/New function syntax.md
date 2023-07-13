# **`new Function()` syntax**

The `new Function()` syntax is used to turn strings into a function.

The function parameters are optional and the `functionBody` accepts strings and variables with strings:

```js
let func = new Function([arg1, arg, ...argN], functionBody);
```

## **Closure**

When a closure is created, it references the lexical environment where it was created. But when they are used with the `new Function` syntax, it will reference the global environment:

```js
let value = "GlobalEnvironment";

function outputEnvironment() {
	let value = "LocalEnvironment";
	let closure = new Function("alert(value)");

	return closure;
}

outputEnvironment()(); // GlobalEnvironment
```

This is useful for when the variables are only going to be known at the time of execution.

The problem with storing them in the global environment is that JavaScript compresses them in a _minifier_. As a result, the variables provided to the string-form functions don't work.

So the best way to pass a function is to **pass them as arguments**.
