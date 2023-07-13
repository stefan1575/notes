# **JSON Methods, to JSON**

JSON (JavaScript Object Notation) is a lightweight data format used for data exchange. In JavaScript, this is used to convert objects to a string.

For instance, we would want to convert to send an object's information as a string by looping over it. As the object gets more complex and has nested objects, we would need to implement their conversion.

## **I. Converting objects to JSON**

Javascript provides two methods to for JSON conversion:

- `JSON.stringify` - converts an object into JSON.
- `JSON.parse` - converts JSON back to an object.

For instance, we would want to convert an object into JSON. The resulting `string` is a _JSON-encoded_ object which is ready to be sent over for data exchange or used in data storage:

```js
let applicant = {
	name: "John",
	age: 20,
	isGraduated: true,
	languages: ["html", "css", "JavaScript"],
};

let json = JSON.stringify(applicant);

alert(typeof json); // string

alert(json); // {"name":"John","age":20,"isGraduated":true,"languages":["html","css","JavaScript"]}
```

### **A. JSON vs. Object literals**

The difference between a JSON-encoded object and an object literal:

- Strings are double quoted where there are no backticks ` `` ` or single quotes`''` in JSON. So `'John'` becomes `"John"`.
- Object properties are also double quoted, so `age: 30` becomes `"age":30`.

### **B. Supported data types**

JSON supports the following data types:

- Objects - `{...}`
- Arrays - `[...]`
- Primitives - strings, numbers, `true/false`, and `null`

```js
alert(JSON.stringify("str")); // "str" - double quoted
alert(JSON.stringify(1)); // 1
alert(JSON.stringify(true)); // true
alert(JSON.stringify(null)); // null
alert(JSON.stringify([1, 2, 3])); // [1, 2, 3]
```

### **C. JSON limitations**

#### **JavaScript-specific objects**

Since JSON is language independent, some JavaScript-specific object properties are skipped by `JSON.stringify`:

- Methods - functions that are properties of an object
- Symbolic properties
- Properties that store `undefined`

```js
let JSobjectProperty = {
	method() {
		alert("hello world");
	},
	[Symbol("id")]: "value",
	property: undefined,
};

alert(JSON.stringify(JSobjectProperty)); // {} - empty object
```

#### **Circular references**

Converting an object with references to each other will fail:

```js
let room = {
	capacity: 20,
};

let meetup = {
	title: "Conference",
	participants: [{ name: John }, { name: Tom }],
	place: room,
};

room.occupiedBy = meetup;

alert(JSON.stringify(room)); // error: circular reference
```

## **II. `JSON.stringify`**

Most of the time, `JSON.stringify` is only used with the first argument.

`JSON.stringify` has two optional arguments:

```js
let json = JSON.stringify(value, [...replacer], space);
```

- `value` - The value to be encoded.
- `replacer` - The specific properties to be encoded.
  1.  Either the array of properties.
  2.  or a mapping function `function(key, value)`
- `space`
  1.  If number, the amount of spaces used for formatting.
  2.  If string, it is used for indentation instead of the number of spaces.

### **A. Excluding and transforming properties - `replacer`**

The second argument `replacer` is used, if we need to remove specific properties to be encoded. There are two ways use it:

#### **1. Using an array of values**

Since `JSON.stringify` is applied to the whole object structure, object properties that are nested will be empty if not included in the array:

```js
let room = {
	capacity: 20,
};

let meetup = {
	title: "Conference",
	participants: [{ name: John }, { name: Tom }],
	place: room, // place references the object properties of room
};

room.occupiedBy = meetup;

alert(JSON.stringify(meetup, ["title", "participants"]));
// contains empty objects - {"title":"Conference,"participants:[{},{}]}
```

If we need to filter out circular references using an array, we should list every property except the ones that causes the circular reference:

```js
// now works - room.occupiedBy is omitted
alert(
	JSON.stringify(meetup, ["title", "participants", "place", "name", "capacity"])
);
```

#### **2. Using `function(key, value)`**

This is used since we may have a long list of properties.

We can also use `function(key, value)` to remove specific properties by assigning them unacceptable values like `undefined`.

For instance, if we loop over a circular value, we should set it's value to `undefined`:

```js
let room = {
	participants: 20,
};

let meetup = {
	title: "Conference",
	participants: [{ name: John }, { name: Tom }],
	place: room,
};

room.occupiedBy = meetup;

alert(
	JSON.stringify(meetup, function replacer(key, value) {
		return key === "occupiedBy" ? undefined : value;
	})
);
```

The `replacer` function loops over every key value pair, including nested items. It is applied recursively where:

1. If the current encountered value is an object, it loops over its elements until it encounters a non object value.
2. When the iteration is done for the current nested object and its adjacent properties, it iterates its way up to the next value.

```js
1.) :             [object Object] // "": meetup
2.) title:        Conference
// Array containing nested object
3.) participants: [object Object],[object Object]
	I.)		0:            [object Object]
		II.) name:         John
	III.)	1:            [object Object]
		IV.) name:         Tom
8.) place:        [object Object] // place: room
9.) participants: 20
10.) occupiedBy:  [object Object] // circular reference
```

#### **First call of reducer**

The first call of the function is made using a wrapper object "`{"": object}`", which is an empty key where the value is the targeted object.

This makes `replacer` able to analyze and replace/skip the entire object.

If we want to skip values that return `undefined`, we would need give the initial call `""` an exception since it also returns `undefined`.

### **B. Formatting - `space`**

The third argument `space` can be either be a number or a string up to `10` spaces or characters:

- If `Number`, it is the amount of spaces used for indentation.
- If `String`, it will be inserted before every indentation.

```js
let applicant = {
	name: "John",
	age: 20,
	status: {
		isEmployed: true,
		isMarried: false,
	},
};

alert(JSON.stringify(applicant, null, "===>"));
/* String is inserted as an indentation
{
===>"name": "John",
===>"age": 20,
===>"status": {
===>===>"isEmployed": true,
===>===>"isMarried": false
===>}
}
*/
```

#### **Using `space` while calling all properties**

If the `replacer` is anything other than a function or array, all properties are JSON-encoded.

For instance, if we want to set the number of spaces we should either use `null` or any other type of value that isn't a function or array:

```js
let user = {
	name: "John",
	age: 20,
};

alert(JSON.stringify(user, null, 2));
/* Indent to 2 spaces
{
  "name": "John",
  "age": 20
}
*/
```

## **III. `toJSON()`**

### **A. Built-in `toJSON`**

Some objects such as dates have a built-in `toJSON` method. They are automatically converted to string when JSON-encoded:

```js
let room = {
	participants: 20,
};

let meetup = {
	title: "Conference",
	date: new Date(Date.UTC(2000, 0)),
	room,
};

alert(JSON.stringify(meetup, null, 2));
/*
{
  "title": "Conference",
  "date": "2000-01-01T00:00:00.000Z", 
  "room": {
    "participants": 20
  }
}
*/
```

### **B. Custom `toJSON`**

If an object contains a `toJSON` function, it will be automatically called in `JSON.stringify()`.

For instance, only a value is returned. The nested encoded object is assigned the return value:

```js
let room = {
	number: 20,
	toJSON() {
		return this.number;
	},
};

let meetup = {
	title: "Conference",
	room,
};

alert(JSON.stringify(room)); // 20
alert(JSON.stringify(meetup)); // {"title":"Conference,"room":20}
```

## **IV. `JSON.parse`**

The method `JSON.parse` is used to decode a JSON-string.

The syntax is:

```js
let value = JSON.parse(str, [reviver]);
```

- `str` - The JSON-encoded string to parse.
- `reviver`
  - Called using `function(key, value)`.
  - Specifies which values are converted back or provides a transformation on the object before it's returned.

### **A. `str`**

For instance, we can parse strings with the same format as a JSON-encoded object:

```js
let numbers = "[1, 2, 3, 4, 5]"; // String
numbers = JSON.parse(numbers);

alert(Array.isArray(numbers)); // Array
alert(numbers); // [1, 2, 3, 4, 5]
```

### **B. `reviver`**

There are values that need to be converted since they are "stringified".

For instance, we have a `Date` object that was JSON-encoded. We need to _deserialize_ it or convert it back to its original data structure:

```js
let meetupJSON = '{"title":"conference","date":"2000-01-01T00:00:00.000Z"}';

let meetup = JSON.parse(meetupJSON, function (key, value) {
	if (key === "date") {
		return (value = new Date(value));
	}
	return value;
});

alert(meetup.date);
```
