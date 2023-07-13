# **Accessing DOM elements**

## **I. Element nodes: `<html>`, `<head>`, and `<body>`**

- `document.documentElement` = `<html>`
- `document.head` = `<head>`
- `document.body` = `<body>`

### **`document.body`**

Note that `document.body` may not be available inside the `<head>` section because the `<body>` element doesn't exist at that point in the document's loading process.

As a result, trying to access document.body inside the `<head>` section may return `null`.

This does not mean that the property does not exist however.

## **II. Instance properties**

A `NodeList` is an array-like object containing a collection of nodes.

It's generally recommended to iterate a `NodeList` using `for..of` to avoid unexpected behavior.

This is because `for..in` loops over properties that aren't nodes which are undesirable.

#### **Navigation properties are read-only**

Navigation properties and the DOM collections that are accessed are read-only. This means that we cannot change

### **Children properties**

- `childNodes` - returns a live `NodeList` containing the direct children of a given element.

Its first child has an assigned index of `0` which includes element nodes, text, and comments.

For instance, since it returns an index, we can loop over its element nodes:

```js
// text, h1, text, script
for (let i = 0; i < document.body.childNodes.length; i++) {
	alert(document.body.childNodes[i]);
}
```

- `firstChild` - returns the first child of the node or `null`
- `lastChild` - returns the last child of the node or `null`
- `hasChildNodes()` - returns a boolean value whether or not the given value has child nodes.

### **Sibling properties**

- `parentNode` - Returns the parent of the current node
- `nextSibling` - Returns the next sibling of the current node or `null`
- `previousSibling` - Returns the previous sibling of the current node or `null`

### **Element-only navigation**

- `children` - returns a live `HTML Collection` which only contains element nodes.
- `firstElementChild`, `lastElementChild` - returns the first or last element node.
- `previousElementSibling`, `nextElementSibling` - returns the neighbor element nodes.
- `parentElement` - Returns the parent of the current node excluding the HTML node.

## **III. Tables**

### **`<table>`**

- `table.rows` - a live HTML collection of `<tr>` elements.
  - Also usable by `<thead>`, `<tfoot>`, `<tbody>`.
- `table.tBodies` - a live HTML collection of `<tbody>` elements.
- `table.caption` = `<caption>`
- `table.tHead` = `<thead>`
- `table.tFoot` = `<tfoot>`

### **`<tr>`**

- `tr.cells` - a live HTML collection of `<th>` and `<td>` elements(or data and header cells).
