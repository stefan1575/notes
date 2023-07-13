# **Functions: Creation and Assignment**

## **I. Function declaration**

A function declaration is _hoisted_ which means that it can be used even before its defined:

```js
declaration(); // "Hello World"

function declaration() {
	alert("Hello World");
}
```

## **II. Function expression**

A function expression is when a function is assigned as the value of a variable.

Unlike function declarations, they aren't _hoisted_, meaning that it must be defined before it can be used:

```js
expression(); // undefined

let expression = function () {
	alert("Hello World");
};
```

## **III. Function Reference**

When assigning a value to a function you are creating a reference to the function rather than creating a new copy of the function.

### **Garbage collection**

As long as there is at least one reference to a function, it will not be lost, even if the original reference is lost:

```js
function sum(a, b) {
	return a + b;
}

let add = sum;
sum = null;

alert(add(2, 2)); // 4
```

### **Modifying a function through its reference**

Any changes you make to the function will affect the original.

For example, we can modify a function by changing its reference as being passed as an argument to another function.

This results in the original function being wrapped in another function that add additional behavior. For the original function to work, it needs to be called by the modifier function with the arguments that you want to use:

```js
// modifiedFunction
function calculatorLog(operation) {
	return function (...numbers) {
		// additional behavior
		console.log(operation(...numbers));
	};
}

// originalFunction
function addition(a, b) {
	// original behavior
	return a + b;
}

function subtraction(a, b) {
	return a - b;
}

// originalFunction = modifiedFunction(originalFunction)
addition = calculatorLog(addition);
subtraction = calculatorLog(subtraction);

addition(10, 5); // 15
subtraction(10, 5); // 5
```

## **IV. Execution Context and Returned functions**

When a function is called, an execution context is created for that function. The _call stack_ keeps track on what functions are paused and currently running.

If a function returns a function:

1. The original function's context is paused until the returned function completes execution
2. The returned function's context is added to the call stack and currently runs

In the example above, when `calculatorLog(operation)` reaches the returned anonymous function, the context is paused while the code inside finishes executing.

The nested function gets paused again since it needs to evaluate a function. The call stack currently looks like this:

1. `calculatorLog(operation)`
2. `function(...numbers)`
3. `addition(...numbers)`

Each of the functions get popped off the call stack once they finish evaluating, passing the value to the next function until the outermost function gets the return value.
