# **React**

React is a library that lets you build user interfaces out of individual pieces called components.

It allows us to create self contained reusable UI components.

### **JSX**

React both `.js` and `.jsx` (JavaScript Syntax Extension) which allows us to write HTML-like code alongside JavaScript.

### **UI Primitives**

The UI Primitives that are available to React are dependent on the platform. The browser provides HTML markup as its UI primitives.

## **I. Components**

React components are JavaScript functions made out of markup.

Unlike HTML tags, they must start with a capital letter.

```jsx
function MyButton() {
	return <button>Button</button>;
}
```

### **A. Multiple Components**

If a component has more than one line, it is enclosed in parenthesis.

When returning multiple elements from a component, they must be wrapped with a single parent tag.

An empty tag can be used if we don't want to add an extra tag to your markup for grouping things without leaving a trace in the HTML tree.

#### **Nesting components**

To nest a React component, you should use the function as an HTML tag.

The main file's component is specified by prepending the `export default` keyword. This means that this component will rendered as the actual HTML in the browser.

```jsx
export default function MyApp() {
	return (
		<div>
			<h1>App Title</h1>
			<MyButton />
		</div>
	);
}
```

Components should always be defined at the top-level. If a component is defined inside another element, it affects performance and causes bugs.

### **B. Component Attributes**

Multi-word HTML and SVG attributes are written in camelCase.

The exception is `aria-*` and `data-*` attributes since they are written in dashes.

#### **Adding Styles**

In React, a component style is specified with the `className` attribute where the CSS rules are written in a separate CSS file. It works the same way as the `class` attribute in HTML.

```jsx
<h1 className="title" />
```

The `<link>` tag specifies where the CSS file is located. If you use a build tool or framework its better to consult its documentation to reference a CSS file.

## **II. Embedding JavaScript**

### **Passing Strings**

Curly braces `{}` allow us to embed the values of JavaScript variables within HTML tags. They also allow the use of complex expressions such as string concatenation as long as it evaluates to a single value.

There are two ways to use curly braces in JSX:

1. As a string inside a JSX tag.
2. As an attribute value of a JSX tag.

Using curly braces as a tag because it is expected to be a static identifier. To dynamically render a tag, use a JavaScript conditional statements instead.

### **Passing Objects**

Since the `style` attribute can also accept JavaScript object, it can be used when your styles depend on JavaScript variables.

Not to be confused with inline styles, they are written as camelCase instead of being denoted by hyphens.

## **III. Mutating Components**

In React, the component function should not mutate outside objects or variables to improve performance and avoid bugs due to side-effects.

### **Local Mutation**

It is acceptable for a component to change variables or objects that are created within the scope of the component while rendering.

```jsx
function ListItem({ number }) {
	return <li>Item #{number}</li>;
}
export default function ComponentList() {
	let list = [];

	for (let i = 1; i <= 10; i++) {
		cups.push(<ListItem key={i} number={i} />);
	}

	return <ul>{list}</ul>;
}
```

### **Event Handlers and `useEffect`**

Side-effects are acceptable inside Event Handlers and the `useEffect` hook when synchronizing with some external system.

## **IV. Performance**

In React, rendering starts at the root component tree and loops downwards to find the components that need updates.

This means that state change in a top-level component (below the root element) will trigger a re-render of all its child components.

On the other hand, leaf components (components with no child elements) doesn't trigger any additional re-renders since they have no child components making it a relatively inexpensive operation.

They are often frequently re-rendered because they are the ones that display dynamic data.
