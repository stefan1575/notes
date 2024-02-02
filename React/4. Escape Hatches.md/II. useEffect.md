# **useEffect**

### **Side-Effects**

A side-effect is an operation that happens outside the scope of the current function.

Event handlers usually change the state of a component, which produces side effects since it triggers a re-render that updates a component with new data.

## **I. Effect Function**

The Effect function lets you specify side-effects that are caused by rendering itself.

It is executed after the commit phase of each render including the initial render, so its guaranteed to run at least once.

It describes operations that cannot be done during rendering.

### **Cleanup function**

The cleanup function is executed when the component unmounts and when a dependency triggers a re-render. It is defined by specifying it as the Effect function's return value.

It is used to undo the side-effects performed by the Effect function. Common use-cases include detaching event listeners, aborting fetch requests, and restore variables to their initial values.

### **II. `useEffect`**

The `useEffect` hook runs the Effect function when one of the _reactive values_ in the dependencies array trigger a re-render.

```jsx
import { useState, useRef, useEffect } from "react";

function VideoPlayer({ src, isPlaying }) {
	const videoRef = useRef(null);

	useEffect(() => {
		if (isPlaying) {
			videoRef.current.play();
		} else {
			videoRef.current.pause();
		}
	}, [isPlaying]);

	return <video ref={videoRef} src={src} loop playsInline />;
}

export default function App() {
	const [isPlaying, setIsPlaying] = useState(false);

	function handleClick() {
		setIsPlaying(!isPlaying);
	}

	return (
		<>
			<button onClick={handleClick}>{isPlaying ? "Pause" : "Play"}</button>
			<VideoPlayer
				src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
				isPlaying={isPlaying}
			/>
		</>
	);
}
```

## **III. Dependencies Array**

When a re-render triggers, and the reactive value in the dependency array didn't change, React will skip re-running the Effect function.

Props, state, and other values declared inside the component are reactive values since they are calculated during rendering.

#### **Removing Effect Dependencies**

For React to consider them non-reactive, they have to be declared outside of the component or inside the Effect function.

If we want to set state inside an effect but don't want to cause it to trigger a re-render, we can pass an updater function inside the state's `set` function. The state will be updated at the next render.

### **A. Omitted Dependencies Array**

If the dependencies array is omitted, the Effect function will run after every render of the component.

### **B. Empty Dependencies Array**

If the Effect function doesn't rely on any reactive values and an empty dependency array is provided, it will only run on the initial render. In development, both the setup and cleanup function will run an extra time, which is helpful for finding bugs.

## **IV. Effect Heuristics**

An Effect should only be used to execute external system code as a result of information displayed to the user.

Common use-cases of `useEffect` include the browser DOM, network requests, and third party widgets.

### **Unnecessary Effects**

Effects are unnecessary when:

1. Data is tranformed as a result of state or prop change.

Setting state inside an Effect triggers another render pass. State and props can be calculated during rendering.

When a prop changes, it is better to store a prop as a state and set the state conditionally while rendering. If we want to reset the state, we should give a component a explicit key so that it will be treated as a new component.

2. An external system executes code that doesn't require information to be displayed to the user.

If an external system needs is executed as a result of an user interaction, it should be written inside an event instead.

### **`useSyncExternalStore`**

React has a purpose-built Hook if we need to subscribe to an _external store_ or an external data source manages its own state and emits events that your component is interested in.

The `useSyncExternalStore` hook subscribes to an external store.

```jsx
import { useSyncExternalStore } from "react";
import { externalStore } from "ExternalStore.jsx";

export default function App() {
	const dataSource = useSyncExternalStore(
		externalStore.subscribe,
		externalStore.getSnapshot
	);

	return (
		<>
			<button onClick={() => externalStore.handleClick()}>Add List Item</button>
			<ul>
				{dataSource.map((elem) => (
					<li key={elem.id}>{elem.text}</li>
				))}
			</ul>
		</>
	);
}
```

- `subscribe` - a function that subscribes to the external data source. It returns a cleanup function that unsubscribes from the external data source.
  - `callback` - a built-in event handler that is attached to the external listener.
- `getSnapshot` - a function that returns the external data source.

The `subscribe` and `getSnapshot` function are called when a render is triggered.

The event handler function that triggers a re-render must modify the external data source and call the corresponding external event listener.

```jsx
// ./ExternalStore.jsx
let nextId = 0;
let externalDataSource = [];
let externalListeners = [];

export const externalStore = {
	handleClick() {
		externalDataSource = [
			...externalDataSource,
			{ id: nextId++, text: "#" + nextId },
		];
		emitChange();
	},

	subscribe(newListener) {
		externalListeners = [...externalListeners, newListener];
		return () => {
			externalListeners.filter((listener) => {
				listener !== newListener;
			});
		};
	},

	getSnapshot() {
		return externalDataSource;
	},
};

function emitChange() {
	for (let listener of externalListeners) {
		listener();
	}
}
```

## **V. Effect Lifecycle**

A component may mount, re-render, and unmount while an Effect can start and stop synchronizning when an external system triggers a render which makes them have separate lifecycles.

Effects should represent a separate synchronization process where a new one should be created when the code synchronizes to different things.

This means that we should not add unrelated logic in the same Effect just because the process code runs at the same time. It should be split to different Effects because they might interfere with each other.
