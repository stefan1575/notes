# **`try...catch`**

The `try...catch` construct is composed of a `try`, `catch`, and a `finally` block.

1. The code inside the `try` block is executed.
2. If an error occurred the code in the `try` block stops and the `catch` block starts executing, otherwise the `catch` block is skipped.
3. The `finally` block starts executing.

```js
try {
	// executes first, stopped when an error occurs
} catch (error) {
	// executes if an error is occurs, otherwise skipped
} finally {
	// executes after try and catch
}
```

## **I. `catch` statement**

The `catch` statement is a block of code that executes when an exception is thrown inside the `try` block.

It must be present if we want to keep our script from running when an error occurs:

```js
try {
	nonexistentFunction(); // ignored
} catch {
	console.log("Error occurred, try block stopped"); // (1) Error occurred, try block stopped
}
console.log("Program end!"); // (2) Program end!
```

### **`Error` object**

Error objects are thrown when a runtime error occurs, it is passed as an argument to `catch`.

It has two main properties:

- `name` - name of the error
- `message` - details about the error

```js
try {
	nonexistentFunction();
} catch (Error) {
	console.log(Error.name); // ReferenceError
	console.log(Error.message); // nonexistentFunction is not defined
}
```

## **II. `throw` operator**

The `throw` operator generates a user-defined error by throwing an error object.

The syntax is:

```js
throw ErrorObject;
```

Just like other errors, the execution of the current block will stop and the `catch` block starts executing:

```js
function setAge(age) {
	if (isNaN(age)) {
		throw new Error("Age is not a number");
	}
	return age;
}
try {
	setAge("Twenty");
} catch (Error) {
	console.log(Error); // Error: Age is not a number
}
```

### **Rethrowing**

For instance, we are trying to parse a `JSON`
