# **Rendering**

In React, a render is triggered on initial render and when a component or its ancestor's state is updated.

When a component is rendered it must be a pure calculation. Strict mode calls each component twice to trigger possible side effects done by impure function.

## **I. Initial Render**

For a React app to start, an initial render has to be triggered by calling the `createRoot` method with the target DOM node, and then calling its `render` method to the component that we want to render.

```jsx
import { createRoot } from "react-dom/client";
import App from "./App.jsx";

const root = createRoot(document.getElementById("root"));
root.render(<App />);
```

## **II. Re-render**

A re-render is triggered by invoking `set` functions such as `useState` that updates a component's state.

This means that you have to change a component's state if you want React to change a DOM node in response to a triggered event instead of directly changing the HTML element.

### **State as a snapshot**

When a re-render is calculated, its props, event handlers, and local variables are calculated using its state values at the time of render.

Setting state only changes it for the next render.

When invoking multiple `set` function calls within a single re-render, the state is still re-calculated with the same value at the time of render similar to a snapshot.

```jsx
export default function Counter() {
	const [number, setNumber] = useState(0);

	// triggered event handler
	return (
		<>
			<div>{number}</div>
			<button
				onClick={() => {
					setNumber(number + 1);
					setNumber(number + 1);
					// outputs 0 - state value at the time of render
					alert(number);
				}}
			>
				Button
			</button>
		</>
	);
}
```

A state's value never changes within a render, even if the interaction is asynchronous. The asynchronous code is called with the value of the state before re-rendering happens.

### **Re-calculating state within the same re-render**

When a `set` function is called, the update is added to the _state queue_.

Passing a function with the state as parameter that updates the state value as an argument of the `set` function allows us re-calculate the state value within the same re-render instead of replacing the state value.

```jsx
import { useState } from "react";

export default function Counter() {
	const [number, setNumber] = useState(0);

	return (
		<>
			<div>{number}</div>
			<button
				onClick={() => {
					// replaces current state value
					setNumber(number + 1);
					// 2
					setNumber((number) => number + 1);
					// replaces current state value
					setNumber(1);
				}}
			>
				+3
			</button>
		</>
	);
}
```

## **III. Commit**

React commits changes to the DOM only when the resulting re-render changes DOM nodes.

This is because React compares the updated Virtual DOM with the actual DOM where only the elements affected by the updated changes are updated.

This means that only the element with the updated state will be re-rendered and commited leaving the other elements with no state changes intact.
