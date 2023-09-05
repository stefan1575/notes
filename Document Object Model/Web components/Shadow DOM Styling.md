# **Shadow DOM Styling**

## **I. Shadow host selectors**

The `:host` selector selects the shadow host (custom element) within its shadow DOM.

### **CSS Specificity**

The custom element itself resides in the light DOM which means that it can be referenced externally.

Since `:host` is a pseudo-class selector, it has a lower priority than element selectors.

### **Selectors**

- `:host(selector)` - selects the shadow host with the specified `selector`.
- `:host-context(ancestor)` - selects the shadow host if it is a child of the specified `ancestor` or has the selector itself.

## **II. Slotted element selectors**

Since slotted elements come from the light DOM, it does not use the styles local to the shadow host.

- `slot[name="value"]` - selects the specified `slot` HTML element within its shadow DOM.
  - Does not style the content that is passed into the `slot` HTML element from the light DOM.
- `::slotted(selector)` - selects a slotted element with the specified selector within its shadow DOM (inside a template).
  - This selector cannot select its children since it doesn't descend further into the slot.

## **III. Custom CSS properties**

Custom CSS properties are user-defined variables starting with `"--"` that contain specified property values.

```css
selector {
	--property-name: property-value;
}
```

The `var` is a CSS function that inserts the value of a custom property.

```css
selector {
	property: var;
}
```

Custom CSS properties are only available in the element that they are defined.

```css
/* Global CSS variable, available everywhere*/
:root {
	--main-color: #000000;
}

body {
	color: var(--main-color);
}
```

### **Defining global CSS properties**

Custom CSS properties can be referenced in both the light and shadow DOM.

You still have to define the CSS in you local styles however, only the value can be set globally.
