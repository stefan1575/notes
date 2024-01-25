# **`useRef`**

The `ref` object contains a `current` property with the given `initialValue` as its value.

The `useRef` hook creates a _ref_ object that contains a `current` property with the given argument as its value.

Unlike state, does not trigger a re-render and it behaves like an regular JavaScript object and unlike regular variables it isn't initialized from scratch every time a component re-renders.

```jsx
import { useRef } from "react";

export default function Counter() {
	const ref = useRef(0);

	function handleClick() {
		ref.current = ref.current + 1;
		alert(ref.current);
	}

	return <button onClick={handleClick}>Counter</button>;
}
```

They are typically used for timeout IDs, storing and manipulating DOM elements, and storing values that aren't necessary to calculate JSX (values that don't require a re-render).

### **Best Practices**

To make your components more predictable:

1. Treat refs as an escape hatch for external or browser APIs.
2. Avoid reading or writing the `ref` object during rendering. The render function is expected to be pure where and it can accidentally introduce side-effects.
3. Stick to non-destructive actions like reading the DOM node or changes that don't modify the DOM manually. You can safely modify parts of the DOM that aren't managed by React however.

The exception is initializing the ref with a value during the first render when you need to lazily initialize something expensive and complex.

This applies to refs holding DOM nodes since during the first render, `ref.current` will be `null` so it will be too early to read them. Refs are usually accessed from event handlers or Effects.

```jsx
import { useRef } from "react";

function App() {
	const myRef = useRef(null);
	function initializeRef() {
		if (!myRef.current) {
			myRef.current = "Value";
		}

		return myRef.current;
	}

	function handleEvent() {
		const map = initializeRef();
		// ...
	}
}
```

## **I. `ref` attribute**

The `ref` attribute attaches a reference to the DOM node in the given `ref` object.

```jsx
import { useRef } from "react";

export default function App() {
	const myRef = useRef(null);
	return <div ref={myRef}></div>;
}
```

This allows to perform browser API methods that are available to the referenced DOM node.

### **`ref` callback**

Since hooks can only be called at the top level of a component, you cannot generate a list of refs using the `useRef` hook conditionally or inside a loop.

When a function is provided in the `ref` attribute, it will be automatically called in the initial render. The callback function is either called with the current `node` or `null` when the component is mounted or unmounted.

```jsx
import { useRef } from "react";

const list = [
	{ id: 0, name: "Alpha" },
	{ id: 1, name: "Beta" },
	{ id: 2, name: "Charlie" },
];

export default function App() {
	const myRef = useRef(null);
	function initializeRef() {
		if (!myRef.current) {
			myRef.current = "Value";
		}
		return myRef.current;
	}

	return (
		<ul>
			{list.map((elem) => (
				<li
					key={elem.id}
					ref={(node) => {
						const map = initializeRef();
						if (node) {
							map.set(elem.id, node);
						} else {
							map.delete(elem.id);
						}
					}}
				>
					{elem.name}
				</li>
			))}
		</ul>
	);
}
```

## **II. `forwardRef`**

By default, custom components cannot be given refs because React doesn't let a component access the DOM nodes of other components.

The `forwardRef` function creates a component that can expose a DOM node with the `ref` attribute.

### **`render`**

It accepts a `render` function that contains the `props` and `ref` as parameters which are passed in the returned component.

```jsx
import { forwardRef, useRef } from "react";

const CustomInput = forwardRef((props, ref) => {
	return <input {...props} ref={ref} />;
});

export default function App() {
	const myRef = useRef(null);

	function handleClick() {
		myRef.current.focus();
	}

	return (
		<>
			<CustomInput ref={myRef} />
			<button onClick={handleClick}>Focus</button>
		</>
	);
}
```

## **II. `useImperativeHandle`**

A common pattern is for low-level components to expose their refs with the `forwardRef` function for the parent component to perform browser API methods or manipulate their behavior.

The `useImperativeHandle` hook allows us to control which methods are available to the component that has its ref exposed.

The methods that are exposed don't have to be browser API methods, we can perform our own code alongside it as well.

```jsx
import { useImperativeHandle, forwardRef, useRef } from "react";

const CustomInput = forwardRef((props, ref) => {
	const customInputRef = useRef(null);

	useImperativeHandle(ref, () => {
		return {
			focus() {
				customInputRef.current.focus();
			},
		};
	});
	return <input {...props} ref={customInputRef} />;
});

export default function App() {
	const myRef = useRef(null);

	function handleClick() {
		myRef.current.focus();
	}

	return (
		<>
			<CustomInput ref={myRef} />
			<button onClick={handleClick}>Focus</button>
		</>
	);
}
```

## **III. `flushSync`**

In React, state updates are queued where the DOM nodes affected by the update are reflected during the commit phase.

This can affect third party integrations such as browser APIs and UI libraries since the exposed method of the DOM node will reference the current DOM which has queued state updates.

The `flushSync` function forces the state updates inside its callback argument to update the DOM synchronously.

```jsx
import { flushSync } from "react-dom";

flushSync(() => {
	setState("Value");
});
```

This should be used
