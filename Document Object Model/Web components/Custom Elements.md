# **Custom HTML Elements**

Custom elements are HTML tags with custom behavior.

1. Autonomous custom elements - tags that are extended from `HTMLElement`.
2. Customized built-in elements - tags that are extended from the built-in elements that inherit from `HTMLElement`.

## **I. Custom element creation**

### **A. Extending HTML elements**

To create a custom element, we should create a class that extends the HTML element that we want to inherit features:

```js
class CustomElement extends HTMLElement {
	constructor() {
		// must be called to in a constructor
		super();
	}
}
```

The custom element class has optional methods:

- `connectedCallback` - invoked when appended to the DOM.
- `disconnectedCallback` - invoked when removed from the DOM.
- `attributeChangedCallback` - invoked when the observed attributes is added, changed, or removed.
  - `name` - the attribute that changed.
  - `oldValue` - the previous value of the changed attribute.
  - `newValue` - the new value of the changed attribute.
- `static get observedAttributes` - a returned array that contains the attributes to be observed by `attributeChangedCallback`.

## **B. customElements: `define`**

The `define` method creates a custom element.

```js
customElements.define(name, constructor, [options]);
```

- `name` - the tag name of the custom element which must contain a hyphen.
- `constructor` - the constructor of the custom element
- `options`
  - `extends` - the tag name of the built-in element to extend. It is needed since there are different tags that share the same DOM class.

### **Browser parsing before `define` call**

If the browser parses the custom element before the `define` method is called, it becomes `undefined`.

Undefined elements can be styled with `:not(:defined)`.

- `customElements.get(name)` - returns the constructor of the given element.
- `customElements.whenDefined(name)` - returns a promise that resolves without a value when the given element.

## **II. Rendering**

Rendering cannot be done inside the `constructor` method.

This is because the `constructor` is immediately called upon element creation and the element itself does not exist on the DOM. As a result it cannot access its own properties, methods, and attributes.

### **Dynamic Rendering**

To render content dynamically, we can create a separate helper function which will be initially used in `connectedCallback` and create a function that updates their values in `attributeChangedCallback`.

### **Element render order**

When the a custom element is created, we do not have access to its children in `connectedCallback`, we only have access to the element itself.

Although we can defer the execution as a macrotask to gain access to its children, if we have another nested custom element, both their `connectedCallback` methods will execute first before the macrotasks are queued.
