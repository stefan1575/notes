# **DOM Tree**

In an HTML document, every HTML tag and its children is represented as an object.

### **Text nodes**

The text inside the tag or _text nodes_ are objects as well. This includes special characters such as newlines "`\n`" and spaces "`␣`".

There are two top level exceptions:

1. Spaces and newlines before `<head>`
2. Content outside of `<body>` is automatically moved inside body at the end.

## **I. Autocorrection**

The browser automatically corrects malformed HTML such as the automatic creation of closing tags and the missing `<html>`, `<head>`, and `<body>` tags.

### **`<tbody>`**

When creating tables, `<tbody>` is automatically created by the browser if not present.

If `<tbody>` is not present, it is created between `<thead>` and `<tfoot>`:

```html
<table>
	<table>
		<thead>
			<tr>
				First Header
			</tr>
			<tr>
				Second Header
			</tr>
		</thead>
		<!-- Automatically Created if not present -->
		<tbody>
			<tr>
				<td>Row #1 - Column #1</td>
				<td>Row #1 - Column #2</td>
			</tr>
			<tr>
				<td>Row #2 - Column #1</td>
				<td>Row #2 - Column #2</td>
			</tr>
		</tbody>
	</table>
</table>
```

## **II. Node types**

In JavaScript, there are 12 node types but in practice we only interact with 4 of them:

1. `document` - `<html>` or the main entry point to the DOM
2. Element nodes - The tags inside `<html>`
3. Text nodes - Any text inside the body which includes tabs and newlines
4. Comments - It won't be shown, but can be read with JavaScript from the DOM.
