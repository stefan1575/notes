# **Extending built-in classes**

The `extends` keyword allows us to extend built-in classes with our own custom methods by using the class as its constructor:

```js
class ExtendedArray extends Array {
	isEmpty() {
		return console.log(this.length === 0);
	}
}

let arr = new ExtendedArray();
arr.isEmpty(); // true

arr.push("John");
arr.isEmpty(); // false
```

### **`Symbol.species`**

The `Symbol.species` accessor property overrides the default constructor when an instance is copied.

This means that when an instance is copied, it will use the specified constructor returned by `Symbol.species` rather than its original constructor.

So any copied instances won't be able to access the custom properties in the original constructor:

```js
class ExtendedArray extends Array {
	isEmpty() {
		return console.log(this.length === 0);
	}
	static get [Symbol.species]() {
		return Array;
	}
}

let arr = new ExtendedArray();
arr.isEmpty(); // true

arr.push("John");
arr.isEmpty(); // false

let copiedArr = JSON.parse(JSON.stringify(arr));
copiedArr.isEmpty(); // Error: isEmpty not found
```

## **Extended built-in classes do not inherit static methods**

Built-in objects have their own static methods. For instance, an Array has its own `Array.isArray()` static method.

Unlike regular classes which inherit static methods from its parents, built-in objects do not inherit static methods from each other.

This is because the static methods of the built-in objects are separate from the prototype chain.
