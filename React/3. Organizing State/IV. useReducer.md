# **`useReducer`**

The `useReducer` hook allows us to consolidate the logic of multiple event handlers that set state into a single function called a `reducer`.

```jsx
import { useReducer } from "react";

export default function Component() {
	const [state, dispatch] = useReducer(reducer, initialState);
	// ...
}
```

## **I. `dispatch`**

The `dispatch` function calls the `reducer` function with the provided `action` as its argument.

- `action` - The argument passed to the `reducer`. It can accept any value but by convention it accepts an object with a `type` property identifying it.

## **II. `reducer`**

The `reducer` function runs when the `dispatch` function is called with its `action` and the current `state` as its arguments.

- `state` - the name of the state to update
- `action` - the argument passed on the `dispatch` function call.

Similar to `set` functions, they must be pure where the same inputs must return the same output and update the state value without performing a mutation.

```jsx
import { useReducer } from "react";

export default function App() {
	const [state, dispatch] = useReducer(reducer, { count: 0 });

	function reducer(state, action) {
		switch (action.type) {
			case "increment":
				return { count: state.count + 1 };
			case "decrement":
				return { count: state.count - 1 };
			default:
				return counter;
		}
	}

	function increment() {
		dispatch({ type: "increment" });
	}
	function decrement() {
		dispatch({ type: "decrement" });
	}
	return (
		<>
			<button onClick={decrement}>-</button>
			<h1>Counter: {state.count}</h1>
			<button onClick={increment}>+</button>
		</>
	);
}
```

## **III. `useImmerReducer`**

The `useImmerReducer` hook's `reducer` function takes a `draft` object instead of the current state which allows us to use mutating syntax.
