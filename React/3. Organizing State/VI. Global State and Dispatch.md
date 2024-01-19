# **Global State and Dispatch**

The `reducer` function consolidates the logic of event handlers making writing them more concise.

One downside of the `reducer` is that its returned `state` and `dispatch` are only available top level of a component.

This means that we have to pass down the `state` and the associated reducer event handlers via props which can cause prop drilling.

Making the returned `state` and `dispatch` variables of the `useReducer` hook as context allows us to share state deep within the tree without the use of prop drilling.

## **I. Defining Global State and Dispatch**

To make the `state` and `dispatch` global:

1. Define the `state` and `dispatch` as context variables where the value will be provided in the context provider component.
2. Attach the `useReducer` hook in the top level of the context provider component.
3. Create a component containing the state context provider with a the dispatch context provider as its child. Their `value` prop should be the corresponding returned `state` and `dispatch` variables of the `useReducer` hook.

```jsx
// ./Context.js
import { createContext } from "react";

export const StateContext = createContext(null);
export const DispatchContext = createContext(null);

// ./App.js
import { useReducer } from "react";
import { StateContext, DispatchContext } from "Context.js";

export default function App() {
	const [state, dispatch] = useReducer(reducer, initialState);

	return (
		<StateContext.Provider value={state}>
			<DispatchContext.Provider value={dispatch}></DispatchContext.Provider>
		</StateContext.Provider>
	);
}
```

## **II. Consuming Global State and Dispatch**

- Event Handlers - If the event handler logic is written in the `reducer`, we can import `useContext` and the dispatch context the global `dispatch` and can read it with `useContext(DispatchContext)`.
- Component Props - If our nested component relies on the root component's `state`, we can import `useContext` and the state context and read it with `useContext(StateContext)`.

## **III. Global Context Patterns**

We can reduce boilerplate code by creating a context provider component that takes `children` as a prop to pass JSX inside it.

1. Create a context provider component that takes `children` as a prop to pass JSX inside it.
2. Attach a `useReducer` hook with our own `reducer` and `initialState`.
3. Return a component containing the state context provider with a the dispatch context provider as its child where its `value` is the returned `state` and `dispatch` of the `useReducer` hook.

```jsx
import { useReducer, createContext } from "react";

export const StateContext = createContext(null);
export const DispatchContext = createContext(null);

export function ContextProvider({ children }) {
	const [state, dispatch] = useReducer(reducer, initialState);

	return (
		<StateContext.Provider value={state}>
			<DispatchContext.Provider value={dispatch}>
				{children}
			</DispatchContext.Provider>
		</StateContext.Provider>
	);
}
```

We can shorten the `useContext` call by creating a function that returns the `useContext` with the corresponding `state` or `dispatch` as an argument.

```jsx
// ...
import { useContext } from "react";

export function useStateName() {
	return useContext(StateContext);
}

export function useDispatch() {
	return useContext(DispatchContext);
}
```
