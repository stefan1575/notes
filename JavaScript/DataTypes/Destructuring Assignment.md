# **Destructuring assignment**

Destructuring assignment is a special syntax that allows us to unpack values for arrays or properties from objects.

The array or property itself is copied but not modified.

## **I. Array Destructuring**

For instance, we can assign variables to individual array elements:

```js
let array = ["John", "Smith"];

// let firstName = array[0];
// let lastName = array[1];
let [firstName, lastName] = array;

alert(firstName); // "John"
alert(lastName); // "Smith"
```

### **Default values**

If you are trying to assign a variable to an absent value, it will return `undefined`.

To prevent this, we can provide default values to replace the missing one using `=`:

```js
let [accountType = "Guest", name = "Anonymous"] = ["Administrator"];
alert(accountType); // "Administrator"
alert(name); // "Anonymous"
```

Default values can also be complex expressions or functions. They will only be executed if the value is absent:

```js
let [accountType = prompt("Account type?"), name = prompt("Name?")] = [
	"Administrator",
];
alert(accountType); // "Administrator"
alert(name); // prompt()
```

### **Rest: "`...`"**

The rest syntax "`...rest`" is used at the end of a destructuring assignment.

It stores the remaining values into an array:

```js
let [name1, name2, ...otherNames] = ["John", "James", "Jack", "Steve"];
alert(otherNames); // ["Jack", "Steve"]
```

### **A. Array Methods**

This also works with _array returning_ methods:

```js
let [firstName, lastName] = "John Smith".split(" ");
alert(firstName); // "John"
alert(lastName); // "Smith"
```

### **B. Iterables**

Aside from arrays, destructuring also works with strings and sets since they are iterable:

```js
// string
let [One, Two, Three] = "123";
alert(One); // "1"

// Set
let [One, Two, Three] = new Set([1, 2, 3]);
alert(One); // 1
```

### **C. Assignables**

We can use "assignables" such as object properties:

```js
let fullname = {};
[fullname.firstName, fullname.lastName] = "John Smith".split("");

alert(fullname.firstName); // "John"
alert(fullname.lastName); // "Smith"
```

### **D. Looping over a key-value pair**

#### **`Object.entries()`**

If we need to loop over the key and value of an object, we can use `Object.entries()`:

```js
let fullname = {
  firstName: "John",
  lastName: "Smith",
};

for (let [key, value] of fullname) {
  alert(`${key}: ${value}); // firstName: "John"
}
```

#### **`Map`**

We can also loop over a `Map` since it's an iterable:

```js
let fullname = new Map();
fullname.set("firstName", "John");
fullname.set("lastName", "Smith");

for (let [key, value] of fullname) {
  alert(`${key}: ${value}); // firstName: "John"
}
```

### **E. Swapping variables**

Array destructuring can be used to create an temporary array and immediately destructure it in swapped order:

```js
let burger = "10$";
let fries = "20$";

[burger, fries] = [fries, burger];

alert(burger); // "20$"
alert(fries); // "10$"
```

## **II. Object destructuring**

The destructuring syntax also works with objects:

```js
let { key1, key2 } = {
	key1: "anyValue",
	key2: "anyValue",
};
```

When using destructuring to assign a variable to an object property:

- The variable must be the same as the key.
- An existing object must be assigned.
- The order of the assigned variables does not matter.

```js
let menu = {
	Burger: 100,
	Fries: 50,
	Soda: 20,
};

let { Fries, Soda, Burger } = menu;

// works the same as menu.Burger, ...
alert(Burger); // 100
alert(Fries); // 50
alert(Soda); // 20
```

### **Change variable name**

To change the variable name of the object, we should assign a value to the variable:

```js
let menu = {
	Burger: 100,
	Fries: 50,
	Soda: 20,
};

let { Burger: item1, Fries: item2, Soda: item3 } = menu;

alert(item1); // 100
alert(item2); // 50
alert(item3); // 20
```

### **Default values: Missing properties**

When assigning default values of properties that doesn't exist:

- We can assign variables for non-existing keys using "`=`".
- Default values can be expressions or function calls.
- We can combine default values with the variable name change "`:`".

```js
let menu = {
  Burger: 100,
};
let { Burger: prompt("") = 100, Fries: prompt("") = 50 } = menu;

alert(Fries);   // [1] prompt() [3] result of prompt
alert(Burger);  // [2] 100
```

### **Rest "`...`"**

The rest syntax stores the remaining properties to an object:

```js
let menu = {
	Burger: 100,
	Fries: 50,
	Soda: 20,
};

let { Burger, ...menuItems } = menu;

alert(menuItems.Fries); // 50
alert(menuItems.Soda); // 20
```

- JavaScript treats `{...}` as a code block, not inside another expression.

As a result, trying to use the object destructuring syntax without declaring it with `let` will lead to an error:

```js
let Burger, Fries, Soda;

// leads to an error
{Burger, Fries, Soda} = {Burger: 100, Fries: 50, Soda: 20};
```

We can wrap the expression in parenthesis `(...)` to show that it is not in a code block:

```js
let Burger, Fries, Soda;
({ Burger, Fries, Soda } = { Burger: 100, Fries: 50, Soda: 20 });

alert(Burger); // 100
```

## **III. Nested destructuring**

If we have an object or array that has nested elements, we can extract deeper portions by mirroring the depth of the nested object.

Aside from setting variables to the elements, we can use _default values_ to set missing values and _colon_ to set a different variable name:

```js
let menu = {
	Burger: {
		Small: 100,
		Medium: 200,
	},
	Fries: 50,
	Utensil: ["Spoon", "Fork", "Knife"],
	ExtraItems: true,
};

let {
	Burger: {
		Small,
		Medium,
		// Colon ":" to set/change name and "=" set value
		Large: Large = 300,
	},
	Fries,
	// Assign variable to array elements
	Utensil: [item1, item2, item3],
	...miscellaneous
} = menu;

alert(Large); // 300
alert(item1); // "Spoon"
alert(miscellaneous.ExtraItems); // true
```

- Note that the keys containing the nested objects do not have variables since we take their contents:
  ```js
  alert(Burger); // error: undefined
  alert(Utensil); // error: undefined
  ```
- To destructure an object that's inside an array, we need to set the variable name:

  ```js
  let nestedObject = {
  	key: [{ insideArray: "value" }],
  };

  ({
  	key: [{ insideArray: anyName }],
  } = nestedObject);

  alert(anyName);
  ```

## **IV. Destructuring in function parameters**

The syntax is the same for a destructuring assignment where we can use colons `":"` to name variables and equals `"="` to specify the default values:

```js
function destructuredParameter({
	incomingProperty: varName = defaultValue
});
```

When creating functions with many parameters that have optional ones, we can encounter a few problems.

The problem remembering the order of parameters especially when the number increases.

Another one is where the default values are overwritten by something undesirable like `undefined` being accepted if a function parameter has a default value:

```js
function showMenu(title = "Untitled", width = 200, height = 100, items = []) {
  ...
};

showMenu("My Menu", undefined, undefined, ["item1", "item2"]);
```

If we use the destructuring syntax when creating a function, we can pass an object as an argument inside the function call:

```js
let settings = {
	title: "My Menu",
	items: [item1, item2],
};
function showMenu({
	title = "Untitled",
	width = 200,
	height = 100,
	items = [],
}) {
	alert(`Title: ${title}`);
	alert(`Width: ${width}, Height: ${height}`);
	alert(`Items: ${items}`);
}

showMenu(settings); // "My Menu, 200, 100, item1, item2
```

### **Using every default value - `{}`**

If we want to use the default values in the function parameters, we should specify an empty object by giving the destructured object a default value of `{}`:

```js
function showMenu({title = "Untitled", width = 200, height = 100, items = []} = {}) {
	...
}
```

## **Summary**

- To destructure a nested object/array, we should mirror the targeted structure.
- To destructure a nested array inside an object, we need to set the variable name.

### **Array destructuring**

- When using the destructuring assignment array syntax, `rest` gathers the unassigned arrays into one array.
- The array destructuring syntax also works with other iterables such as strings.

```js
let [property1: varName = "defaultValue", ...rest] = arr;
```

### **Object destructuring**

- When creating a destructured object, the order of the assigned arguments do not matter.
- The keys containing the nested objects have no variable names since we take their contents.

```js
let { property1: varName = "defaultValue", ...rest } = obj;
```
