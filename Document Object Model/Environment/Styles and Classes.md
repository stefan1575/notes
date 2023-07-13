# **Styles and Classes**

In JavaScript, you can manipulate CSS by using styles and classes.

Classes should be the prefered selector most of the time. Direct `style` property modifications are only acceptable for complex calculations.

## **I. Element: `className` and `classList`**

- `elem.className` - gets or sets the value of the `class` attribute.
- `elem.classList` - returns an array-like object containing a live collection of class attributes of `elem`
  - `add("class")` - adds `class` to `elem`
  - `remove("class")` - removes `class` to `elem`
  - `toggle("class")` - adds `class` if doesn't exist, otherwise removes it
  - `contains("class")` - returns `true` if `class` exists

`className` refers to the full class string of an element, while `classList` refers to the individual classes of that element.

`classList` is iterable using `for..of`:

```html
<body class="page content"></body>
<script>
	for (let name of document.body.classList) {
		alert(name); // page, content
	}
</script>
```

## **II. `style`**

Each `elem` has a `style` property which is an object that corresponds to what's written in the style attribute

In JavaScript, every dash in HTML corresponds to capitalization of the first letter:

```js
elem.style.backgroundColor; // background-color
elem.style.WebkitBorderRadius; // -webkit-border-radius
```

### **Removing CSS**

- `elem.style.display = "none"` - hides css, still exists
- `elem.style.display = ""` - removes all custom css
- `elem.style.removeProperty('styleProperty')` - removes css property

## **III. Reading CSS element - `getComputedStyle`**

- `getComputedStyle(element, [pseudoElem])` - returns an object that contains all the css properties of the specified element.
  - `element` - The full property name of the element. Shorthand elements are not accepted since it only returns one value.
  - `pseudoElem` - The optional pseudo-element in reference to `element`.

It is used because the `style` attribute can't read individual elements.

We can read `getComputedStyle` object instead:

```html
<style>
	body {
		background-color: red;
		font-size: 10px;
	}
</style>
<script>
	let computedStyle = getComputedStyle(document.body);

	alert(computedStyle.backgroundColor); // rgb(255, 0, 0)
	alert(computedStyle.fontSize); // 10px
</script>
```

### **Computed and Resolved values**

1. Computed style value - The value of the CSS property after all CSS rules and CSS inheritance have been applied. They can be expressed in both absolute and relative units.
2. Resolved style value - The final value applied to the element. The browser takes the computed value and makes them fixed and absolute which is returned by `getComputedStyle`.
