# **Iteration: `keys`, `values`, `entries`**

The methods `keys()`, `values()`, and `entries()` are supported in:

- `Map`
- `Set`
- `Array`

`Objects` also support similar methods but the syntax is different.

## **I. `Object.keys()`, `values()`, `entries`**

Plain objects have the following methods:

- `Object.keys(obj)` - returns an _array_ of keys.
- `Object.values(obj)` - returns an _array_ of values.
- `Object.entries(obj)` - returns an _array_ of `[key, value]` pairs.

For instance, we can loop over the object's values by appending the method to loop over its values:

```js
let user = {
	name: "John",
	gender: "Male",
};

for (let value of Object.values(user)) {
	alert(value); // "John", "Male"
}
```

The difference between `Object` and the iterables `Array`, `Map` and `Set` are the following:

|              | Map                                      | Object                |
| ------------ | ---------------------------------------- | --------------------- |
| Call syntax  | `map.keys()`                             | `Object.keys(obj)`    |
| Return value | iterable object that contains key values | array of `key` values |

The reason why we need to use `Object.keys(obj)` instead of `obj.keys` is for flexibility. We can use our own implementations since objects are the base of all complex structures.

For example, we have an object called `data` that implements its own `data.values()` method, where `Object.values(data)` is still usable.

### **`Object.keys`, `values`, `entries` ignore symbolic properties**

If we want to return all symbolic keys, we should use:

- `Object.getOwnPropertySymbols` - returns an array containing symbolic keys.
- `Reflect.ownKeys(obj)` - returns an array containing all keys including symbolic keys.

## **II. Transforming objects**

Objects lack methods that exist for other types like `Array`, `Map` and `Set`. To apply the methods of the other objects, we should transform it.

For instance we would want to modify the values of the object:

1. Transform
   - `Object.entries` - returns an `Array` of key and value pairs.
2. Apply the array specific method
   - `map` - create an argument to keep the key and modify the value.
3. Revert
   - `Object.fromEntries` - transforms a key/value pair into an object.

```js
let prices = {
	Burger: 200,
	Fries: 100,
	Soda: 50,
};

let discount = Object.fromEntries(
	Object.entries(prices).map((food) => [food[0], food[1] / 2])
);

alert(discount.Soda);
```
