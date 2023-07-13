# **Properties - `get` and `set`**

Accessor properties are set to `undefined` by default when creating an object property.

## **I. Accessor property flag**

An _accessor property_ is a property that has a getter and/or setter function to retrieve or set its value, rather than directly modifying as regular object property.

### **A. `get` method**

The `get` method retrieves the value of the accessor property by returning the value of the property as defined in the `get` function. No parameters can be set since you can access its value.

It's called when the property is accessed:

```js
let user = {
	firstName: "John",
	lastName: "Smith",
	get fullName() {
		return `${this.firstName} ${this.lastName}`;
	},
};

alert(user.fullName); // John Smith
```

### **B. `set` method**

The `set` method creates or modifies the value of the _accessor property_ by accepting the value as an argument and assigning it as defined in the `set` method. It only accepts one parameter which will be used as an argument.

It's called when a value is assigned to the accessor property:

```js
let user = {
	// firstName: "John" and lastName: "Smith",
	set fullName(name) {
		[this.firstName, this.lastName] = name.split(" ");
	},
};

user.fullName = "John Smith";
alert(user.firstName); // John
alert(user.lastName); // Smith
```

If we try to access the _accessor property_ without the getter method, it will return `undefined`.

## **II. Accessor descriptors**

The available property flags of accessor descriptors are different from data descriptors:

- `get` - Called when the property is read.
- `set` - Called when a value is assigned to the property.
- `enumerable` and `configurable` - Same as data properties.

When creating an accessor descriptor using `Object.defineProperty`,

Since accessor descriptors don't have a `value` and `writable` property flag, supplying them alongside with `get` or `set`, it will lead to an error:

```js
let user = {
	name: "John",
	surname: "Smith",
};

Object.defineProperty(user, "fullName", {
	get() {
		return `${this.name} ${this.surname}`;
	},
	value: "Tom Smith",
	writable: false,
}); // Error: Cannot specify value or writable attribute to accessors
```

## **III. Rejecting values**

Setters can be used to reject input if it doesn't meet a certain condition by using `return` without a value.

For instance, we can stop the accessor descriptor from accepting a value with a specified length:

```js
let user = {
	get name() {
		return this.name;
	},
	set name(value) {
		if (value.length > 2) {
			alert("Input at least 3 characters");
			return;
		}
		this.name = value;
	},
};

user.name = "John";
alert(user.name); // John

user.name = ""; // Input at least 3 characters
```
