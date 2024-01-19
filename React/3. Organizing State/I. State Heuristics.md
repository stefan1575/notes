# **State Heuristics**

When writing state, you have to describe the UI you want to see for the different visual states of your component, then set those state values in response to human or computer inputs.

## **I. Defining State Variables**

### **Grouping Related State**

When you have multiple state variables change together, it might be a good idea to unify them into a single state variable such as an object.

```jsx
const [coordinates, setCoordinates] = useState({
	x: 0,
	y: 0,
});
```

One caveat is that you need to copy existing properties when making a change to one property.

```jsx
setCoordinates({
	...coordinates,
	y: 20,
});
```

### **Flattening Nested State**

Instead of using a tree-like structure, you can flatten nested state by creating an array for each property containing identifiers for each of their children.

The `useImmer` hook makes updating the state object more concise since we can use the mutating syntax to directly reference identifiers of the target property and its children.

### **Mirroring Props in State**

If you pass a component prop as a state's initial value and the parent prop gets updated, the state prop of the child will not get updated since only `set` functions trigger a re-render.

Mirroring a state variable is only acceptable when you want ignore all updates for a specific prop. The convention is to prepend the name with `initial` or `default`.

```jsx
function Background({ initialColor }) {
	const [color, setColor] = useState(initialColor);
}
```

If you want the updated value of the passed prop, we should let the parent prop trigger a re-render and directly pass the prop to the component instead of using `useState`.

```jsx
function Background({ backgroundColor }) {
	return (
		<div style={color: backgroundColor}>
	)
}
```

## **II. Determining Unnecessary State**

### **Paradoxical State**

Paradoxical state occurs when multiple state variables contradict each other with one another where one can only be `true` and another can only be `false`.

This means that the state is not constrained enough and the paradoxical state only corresponds to a single state.

### **Avoiding Duplicated State**

Duplicated state occurs when a state variable stores a value that is already represented in an existing state.

We can determine that the state variable is duplicated if:

1. Its value references a property of an existing state.
2. Its value can be calculated with an existing state variable during rendering.
3. It is an inverse of value of an existing state.

This causes additional overhead since you have to update both state values for every change.
