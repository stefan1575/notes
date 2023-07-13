# **Variable scope, closure**

## **I. Code blocks `{...}`**

When a variable is created, it is only visible inside it's own code block. Meaning that outer variables won't be able to access inner variables.

```js
{
	let inner = "hello";
}

alert(inner); // undefined: cannot access inner variable
```

An exception would be the `for` loop since the variable declared inside of it, is considered part of the code block.

## **II. Function expressions**

### **A. Closed expression**

Functions with _closed expressions_ are those with their own arguments and only depends on their internal data.

1. When a function is called, it gets pushed into the _call stack_, where it's executed.
2. The internal data is only kept in the _stack memory_ until the function is finished running.

```js
function sum(a, b) {
	return a + b;
}

alert(sum(2, 2)); // 4
```

### **B. Open expression**

On the other hand, functions with _open expressions_ can access variables outside of their own code block `{...}`. The referenced data is either located at:

1. _Global scope_ - A variable that is located outside of a function, where it could be accessed anywhere.

   ```js
   let y = 2;
   function open(x) {
   	return x + y;
   }

   alert(open(2)); // 4
   ```

2. _Outer function_ - A variable that is located outside of a nested function.

   ```js
   function nested(x) {
   	let y = 2;
   	return function () {
   		return x + y;
   	};
   }

   alert(nested(2)()); // 4
   ```

## **III. Lexical Environment**

In JavaScript, every running function, code block, and the script as a whole have an associated _lexical environment_.

1. **Environment record** - It's local variables and the value of `this`. The elements inside of their own scope.
2. A reference to the **outer lexical environment** - The outer environment associated with the lexical inner environment. The elements outside of their own scope.

### **A. Variables**

When the script starts, the declared variables are _uninitialized_ until the code associated to it starts executing.

Global lexical environments don't have any outer references:

```js
// execution start - phrase: <uninitialized> -> outer environment = null
let phrase; // phrase: undefined
phrase = "Hello"; // phrase: "Hello"
```

### **B. Function Declarations**

Unlike variables, function declarations are instantly fully initialized. This allows us to use a function even before it is declared.

In this example, we cannot use this function declaration because the variable hasn't been initialized:

```js
// execution start - phrase: <uninitialized>, say: function -> outer environment = null

say("John"); // cannot access lexical declaration 'phrase' before initialization

let phrase = "Hello";
function say(name) {
	return alert(`${phrase}: ${name}`);
}
```

### **C. Inner and outer lexical environment**

In this function call, two lexical environments are created:

- Inner environment - The local scope of the function.
- Outer environment - The global lexical environment, it contains the referenced variable and the function itself.

```js
let phrase = "Hello";
function say(name) {
	return alert(`${phrase}: ${name}`);
}

// inner environment =  name: "John" -> outer = phrase: "Hello"
// outer environment = say: function, phrase: "Hello" -> outer = null
say("John"); // "Hello: John"
```

If a code is trying to access a variable, it looks at the inner environment first, then the outer one, until it reaches the global environment.

- For the variable `"name"`, `alert` finds it immediately in the inner lexical environment.
- For the variable `"phrase"`, since there is no immediate reference, it follows the outer reference and finds it there.

### **D. Returning a function**

1. In the beginning of a nested function, a lexical environment is created on its outer reference:
2. When the outer function reaches the nested function, a hidden property named `[[Environment]]` is created. It keeps a reference to the lexical environment where it was created in

```js
// makeCounter() = count: 0
// Global lexical environment = makeCounter: function, counter: undefined -> outer = null
function makeCounter() {
	let count = 0;

	// [[Environment]] = <empty> -> outer = count: 0 -> makeCounter() ...
	return function () {
		return count++;
	};
}

let counter = makeCounter();
```

### **E. outerFunction.`[[Environment]]`**

So we know that the nested function keeps a reference to its global lexical environment.

When the function is called and the nested function runs:

1. It looks for the variable in its own lexical environment(empty as there are no local variables).
2. Then it looks for it at it's own global lexical environment.

## **IV. Closure**

A closure is a combination of a function combined with references of its _lexical environment_. It allows the nested function to access to its outer scope. In JavaScript, it is set once at creation time.

1. If a function has an open expression, a closure is created.
2. The closure stores the data in _heap memory_, where it could be accessed later.

### **Garbage collection**

The reason why closures aren't garbage collected is because the nested function keeps a reference to its global lexical environment. This means that outer variables are retained.

For instance, the internal anonymous `function()` references `counter` outside of its own scope(code block `{...}`):

```js
function makeCounter() {
	let counter = 0;

	return function () {
		return ++counter;
	};
}

let counter = makeCounter();

alert(functionCounter()); // 1
alert(functionCounter()); // 2
alert(functionCounter()); // 3
```
