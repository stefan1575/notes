# **State Memory Management**

A component's state is tied to its location in the render tree. This is how React isolates state between components.

### **Render Tree**

A render tree is an internal representation of the UI structure composed of React components. It is built during the rendering process and represents the nested relationship between components for a single render pass.

## **I. Resetting State**

When React removes a component, it destroys its state.

Rendering the same component in place of the old one effectively resets the state.

## **III. Rendering in the same Position**

As a rule of thumb, if we want to preserve the state of a component between re-renders, the structure of the render tree has too match between each re-renders.

### **Same Component**

Conditionally rendering the _same component_ in the same position at the render tree preserves its state.

### **Different Component**

If the conditionally rendered component is different, the previous component alongside its state gets deleted. When it is rendered again, the component itself allongside its state is created from scratch.

Since deleting a component includes its child components, the state all of its children will get deleted as well.

## **IV. Preserving the same Position**

When a `key` is specified in a component, it retains its position in the render tree even if it is removed.

It lets you specify its position in the render tree rather than the default behavior of its location in the HTML tree.

This is useful if treat the same component as two different ones to reset state.

```jsx
import { useState } from "react";

export default function App() {
	const [isPersonA, setIsPersonA] = useState(true);

	return (
		<>
			{isPersonA ? (
				<Counter key="PersonA" person="Person A" />
			) : (
				<Counter key="PersonB" person="Person B" />
			)}
			<button onClick={() => setIsPersonA(!isPersonA)}>Change Person</button>
		</>
	);
}

function Counter({ person }) {
	const [counter, setCounter] = useState(0);
	return (
		<div>
			<h1>
				{person}'s Counter: {counter}{" "}
				<button onClick={() => setCounter(counter + 1)}>+1</button>
			</h1>
		</div>
	);
}
```

If we want to preserve state for removed components, we should lift the state up to their common parent or use another external data structure to store the state such as `localStorage` and initialize its state there when rendering.
