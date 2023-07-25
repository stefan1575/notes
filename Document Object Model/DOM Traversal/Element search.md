# **Element Search**

## **I. Retrieve element by `id`**

- `document.getElementById('id')` - returns the element with the matched `id` property.

## **II. Retrieve element by CSS selector**

### **`Document/Element` retrieval**

- `document.querySelectorAll('selector')` - returns a static `NodeList` containing elements that match the specified group of selectors.
- `document.querySelector('selector')` - returns the first element that matches the given selector.

### **`Element` retrieval**

- `elem.matches('selector')` - returns `true` if the `Element` would be selected by the specified selector.
- `elem.closest('selector')` - returns the nearest ancestor `Element` or itself that matches the specified selector.

### **`hidden` attribute**

The `hidden` attribute indicates that the browser should not render the contents of the element.

We can assign it in an HTML tag or as a DOM property in JavaScript:

```html
<div hidden>Hidden as an attribute</div>
<div id="elem">Hidden as a DOM property</div>

<script>
	elem.hidden = true;
</script>
```

## **III. Retrieve elements by Tag and Class**

- `elem.getElementsByTagName('tag')` - returns a live collection of elements with the given `tag` name or `*` to select all elements.
- `elem.getElementsByClassName('class')` - returns a live collection of elements with the given `class` name
