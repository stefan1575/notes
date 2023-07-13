# **Arrow function expression**

## **I. Arrow function**

An arrow function is an anonymous function expression that takes its `this` context from its surrounding scope.

- `() => expression` - Parenthesis are needed when there is no argument.
- `arg => expression` - You can omit the parenthesis if there is only one argument.

The syntax:

```js
let arrow = () => expression;
```

### **One-line arrow function**

In a one-line arrow function, the expression will become the arrow function's return value.

```js
let sum = (a, b) => a + b;

alert(sum(1, 2)); // 3
```

Just like regular function expressions, arrow functions could also be used to dynamically create a function, where it's created at run-time instead of pre-defining it:

```js
let age = prompt("What is your age?", 18);

let welcome = age < 18 ? () => alert("Hello!") : () => alert("Greetings!");

welcome();
```

### **Multline arrow functions**

Arrow functions with multiple lines are wrapped in curly braces. Unlike one-line arrow functions, we need to specify an explcit `return`.

```js
let sum = (a, b) => {
	let result = a + b;
	return result;
};

alert(sum(1, 2)); // 3
```

## **II. Object Methods - value of `this`**

When an arrow function is used as object method, the value of `this` is not taken from the object that it resides in, but its surrounding context.

### **No binding `arguments` object**

Arrow functions do not have their _own_ `arguments` object. Its reference will be taken from the surrounding scope.

For instance, if an arrow function wasn't used, the context of the `arguments` would be the nested function:

```js
function sayArgs(a, b, c) {
	let arrow = (x, y, z) => {
		for (let i = 0; i < arguments.length; i++) {
			console.log(arguments[i]);
		}
	};

	return arrow(2, 2, 2);
}

sayArgs(1, 1, 1); //
```
