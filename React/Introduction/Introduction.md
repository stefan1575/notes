# **React**

React is a library that lets you build user interfaces out of individual pieces called components.

It allows us to create self contained reusable UI components.

### **JSX**

React both `.js` and `.jsx` (JavaScript Syntax Extension) which allows us to write HTML-like code alongside JavaScript.

## **I. Components**

React components are JavaScript functions made out of markup.

Unlike HTML tags, they must start with a capital letter.

```jsx
function MyButton() {
	return <button>Button</button>;
}
```

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

### **Adding Styles**

In React, a component style is specified with the `className` attribute where the CSS rules are written in a separate CSS file. It works the same way as the `class` attribute in HTML.

```jsx
<h1 className="title" />
```

The `<link>` tag specifies where the CSS file is located. If you use a build tool or framework its better to consult its documentation to reference a CSS file.

## **II. Embedding JavaScript**

### **A. Displaying Data**

Curly braces `{}` allow us to embed the values of JavaScript variables within HTML tags. They also allow the use of complex expressions such as string concatenation as long as it evaluates to a single value.

Since the `style` attribute can also accept JavaScript object, it can be used when your styles depend on JavaScript variables.

### **B. Conditional Rendering**

Both regular JavaScript and curly braces `{}` can be used to conditionally render components.

### **C. Rendering Lists**

To render lists of components, we can use JavaScript constructs that iterate over the target identifier.

When passing an array containing `<li>` elements inside an `<ul>` tag using curly braces, React automatically iterates over the array and renders each `<li>` element individually.

#### **`key` attribute**

Each `<li>` element should have a corresponding `key` attribute that uniquely identifies that item among its siblings. This lets React optimize rendering by efficiently determining which items of the list have been modified.

This is achieved by having a unique corresponding value for the `key` attribute of each element that we want to transform to a list.

## **III. Responding to Events**

To attach an event to a component, we should pass the event handler function as a value enclosed in curly braces to the event listener component attribute.

```jsx
function MyButton() {
	function clickHandler() {
		console.log("Clicked");
	}

	return <button onClick={clickHandler}>MyButton</button>;
}
```

## **IV. Storing Data**

In React, a hook is a built-in function that lets you add state or other React features to the component.

### **`useState`**

The `useState` function attaches

```jsx
import { usestate } from "react";
```

```jsx
const [state, setState] = useState(initialState);
```

- `initialState` - the initial value of the state variable.
- `state` - the variable name of the state variable
- `setState` - a function that updates the state variable. An event handler function
  is usually passed as its value.

#### **Sharing State Variable**

If the same component is rendered multiple times, each of them will get their own state.

To make multiple components share data and update together, you need to move the state to their parent component.
