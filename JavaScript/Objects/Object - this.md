# **`this`**

The object method `this` specifies which object is being accessed. Its value is the "object before dot".

It is used to access the information stored inside the _same object_:

```js
let user = {
	name: "John",
	sayHi() {
		alert(this.name);
	},
};

user.sayHi(); // John
```

The value of `this` is evaluated on run-time. This means that you cannot re-assign a value of `"this"` during execution. It can be only used as a reference to an object:

```js
function setValue() {
    this = {
        name: "Tom"
    },
    return this.name;
}

setValue(); // Error: Cannot re-assign "this"
```

## **I. `this` context**

The value of `this` is evaulated on run-time. This means that its value is set depending on _how_ the function is called.

### **A. Object context**

The value of `this` is not where the function was created, but the object that is used to call the function.

For instance, we define a function in one object then copy it in another object. Since the other object has it's own function, the value of `this` will be taken where its called:

```js
let user = {
	name: "John",
};

let admin = {
	name: "Tom",
	getThis() {
		return this;
	},
};

user.getThis = admin.getThis;

alert(user.getThis()); // John
alert(admin.getThis()); // Tom
```

#### **Functions with `this`**

Since functions are objects, you can also use them to store properties. If the `this` value inside an object is not provided, it returns `undefined`.

> **Not recommended - referencing the outer variable**

We can also access an object property by directly referencing the outer variable.

One consequence of doing this is that when copying the object method with the directly referenced outer variable, it will access the wrong object:

```js
let user = {
	name: "John",
	sayHi() {
		alert(`Hi ${user.name}`);
	},
};
let admin = user.sayHi;

admin.sayHi(); // Hi John
```

### **B. Global Environment Context**

#### **Functions**

Functions and objects behave differently when it comes to the `this` binding in the global environment.

When a function is called in the global environment, the `this` becomes the global object or `window` in a browser. In strict mode it returns a `typeError` of `undefined`.

On the other hand, when an object method is called in the global environment, the `this` value is still bound to the object on which the method is defined.

For instance, even if the function has its own property, it will still retrieve and reference the property in the global environment:

```js
this.user = "Tim";
function sayName() {
	sayName.user = "Tom";
	alert(sayName.user); // Tom

	alert(this); // global object (window in a browser)
	alert(this.user); // Tim
}

sayName();
```

#### **Object methods reference**

Object methods are bound to the object when called in the context of the object.

If the method is assigned to a variable, the variable becomes a reference to the function itself and is no longer bound to the object.

When the function is called, the value of `this` is bound as a standalone function rather than as a method on the object:

```js
let user = {
	name: "John",
	sayHi() {
		return alert(this.name);
	},
};

let objToGlobal = user.sayHi;
objToGlobal(); // undefined
```

## **II. Arrow functions**

Arrow functions have no `this`. If we try to reference `this` in an arrow function, the value is retrieve from its surrounding scope, which is the lexical scope where the arrow function was defined:

```js
let user = {
	name: "John",
	sayHi() {
		// Taken from the declaration - "let user"
		let arrow = () => alert(this.name);
		arrow();
	},
};
user.sayHi(); // John

let arrow = () => console.log(this.name);
user.arrow = arrow;

// it is called as a method, but "this" refers to the global environment
user.arrow(); // Global environment has no "this"
```
