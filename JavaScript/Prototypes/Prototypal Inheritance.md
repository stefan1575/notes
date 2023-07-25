# **Prototypal inheritance**

Instead of copying an object's methods or properties, we can inherit its properties by creating a new object that uses the existing object as its prototype.

## **I. `[[Prototype]]`**

Objects have a hidden property called `[[Prototype]]` that references another object which serves as a fallback source of properties when they are not found on the object itself.

We can only assign an objects prototype to be either _a reference another object_ or `null`.

### **Prototype chain**

The prototype chain is a linear sequence of objects that starts from the object that is being _accessed_ then goes up until it reaches the top of the chain represented by `null`.

### **Accessing an objects property**

When you try to access a property that doesn't exist:

1. The JavaScript engine checks the object for the property.
2. If not found on the object, it checks the object's prototype.
3. If not found on the prototype, it checks the next prototype up the chain.
4. Repeats the process until it reaches the top of the chain.
5. If the property is not found, the engine returns undefined.

To summarize, the JavaScript engine for a property on the object itself, and if not found, it looks up the prototype chain until it finds the property or reaches the top.

This means that we can inherit properties of another object if we don't define it:

```js
let grandparent = {
	name: "John",
	sayHi() {
		alert(this.name);
	},
};

let parent = Object.create(grandparent, {
	name: {
		value: "Tom",
	},
});

let child = Object.create(parent, {
	name: {
		value: "Steve",
	},
});

grandparent.sayHi(); // John
parent.sayHi(); // Tom
child.sayHi(); // Steve
```

### **Limitations of a prototype**

1. An object cannot have multiple prototypes.

The prototype chain is linear, which means that the JavaScript engine can only follow a single path. If we try to assign multiple prototypes to an object, it will simply override the existing one.

2. It is not possible to assign cyclic prototype reference.

In the prototype chain, the final reference should be `null`. JavaScript will throw an error if we try to assign a prototype in a circle:

```js
Object.setPrototypeOf(grandparent, child); // error: cyclic __proto__ value
```

## **II. Setting an object's prototype**

### **A. `Object.create` - new objects**

The `Object.create` method allows us to create a new object while assigning it it's prototype and descriptors.

The syntax is:

```js
let newObject = Object.create(prototype, propertiesObject);
```

- `prototype` - The object's prototype to be set.
- `propertiesObject` - An object containing property keys as its own keys, with the value of each key being a descriptor object.

This allows us to create an exact copy of an object, including the non-enumerable properties and its prototype with `Object.getOwnPropertyDescriptors`:

```js
let clone = Object.create(obj, Object.getOwnPropertyDescriptors(obj));
```

### **B. `Object.setPrototypeOf` - existing objects**

The `ObjectsetPrototypeOf` method sets or modifies the existing object's prototype.

The syntax is:

```js
Object.setPrototypeOf(object, prototype);
```

- `object` - The target object to have its prototype set.
- `prototype` - The object's new prototype or `null`.

## **III. Accessor properties**

Prototypes are only looked up when a property is not found on an object.

Accessor properties are an exception since assignment is handled by the setter function, which means that writing a value with the same name as the accessor property is actually the same as calling a function.

```js
let admin = {
	name: "John",
	surname: "Smith",
	set fullName(name) {
		[this.name, this.surname] = name.split(" ");
	},
	get fullName() {
		return `${this.name} ${this.surname}`;
	},
};

let guest = Object.create(admin);

alert(guest.fullName); // John Smith

// assignment triggers the setter function
guest.fullName = "James Smith";
alert(guest.fullName); // James Smith
alert(admin.fullName); // John Smith - unaffected
```

## **IV. `this` modifies own state**

The value of `this` is determined by the object that it is called with, and is not affected by the prototype chain.

This means that when running inherited methods, it will modify its own state.

For instance, a new property is created when using when you explicitly assign a value on the property with the `this` keyword:

```js
// same objects as previous example
function getOwnProperty(object) {
	for (property in object) {
		if (object.hasOwnProperty(property)) {
			alert(`${property}: ${object[property]}`);
		}
	}
}

getOwnProperty(guest); // name: "James", surname: "Smith"
```

## **V. Iterating over prototypes**

### **`for..in` loop**

The `for..in` loop iterates over inherited properties.

```js
let bird = {
	hasWings: true,
};

let penguin = Object.create(bird, {
	flies: {
		value: false,
		enumerable: true,
	},
});

function getAllValues(object) {
	for (property in object) {
		alert(`${property}: ${object[property]}`);
	}
}

getAllValues(penguin); // flies: false, hasWings: true
```

### **`obj.hasOwnProperty(key)`**

The `obj.hasOwnProperty(key)` method returns `true` if an object has its own property.

It can be used to filter out inherited values:

```js
let bird = {
	hasWings: true,
};

let penguin = Object.create(bird, {
	flies: {
		value: false,
		enumerable: true,
	},
});

function getOwnProperty(object) {
	for (property in object) {
		if (object.hasOwnProperty(property)) {
			alert(`${property}: ${object[property]}`);
		}
	}
}

getOwnProperty(penguin); // flies: false
```
