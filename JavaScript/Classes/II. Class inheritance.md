# **Class inheritance**

## **I. `extends` keyword**

The `extends` keyword is used to create a subclass that inherits properties and methods from its parent class. The syntax is:

```js
class Child extends Parent {}
```

The created subclass can have its own set of properties and methods which the parent class cannot access:

```js
class User {
	constructor(name) {
		this.name = name;
	}

	sayHi() {
		alert(`Hi, ${this.name}`);
	}
}

class Admin extends User {
	sayBye() {
		alert(`Bye, ${this.name}`);
	}
}

let admin = new Admin("John");
admin.sayHi(); // Hi, John
admin.sayBye(); // Bye, John
```

### **`ParentClass` expression**

Aside from a `class`, any expression is allowed as long as it evaluates to a constructor function:

```js
function makeClass() {
	return class {
		sayHi() {
			alert(`Hi, user`);
		}
	};
}

class User extends makeClass() {}

let user = new User();
user.sayHi(); // Hi, user
```

## **II. `super`**

The `super` keyword is used to access the parent class's constructor or methods from the child class.

- `super.method(argN)` - Calls the immediate matching method in its implementation on the prototype chain starting from its parent class.
- `super(propN)` - When `super` is used inside a constructor, calls the parent constructor alongside the properties we want to inherit.

### **A. Overriding methods**

Using `super` inside a method calls the nearest method with the same name in the prototype chain in its own implementation.

Since the prototype chain traversal starts from its parent class, it allows us to modify or augment its behavior when used alongside the child's implementation:

```js
class User {
	constructor(name) {
		this.name = name;
	}
	sayHi() {
		alert(`Logging in, ${this.name}`);
	}
}

class Admin extends User {
	sayHi() {
		super.sayHi();
		alert(`Welcome admin, ${this.name}`);
	}
}

let admin = new Admin("John");

admin.sayHi();
// Logging in, John
// Welcome admin, John
```

### **B. Overriding constructors**

In constructor subclasses, `super` must be called before accessing any `this` properties.

If you don't intend to inherit the properties from the `ParentClass` constructor, use `super()` with no arguments.

```js
class ParentClass {
	constructor(name, age) {
		this.name = name;
		this.age = age;
	}

	sayHi() {
		alert();
	}
}

class ChildClass extends ParentClass {
	constructor(name, age, status) {
		super(name, age);
		this.status = status;
	}
}

let child = new ChildClass("John", 20, "single");
```

## **III. Derived constructor doesn't execute `new`**

### **Subclass constructors**

The derived constructor cannot create an empty object, it can only modify it. Trying to call a subclass constructor will throw an error:

```js
class Parent {}

class Child extends Parent {
	constructor() {
		this.name = name;
	}
}

let child = new Child(); // Error: Must call super in derived class before accessing 'this'
```

### **Parent class field override**

As a result, when parent class constructor, it will always use its own class field over any subclass:

```js
class Parent {
	name = "John";
	constructor() {
		alert(this.name);
	}
}

class Child extends Parent {
	name = "Steve";
}

new Parent(); // John
new Child(); // John
```

This is because of the order of which class fields are initialized when invoking a subclass with the `new` keyword:

1. Parent class fields are initialized with their default values.
2. Runtime skips the check since the subclass does not have a constructor.
3. An empty object with `super()` is automatically created.
4. The parent constructor is executed.
5. The subclass fields are initialized with their default values.

## **IV. `super` internals - `[[HomeObject]]`**

`[[HomeObject]]` is an internal property of a method that points to where the object is defined.

It is used by `super` when invoked with a method, which looks up the prototype chain of its original `[[HomeObject]]`.

### **Copying a method with super**

Since `super` looks up the prototype chain on its original object when invoking the method, it is bound to the object it was created in.

This means that the copied method will still look up its original prototype chain rather than its own `[[HomeObject]]`

```js
class Bird {
	type = "Bird";
	sayType() {
		alert(`super retrieved from bird - [[Prototype]]: ${this.type}`);
	}
}

class Penguin extends Bird {
	sayType() {
		super.sayType();
	}
}

class Fish {
	type = "Fish";
	sayType() {
		alert(`super retrieved from fish - [[Prototype]]: ${this.type}`);
	}
}

class Tuna extends Fish {
	sayType = new Penguin().sayType;
}

let tuna = new Tuna();
tuna.sayType(); // super retrieved from bird - [[Prototype]]: Fish
```
