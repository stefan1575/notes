# **Static and Private**

## **I. `static` keyword**

The `static` keyword is used to define properties and methods on the class itself, inherited by their subclasses.

To use the `static` keyword, prepend it to the target property or method:

```js
class User {
	static thisUser() {
		alert(this === User);
	}
}

User.thisUser(); // true
```

It is the same as assigning a property to an object using the dot notation:

```js
class User {}
User.thisUser = function () {
	alert(this === User);
};
User.thisUser(); // true
```

## **II. Private - `#`**

Private properties and methods are only accessible within the class body. They cannot be inherited since they are not part of the prototype model.

They are defined by prefixing the `"#"` before the method or property name:

```js
class PrivateProperties {
	#privateField = "value";
	#privateMethod() {
		console.log("Running #privateMethod");
	}

	static #privateStaticField = "value";
	static #privateStaticMethod() {
		console.log("Running #privateStaticMethod");
	}
}
```

Since they are only accessible within the class body, the only way to invoke and use them is by reference:

```js
class User {
	#source = "User";
	locateSource() {
		console.log(`Object created by ${this.#source}`);
	}
}

let obj = new User();
obj.locateSource(); // Object created by User
```

### **Private properties - underscore `"_"`**

Another way to create a private property is to prefix a property with an `"_"` to denote that a property should not be accessed outside its class or object.

However unlike privates, they are inherited since it is not enforced at a language level.

## **III. Static and instance property scope**

Static properties and the instances of the class are scoped to their respective class and instances.

This means that either a static method cannot access instances and vice versa:

```js
class PrivateProperty {
	#privateMethod() {
		console.log("Running #privateMethod");
	}

	runPrivateMethod() {
		return this.#privateMethod();
	}

	// doesn't work
	static runPrivateMethod() {
		return this.#privateMethod();
	}
}

let instance = new PrivateProperty();
instance.runPrivateMethod(); // (1) Running #privateMethod

PrivateProperty.runPrivateMethod(); // (2) TypeError: Object must be an instance of class PrivateProperty
```
