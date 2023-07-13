# **Property flags and the descriptor object**

## **I. Property flags**

Property flags are a set of values that can be used to configure the behavior of a property.

When creating an object using the default syntax, the _data properties_ are set to `true`.

### **Data Property flag**

The property flags are of a data descriptor used to restrict the behavior of object properties that have a `value` characteristic.

- `writable` - If `false`, the property's _value_ cannot be changed. It will be in a read-only state.
- `enumerable` - If `false`, the property won't appear in an enumeration of a loop.
- `configurable` - If `false`, the property cannot be deleted and the property flags cannot be modified. One exception is modifying `writable` to `false`.

## **II. Descriptor object query**

The _property descriptor object_ contains the value and property flags of an object property.

When mutating the returned object, it doesn't have any effect on the original property's configuration.

### **A. `Object.getOwnPropertyDescriptor`**

`Object.getOwnPropertyDescriptor` returns the property descriptors of the specified property on a given object.

The syntax is:

```js
Object.getOwnPropertyDescriptor(object, property);
```

### **B. `Object.getOwnPropertyDescriptors`**

`Object.getOwnPropertyDescriptors` returns all of the property descriptors of the given object.

The syntax is:

```js
Object.getOwnPropertyDescriptors(object);
```

## **III. Modifying property flags**

When defining or modifying property flags a reference to the original object is returned.

### **A. `Object.defineProperty`**

The `Object.defineProperty` method can define new properties or modify property flags of existing properties on an object.

Unlike the regular syntax of creating a property, it allows us specify property flags in addition to the `value`.

The syntax is:

```js
Object.defineProperty(object, propertyName, descriptorObj);
```

- `object`, `propertyName` - The target object and property to be modified.
- `descriptorObj` - An object containing the modified property flags.

For new properties, we need to explicitly specify what values are `true` since the properties created using this method default to `false`:

```js
let obj = {};
Object.defineProperty(obj, "name", {
	value: "John",
	writable: true,
	enumerable: true,
	configurable: true,
});

// Make the property read only
Object.defineProperty(obj, "name", {
	writable: false,
});
```

### **B. `Object.defineProperties`**

The other method `Object.defineProperties` allows us to define new or modify multiple properties.

The syntax is:

```js
Object.defineProperties(object, descriptorObj);
```

- `descriptorObj` - An object containing property keys as its own keys, with the `value` of each key being a _descriptor object_ containing the modified property flags.

For instance,

```js
let obj = {};

Object.defineProperties(obj, {
	name: {
		value: "John",
		writable: false,
		enumerable: true,
		configurable: false,
	},
	age: {
		value: 20,
		writable: true,
		enumerable: true,
		configurable: true,
	},
});
```

#### **Cloning objects with property flags**

If an object is cloned using a traditional loop to iterate every property to another object, hidden properties aren't copied such as symbols and properties with non-enumerable flags.

```js
function cloneProperties(original, clone) {
	// copy each key
	for (let key in obj) {
		clone[key] = original[key];
	}
}
```

To clone an object while retaining its property flags and other hidden properties we should:

1. Create a new object while defining its property flags and other hidden attributes using `Object.defineProperties`, where we use an empty object as our target object to be modified.
2. Return the `object` alongside its descriptors to be cloned using `Object.getOwnPropertyDescriptors(obj)`.

```js
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
```

## **IV. Sealing an entire object**

There are methods that allow us to restrict the behavior of an object. These methods are rarely used in practice.

- `Object.preventsExtension(obj)`
  - Forbids the addition of new properties.
- `Object.seal(obj)`
  - Forbids the addition of new properties.
  - `configurable` set to `false`
- `Object.freeze(obj)`
  - Forbids the addition of new properties.
  - `writable` and `configurable` set to `false`

We can also test if all the properties of an object

- `Object.isExtensible` - Checks if new properties can be added.
- `Object.isSealed` - Checks if `isExtensible` and `configurable`.
- `Object.isFrozen` - Checks if `isExtensible`, `writable`, and `configurable`.
