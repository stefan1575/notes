# **Event Handlers**

To attach an event to a component, we should pass the event handler function as a value enclosed in curly braces to the event listener as a prop.

```jsx
function MyButton() {
	function clickHandler() {
		console.log("Clicked");
	}

	return <button onClick={clickHandler}>MyButton</button>;
}
```

### **Reading props in Event Handlers**

Event handlers have access to the component that they are attached in.

## **I. Passing Event Handlers as props**

Since event handlers are props themselves, they can be passed to their child components.

A common implementation is to make the parent component specify the child's event handler.

```jsx
function Button({ onClick, children }) {
	return <button onClick={onClick}>{children}</button>;
}

export default function PlayButton() {
	function handleClick() {
		alert("Playing Video");
	}

	return <Button onClick={handleClick}>Back</Button>;
}
```

### **Event Handler Prop names**

Built-in components only support native JavaScript events prepended by `"on"` as their props.

In custom components, the native JavaScript events can be renamed in the component's props. However by convention, it should start with `"on"` followed by a capital letter.

```jsx
// renaming onClick to custom event name
function Button({ onPlay, children }) {
	return <button onClick={onPlay}>{children}</button>;
}

export default function PlayButton() {
	function handleClick() {
		alert("Playing Video");
	}

	return <Button onPlay={handleClick}>Back</Button>;
}
```

## **II. Event Propagation**

When an event triggers, it propagates up the tree which triggers all ancestor component events.

The `event.stopPropagation()` method stops an event from reaching its parent components.

```jsx
function Button({ onClick, children }) {
	return <button onClick={onClick}>{children}</button>;
}

export default function App() {
	function handleClick(event) {
		event.stopPropagation();
		alert(`You clicked the ${event.target.tagName}`);
	}

	return (
		<div onClick={handleClick}>
			<Button onClick={handleClick}>Button</Button>
		</div>
	);
}
```

### **Capture Phase**

When an capture phase event triggers, it runs the outermost ancestor with a capture phase first, then propagates down triggering all the capture phase handlers until the event target is reached before entering the bubble phase.

In React, we can append `Capture` at the end of the event name prop to make it run at the capture phase.

```jsx
// runs the capture phase handler first when button is clicked
export default function Analytics() {
	function handleClick(e) {
		e.stopPropagation();
		console.log("btn clicked");
	}

	return (
		<div
			onClickCapture={() => {
				console.log("div clicked");
			}}
		>
			<button onClick={handleClick}>Btn 1</button>
			<button onClick={handleClick}>Btn 2</button>
		</div>
	);
}
```

### **Component prop function call**

Another common implementation is to call the passed function prop inside the as event handler instead of directly passing it as the event handler's value.

This allows the parent component to specify some additional behavior such as stopping the event from bubbling up its ancestor.

```jsx
function Button({ onClick, children }) {
	return (
		<button
			onClick={(event) => {
				event.stopPropagation();
				onClick();
			}}
		>
			{children}
		</button>
	);
}

export default function App() {
	function handleClick(event) {
		alert(`You clicked the ${event.target.tagName}`);
	}

	return (
		<div onClick={handleClick}>
			<Button onClick={handleClick}>Button</Button>
		</div>
	);
}
```
