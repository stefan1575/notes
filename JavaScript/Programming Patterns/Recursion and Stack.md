# **Recursion and Stack**

Recursion is a programming pattern where we take a problem and solving it using simpler versions of the problem.

## **I. Recursive function**

In recursive functions, it will call itself.

To prevent infinite recursion, we must have a condition or a branch that stops the recursive call when the condition is met.

The syntax of a recursive function is:

```js
function recurse() {
	// condition of the base case(s)
	...
	// recursive function call with the recursive step
	recurse();
	...
}
```

1. Recursion works backwards where the base is calculated first
2. In the return statements:
   - the recursive function is calculated first before the return value.
   - the expression is calculated with the result of the recursive function.
   - the expression is finally returned.

## **II. Execution Context and Stack**

_Execution context_ contains details about the execution of a function, where the _control flow_ is now.

- **Call stack** - It keeps track of what function is currently being run and what functions are being called from that function.
- **Scope** - It contains details about the _current variables_ and the value of _`this`_ which is used in objects.

### **A. Initial function call**

In the beginning of a function call, an execution context is created with the passed arguments.

The function starts executing after the context is created, the current context changes to the applicable condition and is evaluated.

### **B. Nested function call**

To do a nested function call, JavaScript "remembers" the current context in a call stack.

1. The current function is paused.
1. The current context alongside that function is "remembered" on the call stack.
1. A new execution context is created for the new subcall.
1. After the subcall is finished, the previous content gets popped off the call stack.
1. The execution resumes immediately after the subcall.

### **C. Exit - Base case**

When the nested function reaches the base case, it is evaluated and popped off the call stack.

The previous one is goes back to the top of the call stack and is resumed, working its way up.

## **III. Solving recursive problems**

### **A. Find the simplest possible input**

This is also known as the _base case_ where we can only provide an explicit answer. Other solutions will revolve around the base case.

```js
// Write a recursive function where "n" is raised to the power of "x"
function recurse(x, n) {
	// base case
	if (x === 1) {
		return x;
	}
	// ...
}

alert(recurse(2, 1)); // 2
```

### **B. Find the relationship of an input between smaller and larger ones**

We should find the similarities and differences between the other inputs.

```js
// the similarities between them is they use the same multiplier for each function call
// the difference is amount of times they have been raised to the power of "n"
recurse(2, 3); // 8
recurse(2, 2); // 4
recurse(2, 4); // 16
```

### **C. Generalize the pattern**

The difference between each inputs should have an expression with a _recursive step_, where each result should be its simpler form until it reaches the base case.

The result should have a branch with the _base case_ and another branch with the recursive pattern:

```js
function recurse(x, n) {
	// base case
	if (n == 1) {
		return x;
	}
	// recusive pattern
	else {
		// n - 1: recusive step
		return x * recurse(x, n - 1);
	}
}

alert(recuse(2, 3)); // 8
```

## **IV. Recursive traversals**

Another use of recursion is traversing through objects with multiple nested objects.

Using loops may not be a proper solution since as the object expands, we have to create more subloops to traverse through a nested object.

For instance, we would want to gather the sum of each `salary` value.

1. We need to find the most trivial way to get the base (`salary` value).
2. Otherwise, we keep iterating over property values with nested objects until we encounter an array.

```js
let company = {
	sales: [{
		name: "John",
		salary: 1500,
	}, {
		name: "Tom"
		salary: 1500,
	}],

	production: {
		manufacturing: [{
			name: "James",
			salary: 1000,
		}, {
			name: "Jack",
			salary: 1000,
		}],
		logistics: [{
			name: "Peter",
			salary: 1200,
		}],
	}
};
```

In this example, the base of recursion is an immediately located array inside an object property (department). The other `salary` values are located within one or more layers (subdepartment).

For other values that don't immediately contain an array, we should make it be recursively called until it satisfy the condition. We should also make sure that we store each `reduce` call:

```js
function sumSalaries(dept) {
	// base case (case: 1)
	if (Array.isArray(dept) {
		return dept.reduce((prev, current) => prev + current.salary, 0)
	}
	// recursive call (case: 2)
	else {
		let sum = 0; // store the reduce call
		for (let subdept of Object.values(dept)) {
			sum += sumSalaries(subdept); // recursive call until object property is an array
		}
		return sum;
	}
}
```

## **V. Recursive Structures**

A recursive data structure is a data structure that is composed of simpler instances of itself.

- **There is no need to reorganize the data structure unlike arrays.**

They are preferable over using arrays since most structural modifications require mass-renumbering of their queues. The only exceptions would be methods that operate at the end of an array like `pop`/`push`. This is used if we need fast insertion or deletion.

A disadvantage of this however is that search operations are slow since the elements are referenced sequentially.

### **A. Linked list**

Linked lists is an object with a value and a `next` property referencing the proceeding element. It returns `null` if it's the last reference.

The syntax is:

```js
let link = {
	value: 1,
	next: {
		value: 2,
		next: {
			value: 3,
			next: {
				value: 4,
				next: null,
			},
		},
	},
};
```

Another syntax is naming property keys with `next` and assigning an object value:

```js
let link = { value: 1 };
link.next = { value: 2 };
link.next.next = { value: 3 };
link.next.next.next = { value: 4 };
link.next.next.next.next = null;
```

### **B. Multiple linked lists - Split and Join**

To use multiple nodes (references) to split the list, we create a node where we want to the chain to start and set that part `null`:

```js
let link = { value: 1 };
link.next = { value: 2 };
link.next.next = { value: 3 };
link.next.next.next = { value: 4 };
link.next.next.next = null;

// continue the linked list at the 3rd chain
let secondLink = link.next.next;
// remove the values starting from the 3rd chain
link.next.next = null;

// link = { value: 1, next: { value: 2, next: null } }
// secondLink = { value: 3, next: { value: 4, next: null } }
```

To join back the linked list, we supply the reference where we want to continue:

```js
link.next.next = secondLink;
```

### **C. Add and remove linked list items**

#### **Adding items**

To **prepend** or put an item from the start of the list, we set the value of `next` the same as the targeted object:

```js
let link = { value: 0, next: link };
```

To **add** an item in the middle, we set the `next` value the same as the target value:

```js
link.next.next = { value: 2.5, next: link.next.next };
```

#### **Removing items**

To **remove** an item, we change the target value to the `next` of the proceeding item:

```js
link.next = link.next.next;
```
