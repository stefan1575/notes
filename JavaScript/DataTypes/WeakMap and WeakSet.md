# **WeakMap and WeakSet**

The JavaScript garbage collection algorithm keeps a value in memory while it's "reachable" and can be potentially be used such as functions:

```js
let obj = { key: "value" };
obj = null;
// object is now garbage collected
```

As long as an `Object` is kept in a data structure (such as an Array or Map), it will still be reachable even if we overwrite all of the reference.

For instance, the reference still exists because we can call it in the data structure `map` using `map.keys()`:

```js
let obj = { key: "value" };
let map = new Map();
map.set(obj, "value");
obj = null;
alert(map.get(obj); // "value"
```

## **I. `WeakMap`**

`WeakMap` is a collection of key and value pairs where the keys must be objects.

The syntax is:

```js
let weakMap = new WeakMap();
```

Meanwhile, storing a reference in a `weakMap` data structure is garbage collected if there are no other references:

```js
let obj = { key: "value" };
let weakMap = new WeakMap();
map.set(obj, "value");
obj = null;
alert(weakMap.get(key)); // undefined
```

### **Methods**

Other methods aren't supported such as iteration so there is no way to loop over its keys or values.

`WeakMap` only has the following methods:

- `weakMap.get(key)`
- `weakMap.set(key, value)`
- `weakMap.delete(key)`
- `weakMap.has(key)`

### **Additional data storage**

For instance, we would want to store data that only exists while the object still alive, and the object "belongs" to another code.

An example would be storing the current visit count. When the visitor leaves, we would want to discard their visit count:

```js
// 📁 visitCount.js
let currentVisitors = new WeakMap();

function visitCounter(user) {
	let count = currentVisitors.get(user) || 0;
	currentVisitors.set(user, count + 1);
}
```

`WeakMap` stores the count while the reference is still active. Once the reference no longer exists, the data is discarded:

```js
// 📁 session.js
let user01 = { name: "Anon" };
visitCounter(user01);

user01 = null; // the current user leaves the session
```

### **Cache**

For instance, we would want to store a results of a function. Where the stored "cache" can be reused future calls on the same object.

```js
// 📁 cache.js
let cache = new WeakMap();

// calculate and remember the result
function process(obj) {
	if (!cache.has(obj)) {
		let result = /* calculate the result for */ obj;

		cache.set(obj, result);
	}

	return cache.get(obj);
}

// 📁 main.js
let obj = {
	/* some object */
};

let result1 = process(obj);
let result2 = process(obj);

// ...later, when the object is not needed any more:
obj = null;
```

## **II. `WeakSet`**

`WeakSet` is a collection of values where they must be objects. Otherwise they both work similarly.

### **Check if the value exists**

This is used for simple yes or no facts where a membership of an object may mean something.

For instance, we want to check if a certain is currently visiting the site:

```js
let currentVisitors = new Weakset();

let user01 = { name: "John" };
let user02 = { name: "Pete" };

currentVisitors.add(user01);

alert(currentVisitors(user01)); // John is visiting
alert(currentVisitors(user02)); // false

john = null;

alert(currentVisitors(user01)); // false
```
