# **Map and Set**

## **I. Map**

Map is a collection of keyed items but unlike an `Object`:

- Allows for keys of any type, either _primitives_ or _objects_.
- Keys are not converted to strings.
- It remembers the insertion order similar to arrays.

For instance, numbers and boolean values can be keys:

```js
let map = new Map();

map.set("string", "str"); // string key
map.set(1, "num1"); // number key
map.set(true, "bool1"); // boolean key

alert(map.get("string")); // "str"
alert(map.get(1)); // "num1
alert(map.get(true)); // "bool1"
```

### **A. Methods**

To create a map, the syntax is:

```js
let map = new Map();
```

The methods and properties are:

- `map.set(key, value)` - Stores or updates an entry with the specified key and value.
- `map.get(key)` - Returns the value by the given key. Otherwise, returns `undefined` if key doesn't exist.
- `map.has(key)` - Returns `true` if the key exists.
- `map.delete(key)` - Removes the element by the specified key. Returns `true`, if the key existed and has been removed.
- `map.clear` - Removes every element in the `map`.
- `map.size` - Returns the number of elements in the `map`.

#### **Do not use `map[key]`**

If we do this, we will run into limitations of an object, such as string-only symbol keys. We should use `map` methods instead.

### **B. `Object` as a key**

In a `map`, we can store references of an object as a key:

```js
let userName = { name: "anon2000" };
let timesVisited = new Map();

timesVisited.set(userName, 100);
alert(timesVisited.get(userName)); // 100
```

When trying to use another object as a key, it will get converted into a string. Objects that get converted to strings will become `"[object Object]"`.

Therefore, when trying to use multiple objects as keys, it will just get overwritten as `"[object Object]"`:

```js
let john = {name: "John"};
let tom = {name: "Tom"};

let timesVisited = {};

timesVisited[john] = 100; // "[object Object]": 100

// overwrites previous value
timesVisited[tom] = 123;  // "[object Object]": 123

alert(timesVisited["object Object]"); // 123
```

- When comparing keys, strict equality `===` is used. `NaN` works as intended.

- When using `map.set`, it is chainable since it returns the map itself:
  ```js
  map.set("string", "str").set(1, "num1").set(true, "bool1");
  ```

## **II. Iteration over map**

### **Using `for..of`**

There are three methods if we want to loop over a map using the `for..of` loop:

- `map.keys()` - Returns an iterable object that contains `key`.
- `map.values()` - Returns an iterable object that contains `value`.
- `map.entries()`- Returns both the `[key, value]` pair, used by default in the `for..of` loop.

For instance, we chain the method to the map if we only want to get either the `key` or `value`. They will be iterated by the insertion order:

```js
let menu = new Map([
	["Burger", 100],
	["Fries", 50],
	["Soda", 20],
]);

// Iterate over keys
for (let food of menu.keys()) {
	alert(food); // Burger, Fries, Soda
}

// Iterate over values
for (let price of menu.values()) {
	alert(price); // 100, 50, 20
}

// Iterate over keys and values
for (let item of menu) {
	alert(item); // Burger, 100; Fries, 50; Soda, 20
}
```

### **Using `forEach`**

Similar to arrays, `Map` has a built-in `forEach` method:

```js
menu.forEach(value, key, map) => {
  // Food: Burger, Price: 100
  alert(`Food: ${key}, Price: ${value}`);
}
```

## **III. `Object.entries`**

`Object.entries` targets an object, returns an array of key/value pairs. It can be used to store an entire object inside the map:

```js
let obj = {
	name: "John",
	age: 20,
};

// stores - [ ["name", "John"], ["age", 20] ]
let map = new Map(Object.entries(obj));

alert(map.get("name")); // John
```

## **IV. `Object.fromEntries`**

`Object.fromEntries` targets an iterable, returns an object with key/value pairs. It can be used to create an object from a map.

For instance, the method `map.entries()` is an iterable which returns key/value pairs, it is suitable for this method:

```js
let map = new Map([
	["Burger", 100],
	["Fries", 50],
	["Soda", 20],
]);

// stores - Burger: 100, Fries: 50, Soda: 20
let obj = Object.fromEntries(map.entries());

alert(obj["Soda"]); // 20
```

Since `map` is also an interable that returns key/value pairs, we can omit `map.entries()` and use the target map instead:

```js
let obj = Object.fromEntries(map);
```

## **IV. `Set`**

`Set` is a collection of values that can only **occur once** which can either be _primitives_ or _object references_.

When creating a `Set`, we can provide an iterable, which usually an array. It copies the unique values and duplicate values are discarded.

### **Methods**

The syntax is:

```js
let set = new Set(iterable);
```

The methods and properties are:

- `set.add(value)` - adds a new value, returns the set itself.
- `set.delete(value)` - removes the value and returns `true` if the value existed.
- `set.has(value)` - returns `true` if the value existed.
- `set.clear()` - removes every element from the the target `set`.
- `set.size` - returns the element count.

The alternative to using `Set` is arrays, but the performance would be worse since the method may iterate over every element on the array. `Set` is better suited for uniqueness checks.

## **V. Iteration over set**

### **Using `for..of`**

We can either loop over a set using either `for..of` or `forEach`:

```js
let menuList = new Set(["Burger", "Fries", "Soda"]);

for (let item of menuList) {
	alert(item);
}
```

### **Using `forEach`**

As there are no keys in set, `value` is passed as an argument in both `value` and `key`:

```js
let menuList = new Set(["Burger", "Fries", "Soda"]);
menuList.forEach((value, key, set) => {
  alert(value);
}
```

The following methods are also supported for compatibility purposes. It may help to replace `Map` with `Set` in certain cases with ease:

- `set.keys()` - returns an iterable object.
- `set.values()` - the same as `set.keys()`, for compatibility purposes.
- `set.entries()` - returns an iterable object "`[value, value]`", for compatibility purposes.

## **Summary**

Both `Map` and `Set` remember the insertion order.

**`Map`** - is a collection of keyed items. Unlike an `Object`, it allows the key to be either _primitive_ or _objects_.

Methods and Properties:

- `new Map([iterable])` - syntax for creating a `Map`.
- `map.set(key, value)` - stores a key/value pair, returns the `Map` itself
- `map.get(key)` - returns the value by it's key or `undefined` if value doesn't exist.
- `map.has(key)` - returns `true` if key exists.
- `map.delete(key)` - removes a key/value pair, returns `true` if it existed.
- `map.clear()` - removes every key/value in `Map`.
- `map.size` - returns the number of elements.

**`Set`** - is a collection of unique values similar to an array.

Methods and Properties:

- `new Set(iterable)` - syntax for creating a `Set`
- `set.add(value)` - adds a value or does nothing if it's a duplicate, returns the `Set` itself.
- `set.delete(value)` - removes a value, returns `true` if it existed.
- `set.has(value)` - returns `true` if value exists.
- `set.clear()` - removes every value in `Set`
- `set.size` - returns the number of elements.
