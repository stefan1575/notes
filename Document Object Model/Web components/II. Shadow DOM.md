# **Shadow DOM**

The shadow DOM is a seperate DOM tree that is encapsulated and doesn't interfere with the document's behavior, structure, and styles.

Their encapsulation ensures that the components styles and behavior are isolated preventing conflicts to create reusable custom elements across different parts of the website.

## **I. Element: `attachShadow`**

The `attachShadow` method, attaches a shadow DOM tree to the specified element and returns a reference to its shadow root.

```js
const shadowDOM = elem.attachShadow(options);
```

- `options`
  - `mode`
    - `"open"` - allows access to the `shadowRoot` property.
    - `"closed"` - accessing the `shadowRoot` property returns `null`.

### **Element: `shadowRoot`**

The `shadowRoot` property represents the root node of the shadow DOM where the elements inside a shadow tree are accessed.

```js
const elem = document.createElement("p");

shadowDOM.shadowRoot.append(elem);
```

## **II. Slot**

A `slot` element is a placeholder where content will be inserted. It can only contain other slots or slottable elements.

```js
const menuTemplate = document.getElementById("menu-template");

class customMenu extends HTMLElement {
	connectedCallback() {
		this.attachShadow({ mode: "open" });

		let clonedTemplate = menuTemplate.content.cloneNode(true);
		this.shadowRoot.append(clonedTemplate);
	}
}

customElements.define("custom-menu", customMenu);
```

It is usually contained inside a `template`.

### **Named slots**

The `name` attribute in the `<slot>` element specifies an insertion point for content from another element with a matching `slot` attribute value.

```html
<template id="menu-template">
	<b><slot name="title"></slot></b>
	<i><slot name="item"></slot></i>
</template>

<custom-menu>
	<div slot="title">Fried Egg</div>
	<div slot="item">Egg</div>
	<div slot="item">Oil</div>
</custom-menu>
```

### **Default slots**

A `slot` without a `name` attribute specifies an insertion point for content from all elements without a `slot` attribute.

```html
<template id="menu-template">
	<div id="default-slots">
		<slot></slot>
	</div>
</template>

<custom-menu>
	<div>One</div>
	<div>Two</div>
</custom-menu>
```

### **Slot fallback**

If we put content inside a named or unnamed `slot` tag, it becomes the default fallback if the elements themselves are not present in the light DOM.

### **`slotchange`**

The `slotchange` event is triggered when the `slot` attribute is set or removed.

- `slot.assignedNodes/Elements` - returns an array of nodes/elements assigned to the specified slot.
  - if the `flatten` option is `true`, includes nested `slots` in the returned array.
- `node.assignedSlot` - returns the assigned `slot` element of the node.

## **III. Shadow DOM events**

Scripts that are defined outside of the shadow root have no access to the elements that reside in the shadow DOM.

When an event happens inside the shadow DOM, it gets retargeted to the target element's shadow host.

Since the `slot` element resides in the light DOM, it doesn't get retargeted.

### **Event: `composedPath`**

The `composedPath` method returns an array of HTML objects which represent the event's propagation path of the element starting from the target element.

If the shadow root is closed, the target element starts from the shadow host.

### **Event: `composed`**

The `composed` read-only property is a boolean value that indicates whether the event will propagate across the shadow DOM to the light DOM.
