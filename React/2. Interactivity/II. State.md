# **State**

State is a component's built-in object that is used to store properties that belongs to that component.

It is used to store the current value of a variable resulting from an interaction.

### **Using Regular Variables**

Regular variables can't be used to store data resulting from interaction due to two things:

1. **Local variables don't persist between renders.** A re-rendered component is rendered from scratch.
2. **Changes to local variables won't trigger renders.** When an event is triggered, the component isn't re-rendered.

However, it is better to use a regular variable if the component will not be re-rendered.

## **I. Hooks**

In React, a hook is a built-in function that lets you add state or other React features to the component.

They can be only called at the top level of your component similar to `import` statements.

They are only available while React is rendering and provide a way to tap into its state and lifecycle features.

### **`useState`**

The `useState` hook lets you add a property to a component's state object.

```jsx
import { useState } from "react";
```

The `state` variable is retained and triggers a re-render when the `setState` function is called.

```jsx
const [state, setState] = useState(initialState);
```

- `initialState` - the initial value of the state variable.
- `state` - the variable name of the state variable
- `setState` - a setter function that updates the state variable. It is usually invoked in the event handler function.

```jsx
// regular variable - incorrect
let index = 0;

// useState hook - correct
const [index, setIndex] = useState(0);
```

### **Sharing State Variable**

If the same component is rendered multiple times, each of them will get their own state.

To make multiple components share data and update together, you need to move the state to their parent component.
