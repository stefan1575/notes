# **Attributes and Properties**

## **I. DOM Properties**

DOM nodes are regular JavaScript objects. They have built-in properties and we can add our own properties and methods as well:

```js
// Add new property
document.body.author = {
	name: "John Smith",
};

alert(document.body.author.name); // John Smith

// Add new method
document.body.authorName = function () {
	alert(this.author.name);
};

document.body.authorName(); // John Smith
```

### **DOM properties are typed**

DOM properties can have other data types aside from strings:

```html
<input type="checkbox" style="width: 4em; height: 4em" checked />

<script>
	let input = document.querySelector("input");

	alert(typeof input.type); // string
	alert(typeof input.checked); // boolean
	alert(typeof input.style); // object
</script>
```

## **II. HTML Attributes**

HTML attributes are additional information that configure or adjust the behavior of HTML tags which usually come in name/value pairs.

When a HTML attribute is created, a corresponding property gets created. This does not happen for non-standard attributes however:

```html
<body id="bodyId" nonStandardAttribute="value"></body>
<script>
	alert(document.body.id); // bodyId
	alert(document.body.nonStandardAttribute); // undefined
</script>
```

### **Element: retrieve attribute**

- `elem.hasAttribute('name')` - checks if attribute exists
- `elem.getAttribute('name')` - get attribute value
- `elem.setAttribute('name', 'value')` - set attribute name/value
- `elem.removeAttribute('name')` - remove attribute
- `elem.attributes` - returns a live collection containing attribute objects, with `name` and `value` properties.

These methods can be used with both standard and non-standard attributes:

```html
<body nonStd="value"></body>

<script>
	let nonStandard = document.body;
	nonStandard.setAttribute("newAtr", "newVal");

	// nonStd: "value", newAtr: "newVal"
	for (let attr of nonStandard.attributes) {
		alert(`${attr.name}: ${attr.value}`);
	}
</script>
```

### **Property-attribute synchronization**

When a _standard attribute_ changes, the corresponding property is auto-updated and vice versa:

```html
<body></body>

<script>
	let bodyTag = document.body;

	// Change attribute, auto-update property
	bodyTag.setAttribute("id", "value");
	alert(bodyTag.id); // value

	// Change property, auto-update attribute
	bodyTag.id = "newValue";
	alert(bodyTag.getAttribute("id"));
</script>
```

There are some elements that only synchronize from attribute to property and not vice-versa:

```html
<input />
<script>
	let input = document.querySelector("input");

	// Change attribute, auto-update property
	input.setAttribute("id", "value");
	alert(input.id); // value

	// Change property, attribute is not updated
	input.id = "newValue";
	alert(input.getAttribute("id")); // value
</script>
```

## **III. Custom data attributes - `dataset`**

One usecase of using non-standard attributes is to pass custom data from HTML to JavaScript, or to label HTML elements for JavaScript use.

### **HTMLElement: `dataset` property**

`dataset` is a read-only property that provides access to custom data attributes containing `data-*`. It can be written using the individual properties within the `dataset`.

Non-standard attributes are named using this property to avoid future conflicts as the standard evolves.

```html
<body>
	<div data-user="name"></div>
	<div data-user="age"></div>
</body>

<script>
	let user = {
		name: "John",
		age: 20,
	};

	// loop over <div>'s containing the [data-user] CSS attribute selector
	for (let div of document.querySelectorAll("div[data-user]")) {
		// using the same div, apply the corresponding attribute
		let property = div.getAttribute("data-user");
		div.innerHTML = user[property];
	}
</script>
```

Multi-word attributes become written as camelCase.
