# **DOM and Resource Load Events**

- `DOMContentLoaded` - triggers when the DOM is fully loaded.
- `load`
  - In `document`, triggers when the DOM and external resources are fully loaded.
  - In `element`, triggers when the specified resource is fully loaded.
- `unload` - triggers when the user leaves the page
- `beforeunload` - triggers if the user initiated navigation away from the page or closes the window.
  - When called with `preventDefault`, it shows a confirmation dialog.

## **I. Script loading behavior**

The DOM is fully loaded after the whole HTML document has been parsed.

### **Inline/External scripts**

1. Inline scripts:
   - A `<script>` tag inside the HTML document.
   - Executed as soon as they are encountered during the parsing process, the DOM parsing is paused until the script finishes execution.
2. External scripts:
   - Referenced using the `src` attribute of the `<script>` tag
   - Loaded asynchronously by default, meaning that it doesn't pause parsing of the HTML document.

### **Inline script with CSS reference**

When an inline script contains a CSS reference, the script pauses execution until the CSS Object Model is fully loaded.

```html
<script>
	// value in the stylesheet or the default value
	alert(getComputedStyle(document.body).marginTop);
</script>
```

## **II. Document: `readyState`**

The `readyState` property describes the loading state of the `document`.

- `loading` - DOM is currently being parsed
- `interactive` - DOM is fully loaded
- `complete` - DOM and external resources are fully loaded

## **III. Script: `async` and `defer`**

The `<script>` tag has two attributes that can make the script load without blocking the HTML document from being parsed.

- `async` - the script executes as it completes loading regardless whether the DOM is fully loaded.
  - Load-first order, the order of execution is not guaranteed
- `defer` - the script executes after the DOM is fully loaded.
  - Executed in order that they appear in the HTML document, right before `DOMContentLoaded`.

## **IV. External resources: `load` and `error`**

- `load`
  - In `<script>`, triggers on successful load and execution.
  - In `<img>`, loads upon getting a `src` attribute.
- `error` - triggers on external resource load error.

## **V. Crossorigin Policy**

When a web page tries to load an external resource from a different origin, the browser prevents the request from being made.

The `crossorigin` attribute allows an external resource to be fetched.
