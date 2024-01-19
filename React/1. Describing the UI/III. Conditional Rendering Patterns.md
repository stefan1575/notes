# **Conditional Rendering**

Both regular JavaScript and curly braces `{}` can be used to conditionally render components.

## **I. Conditional Rendering Patterns**

### **Returning `null`**

A component will render nothing if it returns `null`. It is not recommended because you are not in control of the rendering behavior of your child component.

It is only used if the logic required to determine the parent component is tightly coupled with the rest of its child components.

### **Logical AND operator (`&&`)**

In JavaScript, the Logical AND (`&&`) operator returns either the first falsy operand it encounters or the last operand if all values are `true`.

In React, it is used when we want to render a part of a component when the condition is `true` or render nothing otherwise.

One caveat to account for is when testing an expression that doesn't evaluate to a boolean on the left side of the `&&` operator since it will be automatically be returned.

### **Ternary Operator (`? :`)**

The ternary operator (`? :`) is a compact way of writing a conditional expression. We can embed HTML as a truthy or falsy expression by enclosing the HTML in parenthesis `()`.

It is recommended to use tools like variables and functions to tidy up complex expressions.

### **Reassigning variables**

You can reassign variables that are defined with `let`. A common pattern is assigning a default value as a part of a component that changes when the conditions are met and then embedding the variable inside HTML with curly braces `{}`.

## **II. Rendering Lists**

To render lists of components, we can use JavaScript constructs that iterate over the target identifier.

In React, the array are usually transformed into lists using the `map()` method. If we only render list items with certain property keys/values, we should use the `filter()` method before transforming it with `map()`.

When passing an array containing `<li>` elements inside an `<ul>` tag using curly braces, React automatically iterates over the array and renders each `<li>` element individually.

```jsx
const listContent = [
	{
		name: "John",
		key: 0,
	},
	{
		name: "Tom",
		key: 1,
	},
	{
		name: "Tim",
		key: 2,
	},
];

// Renders a list of containing the names properties of listContent
export default function List() {
	const listItems = listContent.map((person) => <li key={key}>{person}</li>);

	return <ul>listItems</ul>;
}
```

One caveat to consider when using arrow functions is that multi-line statements should explicitly return a value.

### **`key` attribute**

Each `<li>` element should have a corresponding `key` attribute that uniquely identifies that item among its siblings which does not change. This lets React optimize rendering by efficiently determining which items of the list have been modified.

This is achieved by having a unique corresponding value for the `key` attribute of each element that we want to transform to a list.

A `key` value may be retrieved from database keys/IDs in a database, or by using an a function that creates a unique identifier if the generated data persists locally.

#### **`<Fragment>`**

When displaying several DOM nodes for each list item without using a container element, we should use the explicit `<Fragment>` since the shorthand fragment `<></>` won't let you pass a key.

It is a component that is imported from the React library.

```jsx
import { Fragment } from "react";
```
