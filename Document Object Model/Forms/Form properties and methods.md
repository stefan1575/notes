# **Form properties and methods**

## **I. Navigation properties**

Since `<form>` properties use the `name` attribute, they can be both retrieved by their index and their name value.

```js
let form = document.forms;
console.log(form.formName === document.forms[0]); // true
```

### **A. `document.forms`**

The `document.forms` property is a collection that contains all the `<form>` elements the document.

```html
<form name="form1"></form>
<form name="form2"></form>

<script>
	console.log(document.forms); // HTMLCollection(2)
</script>
```

### **B. Form children - `form.elements`**

The `elements` property is a collection listing all the elements of the specified `<form>` or `<fieldset>` property.

If there are multiple elements with the same name, then `form.name` becomes a collection:

```html
<form>
	<input type="radio" name="age" value="0-10" />
	<input type="radio" name="age" value="10-20" />
</form>

<script>
	console.log(document.forms[0].elements.age); // RadioNodeList(2)
</script>
```

### **C. Parent reference - `element.form`**

Individual form elements are able to reference its parent element.

```html
<form id="form">
	<input name="element" />
</form>

<script>
	let element = form.element;
	console.log(element.form); // form
</script>
```

## **II. Form element attributes**

### **`<input>`**

- `input.value` - the initial or current value where the behavior varies depending on the `type` attribute.
- `input.checked` - boolean value which describes the initial state for `checkbox` and `radio`.

### **`<select>`**

- `select.options` - collection of the `option` sub-elements
- `select.value` - value of the selected `option`
- `select.selectedIndex` - index value of the selected `option`

### **`<option>`**

- `option.selected` - returns `true` if the `option` is selected.
- `option.index` - index value of the specified `option`
- `option.text` - text inside the option

#### **`Option` constructor**

The syntax is:

```js
let option = new Option("Text", "value");
```

- `text` - text inside the option
- `value` - value of the selected `option`
- `defaultSelected` - if `true`, the `option` is visually selected upon page load
- `selected` - if `true`, the `option` is selected
