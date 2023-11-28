# **Function Prototypes**

As we know, the constructor function creates a new object by returning the value of `this` as an new object with preset values.

## **I. Constructor functions - `F.prototype`**

The constructor's `prototype` property is an object that contains the defined methods, the `constructor` method, and its own `[[Prototype]]`.

It serves as the initial `[[Prototype]]` to all instances created using the `new` keyword.

We can modify the constructor function's built-in `[[Prototype]]` by setting its `prototype` property to another object:

```js
let animal = {
	eats: true,
};

function Bird(specie) {
	this.specie = specie;
	this.hasWings = true;
}

Bird.prototype = animal;

let penguin = new Bird("penguin");

alert(penguin.eats); // true
```

### **Replacing a constructor's `[[Prototype]]`**

Modifying a property of the `prototype` object of a constructor function will be visible on all objects created by that constructor:

```js
function F() {}
F.prototype.x = 5;

let oldObject = new F();
F.prototype.y = 10;

// Changes are still kept
alert(oldObject.x); // 5
alert(oldObject.y); // 10
```

On the other hand, replacing the `prototype` object with a new one will affect only future objects, existing objects will keep their original `[[Prototype]]`:

```js
function F() {}
F.prototype.x = 5;
let oldObject = new F();

F.prototype = { y: 10 };
let newObject = new F();

// old reference is used
alert(oldObject.x); // 5
alert(oldObject.y); // undefined

// reference is replaced
alert(newObject.x); // undefined
alert(newObject.y); // 10
```

## **II. `constructor`**

### **Default Function Prototype - `constructor`**

The default `prototype` of every function is an object containing the `constructor` property which points to the function itself.

```js
function Bird(specie) {
	// Bird.prototype = { constructor: [Function: Bird] }
	this.specie = specie;
	this.hasWings = true;
}
alert(Bird.prototype.constructor === Bird); // true
```

The default `prototype` will be shared across all objects created by the same constructor function:

```js
// same constructor as above
let penguin = new Bird("penguin");
let flamingo = new Bird("flamingo");

alert(penguin instanceof Bird); // true
alert(flamingo instanceof Bird); // true
```

### **Retaining `constructor`**

Assigning a prototype to the function will replace the `constructor` property with the inherited values itself. This means that we can accidentally overwrite its value.

```js
let animal = {
	eats: true,
};
Bird.prototype = animal; // Bird.prototype = { eats: true }

alert(Bird.prototype.constructor == Bird); // false
```

To retain the default `prototype` values we can either add/remove properties on top of the existing prototype:

```js
// instead of Bird.prototype = animal
Bird.prototype.eats = true;

alert(Bird.prototype); // [Function: Bird]
```

### **New object - Prototype `constructor` property**

To create an object using the `constructor` property, we need to first create an object with the constructor function, then reference it to access the `constructor` property with the `new` keyword:

```js
function Bird(specie) {
	this.specie = specie;
	this.hasWings = true;
}

let penguin = new Bird("penguin");
let flamingo = new penguin.constructor("flamingo");

alert(
	Object.getPrototypeOf(penguin.constructor) ===
		Object.getPrototypeOf(flamingo.constructor)
); // true - they both have the same reference
```
