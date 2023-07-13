# **Class**

A `class` is a constructor function that creates an object with initialized properties defined in `constructor` and class fields.

The methods that are defined within the class are stored in the class' `[[Prototype]]`, while the defined properties are initialized in the object itself.

The main difference between classes and constructor functions are:

1. They are not hoisted to the top of the scope.
2. All code inside the class construct is automatically in strict mode.

## **I. Syntax**

The syntax of creating a class with the properties to be initialized and methods to inherit are:

```js
class User {
	constructor(name, surname) {
		this.name = name;
		this.surname = surname;
	}

	method() {
		alert(`Your name is ${this.name} ${this.surname}`);
	}

	get name() {
		return this._name;
	}

	set name(value) {
		if (value.length < 2) {
			alert("Your name is too short");
			return;
		}
		this._name = value;
	}

	classField = "value";
}
```

- `constructor` - A method that creates an object with initialized properties which will be automatically invoked upon creating an object with a class. An empty constructor will be created if not provided.
- `method` - The methods that will be inherited by the object created within the class.
- `get/set` - The getters and setters to be inherited by the object.
- `classField` - The properties that will be added to the object.

To create an object by using a class, invoke it with the `new` keyword:

```js
let user = new User("John", "Smith");
user.method(); // Your name is John Smith
```

## **II. Class Expression**

Similar to functions, they can defined as an expressions:

```js
let User = class {
	method() {
		alert(`Your name is ${this.name} ${this.surname}`);
	}
};
```

Similar to function expressions, they can be named where its methods are only visible within the class:

```js
let User = class Expression {
	constructor(name, surname) {
		this.name = name;
		this.surname = surname;
	}
	method() {
		alert(`Your name is ${this.name} ${this.surname}`);
	}
};

new User("John", "Smith").method(); // Your name is John Smith

new Expression("John", "Smith").method(); // Reference Error: Expression is not defined
```

We can also create a class by returning it as a value:

```js
function makeClass() {
	// return the class
	return class {
		method() {
			alert(`Your name is ${this.name} ${this.surname}`);
		}
	};
}
```

## **III. Computed names**

Similar to objects, the name of the class methods can be computed at run-time using computed property names:

```js
class User {
	["say" + "Hi"]() {
		alert("Hi");
	}
}

new User().sayHi(); // Hi
```

## **IV. Class Fields**

Just like the `constructor` method, we can use complex expressions aside from being able to do simple key-value assignment.

```js
class User {
	name = prompt("What is your name?", "John");
}

let user = new User(); // What is your name?
alert(user.name); // John
```

### **Making bound methods with class fields**

If we use a class method outside of the context of its `[[Prototype]]`, it will lose access of its own properties.

Instead of binding the target method to the object, we can use class fields with arrow functions:

```js
class User {
	constructor(name) {
		this.name = name;
	}

	sayHi = () => {
		alert(`Hi, ${this.name}`);
	};
}

let user1 = new User("John");
setTimeout(user1.sayHi, 1000); // Hi John
```

Class fields allow us to create context-bound methods by initializing properties on the object with arrow functions which take its context from the surround scope.

The behavior of methods being context-bound useful for event handlers and callbacks since we can pass them anywhere without having to worry about context.
