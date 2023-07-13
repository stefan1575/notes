# **Function Properties**

In JavaScript, a functions are callable objects where we can call them, add/remove properties, and pass them by reference.

## **I. `name` property**

The `name` which returns the defined `functionName` and an empty string `''` if it's an anonymous function.

```js
let function functionName() {
    alert(functionName.name)
}
functionName(); // functionName
```

There are other ways to assign names, either by an anonymous function being declared by a variable name or using a default parameter to specify the function name:

```js
// Variable assignment
let assignedName = function () {
	alert(assignedname.name);
};
assignedName(); // assignedName

// Default Parameters
function f(defaultValue = function () {}) {
	alert(defaultValue.name);
}
f(); // defaultValue
```

## **II. `length` property**

The `length` property returns the number of function parameters.

```js
function f(a, b, c) {
	alert(f.length);
}

f(); // 3
```

It excludes rest parameters and only includes parameters before the first default parameter:

```js
// Exclude rest parameters
function restParam(a, b, c...) {
    alert(restParam.length);
}
restParam(); // 2

// Parameters before the default value
function defaultParam(a, b, c = 1, d) {
	alert(defaultParam.length);
}
defaultParam(); // 2
```

- It is sometimes used for _type introspection_(polymorphism) in functions that operate on other functions.

For instance, we can change how the function is handled based on its length size.

```js
function polymorph(...handlers) {
	for (let handler of handlers) {
		if (handler.length == 0) {
			alert(handler.length);
		} else {
			alert(handler.length);
		}
	}
}

function f1() {
	return alert("Zero Parameters");
}
function f2() {
	return alert("Zero Parameters");
}
function f3(a, b) {
	return alert("Two Parameters");
}

polymorph(f1, f2, f3); // 0, 0, 2
```

## **III. Custom properties**

We can add our own custom properties inside the function object.

For instance, we can track the function call count:

```js
function callCount() {
	return callCount.counter++;
}

callCount.counter = 0;
callCount();
callCount();

alert(callCount.counter); // 2
```

### **Substituting closures**

Function properties can sometimes replace closures. The main difference between is that:

- In closures, only nested functions can modify the outer variable.
- External code can modify function properties since the scope is global.

```js
// Function properties are have a global scope
function makeCounter() {
	function counter() {
		return counter.count++;
	}

	counter.count = 0;
	return counter;
}

let counter = makeCounter();

alert(counter()); // 0
alert(counter()); // 1
alert(counter()); // 2

counter.count = 0;
alert(counter()); // 0
```

## **IV. Named Function Expressions - NFE**

NFE's are function expressions that have a name. They are still allowed to be called normally:

```js
let sayHi = function func(person) {
	alert(`Hi, ${person}`);
};

sayHi("John"); // Hi, John
```

There are two special things you can do by calling the named `func`:

1. You can reference `func` inside of the function.
2. It is only visible within the function.

```js
let sayHi = function func(person) {
	if (person) {
		alert(`Hi, ${person}`);
	} else {
		func("Guest");
	}
};

sayHi(); // Hi, Guest
func(); // Error - Not visible outside function
```

### **Local function scope**

A reason to use a named function expression is to restrict access to the function components. Since the function has a global scope, it can be changed by outer code.

When a function expression is declared from a global environment, it becomes visible to other code as its outer lexical environment.

As a result changing the referenced function will result in changes to the function itself:

```js
let sayHi = function (person) {
	if (person) {
		alert(`Hi, ${person}`);
	} else {
		sayHi("Guest");
	}
};

let sayHiReference = sayHi;
sayHiReference = null;

sayHiReference(); // sayHi becomes null
```

Its function local scope can be useful for recursive calls and such.
