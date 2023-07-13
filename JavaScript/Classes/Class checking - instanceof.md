# **Class checking - `instanceof`**

## **I. `instanceof`**

The `instanceof` operator checks if an object is an instance of a particular constructor or inherits from one in the constructor's `prototype` property.

The syntax is:

```js
obj instanceof constructor;
```

We can check if an object prototypically inherits from other built-in constructors:

```js
let arr = new Array();
console.log(arr instanceof Array); // true
console.log(arr instanceof Object); // true
```

### **Changing `prototype`**

Although the class' `prototype` property is not writable, you can change one in a regular constructor.

Since the instances created by the constructor function retains its old `prototype` property, if it's changed after the object is created, the `instanceof` operator will perform a search and see that it does not match.

On the other hand, new instances created after the `prototype` change will be the same:

```js
function Construct() {}
let obj1 = new Construct();

Construct.prototype = {};
let obj2 = new Construct();

console.log(obj1 instanceof Construct); // false
console.log(obj2 instanceof Construct); // true
```

### **`Symbol.hasInstance`**

The `Symbol.hasInstance` static method overrides the default behavior of `instanceof`.

The syntax is:

```js
static [Symbol.hasInstance](instance) {
    // returns a boolean value
}
```

## **II. `Object.prototype.toString`**

The `toString` method converts an object into a primitive value. The result is prefixed with an additional `"object"`.

### **`Symbol.toStringTag`**

The `Symbol.toStringTag` property overrides the default output of the `toString` property:

```js
let obj = {
	[Symbol.toStringTag]: "Custom Object",
};
console.log(obj.toString.call(obj)); // Object Custom Object
```
