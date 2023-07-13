# **DOM Class Hierarchy and Markup Methods**

## **I. DOM Node class hierarchy**

![Browser Environment](/assets/node-class-hierarchy.png)

- `EventTarget` - The root "abstract" class that provides event methods
- `Node` - An "abstract" class that provides core DOM tree functionality.
  - `Document` - Main entry point to the DOM
  - `Element` - Base class for DOM elements that provides element methods.
    - `HTMLElement` - Represents any HTML element
      - `HTMLBodyElement` = `<body>`
      - `HTMLAnchorElement` = `<a>`
      - `HTMLInputElement` = `<input>`
  - `CharacterData` - Represents a `Node` object that contains characters
    - `Text`
    - `Comment`

### **Tag name retrieval**

- `nodeName` - Retrieves the tag name of any nodes.
- `tagName` - Retrieves the tag name of `Element` nodes.

## **II. Markup Manipulation Methods**

### **A. Element: `innerHTML`**

The `innerHTML` property gets or sets the HTML markup contained in the element.

For instance, we can change any nodes contained in the element:

```html
<div>
	<p>Hello World</p>
</div>

<!-- Changed to <h1>Goodbye World</h1> -->
<script>
	document.querySelector("div").innerHTML = "<h1>Goodbye World</h1>";
</script>
```

Inserting `<script>` tags inside `innerHTML` do not execute however.

### **`innerHTML` overwrites the contained HTML markup**

A consequence of `innerHTML` overwriting the contained HTML markup is that the HTML content is that all assets and other resources will be reloaded.

This will affect existing interactions such as removal of the current selected element and higher reload times depending on the size of the content.

### **B. Element: `outerHTML`**

The `outerHTML` property gets or sets the full HTML of the element. This includes its tags and the contained HTML markup.

#### **`outerHTML` replaces the entire element**

When you use outerHTML to replace an element in the DOM, any variables that reference the old element will still point to the original element, even though it has been replaced from the DOM.

If we want to reference the new element, we should do it after the change:

```html
<h1>Hello World</h1>

<script>
	let header = document.querySelector("h1");

	header.outerHTML = "<h1>Goodbye World</h1>";
	console.log(header.outerHTML); // <h1>Hello World</h1>, original object is still referenced

	console.log(document.querySelector("h1").outerHTML); // <h1>Goodbye World</h1>, new element is referenced
</script>
```

### **C. Node: `nodeValue`/`data`**

The `nodeValue` property gets or sets the value of `comment` and `text` nodes. It will return `null` for other node types:

```html
<body>
	Text node
	<!-- Comment node -->
	<script>
		let textNode = document.body.firstChild;
		console.log(textNode.nodeValue); // Text node

		let commentNode = textNode.nextSibling;
		console.log(commentNode.nodeValue); // Comment node
	</script>
</body>
```

### **D. Node: `textContent`**

The `textContent` property provides access to the text content of the node and its descendants.

Unlike other properties such as `innerHTML` which shows human-readable elements, `textContent` will interpret it literally as text:

```html
<body>
	<div id="div1"></div>
	<div id="div2"></div>
</body>
<script>
	let name = prompt("", "<b>Bold text</b>");

	div1.textContent = name; // <b>Bold text</b>
	div2.innerHTML = name; // Bold text
</script>
```
