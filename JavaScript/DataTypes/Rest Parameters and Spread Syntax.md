# **Rest and Spread Syntax**

A function will only support the number of parameters that it has been given. The proceeding arguments will be ignored:

```js
function sum(a, b) {
	return a + b;
}

alert(sum(1, 2, 3, 4)); // 3 and 4 are ignored
```

## **I. Rest parameters**

The rest syntax is a built-in function parameter that accepts an indefinite amount of arguments, where the parameters before it are still treated as variables.

It must be the last provided parameter in the function.

The syntax is:

```js
function doSomething(x, y, ...rest) {
    ...
}
```

### **Array of arguments**

The passed arguments become an array named by the provided `..rest` parameter. This means that it is compatible with array methods and must be treated as such:

```js
function sum(...numbers) {
	let sum = 0;
	for (let number of numbers) {
		sum += number;
	}

	return sum;
}

alert(sum(1, 2)); // 3
alert(sum(1, 2, 3)); // 6
```

### **`length` property**

The rest parameter is not counted towards the function's `length` property. It is counted separately with the number being the amount of arguments that is provided.

A functions length becomes the amount of parameters that can be used as a variable:

```js
function countLength(arg1, arg2, ...args) {
	return args.length;
}

alert(countLength.length); // 2

// for demonstration purposes
alert(countLength("arg1", "arg2", 1, 2, 3)); // 3
```

## **II. The `arguments` object**

The `arguments` object is a built-in array-like object that contains the arguments that are passed inside a function.

We can use `arguments.length` to check how many arguments that a function is called with:

```js
function checkArgs(x, y, z) {
	alert(arguments[0]);
	alert(arguments[1]);
	alert(arguments[2]);
	alert(arguments.length);
}

checkArgs(0, 1, 2); // 0, 1, 2, 3
```

### **Array-like object**

Since it is an array-like, it doesn't have built-in array methods, but unlike other array-likes, it's an iterable. Like other array-like objects, it can be converted to a real array:

```js
function checkArgs(x, y, z) {
	// alert indexes
	for (args in arguments) {
		alert(args);
	}

	return Array.from(arguments);
}

let array = checkArgs(1, 2, 3); // 0, 1, 2
alert(array); // [1, 2, 3]
```

### **Arrow functions**

The `arguments` object is a local variable available within all non-arrow functions. This means that if we try to access `arguments` in an arrow functions, they are taken from the function parameters directly:

```js
function showIndex(arg1, arg2, arg3) {
	let loopArgs = () => {
		for (let arg in arguments) {
			alert(arg);
		}
	};

	return loopArgs();
}

showIndex(1, 2, 3);
```

## **III. Spread Syntax**

The spread syntax `"..."` is used to unpack an iterable such as strings and arrays. It is normally used for functions that require a list of arguments.

There are three places that we can use it:

1. Function parameters
2. Array literals
3. Object literals

In both function parameters and array literals, only iterables are accepted, such as arrays and objects with the `Symbol.iterator` method:

```js
// function parameter
func(a, ...iterable, b);
// array property
let arr = [1, ...iterable, 3, 4, 5];
// object property
let obj = { key: "value", ...object, key2: "value2" };
```

### **A. Function Parameter**

Functions may expect a list of arguments where arrays can't be passed as arguments:

```js
// Math.max() - returns the greatest provided number
let numbers = [2, 3, 1];

alert(Math.max(2, 3, 1)); // 3
alert(Math.max(numbers)); // NaN
```

In function parameters, it unpacks an iterable into a list of arguments:

```js
alert(Math.max(...numbers)); // 2, 3, 1
```

Since they are unpacked we can pass multiple iterables for more arguments and combine them with normal values:

```js
let set1 = [1, 2, 3];
let set2 = [4, 5, 6];
alert(Math.max(1, ...set1, ...set2)); // 6
```

### **B. Object literals**

When attempting to merge multiple objects with the same keys, the last one will be overwritten. Spread syntax cannot be used to mutate an object:

```js
let obj1 = { name: "John", age: 20 }; // overwritten
let obj2 = { name: "Steve", age: 25 };
let obj3 = { ...obj1, ...obj2, gender: "male" };

alert(obj3); // name: "Steve", age: 25, gender: "male"
```

We should use `Object.assign()`, if we want to copy an object since it's properties can be mutated.

### **C. Array literals**

We can use spread syntax to copy and merge multiple arrays:

```js
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let merged = [...arr1, ...arr2];
alert(merged); // [1, 2, 3, 4, 5, 6]
```

It is not recommended to copy multidimensional arrays since they are _shallow copies_, where only the first level is copied, the rest are referenced:

```js
let arr1 = ["firstLayer", ["secondLayer", ["thirdLayer"]]];
let arr2 = [...arr1];
// arr2[0] = firstLayer
// arr2[1] = ["secondLayer", ["thirdLayer"]]

arr2[0] = 1;
arr2[1][0] = 2;
arr2[1][1][0] = 3;

// Only the first layer of arr1 isn't modified
alert(arr1); // ['firstLayer', [2, [3]]]
alert(arr2); // [1, [2, [3]]]
```
