# **Focus and Blur events**

An element recieves focus when it is either clicked, tabbed, or has an `autofocus` attribute.

- `focus` - triggered when an element recieves focus. Doesn't bubble
- `blur` - triggered when an element loses focus. Doesn't bubble

The `document.activeElement` property returns the current focused element.

## **I. Element: `focus/blur` methods**

The `focus` and `blur` are fired _after_ the elements recieve/loses focus, they do not work with the `preventDefault`.

To modify the default behavior, we need to manually set the focus/blur of the element.

- `elem.focus()` - sets the focus on the element.
- `elem.blur()` - unsets the focus on the element.

Since a focus loss can be code initiated, we should avoid causing it ourselves if we want to track user-initiated focus.

## **II. `tabindex` attribute**

The `tabindex` attribute allows an element to be focusable.

- `tabindex="0"` - the focus navigation order is determined by the order in the document source.
- `tabindex="-1"` - the element can only be focused by click or programatically.

## **III. Event delegation: `focusin/focusout`**

- `focusin` - triggered when an element recieves focus.
- `focusout` - triggered when an element loses focus.

Since `focusin/focusout` events bubble, they can be used for event delegation unlike the `focus/blur` methods.

```html
<form id="form">
	<input type="text" />
	<input type="text" />
</form>
<style>
	.focused {
		outline: 1px red solid;
	}
</style>
<script>
	form.addEventListener("focusin", () => form.classList.add("focused"));
	form.addEventListener("focusout", () => form.classList.remove("focused"));
</script>
```
