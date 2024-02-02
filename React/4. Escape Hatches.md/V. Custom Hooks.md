# **Custom Hooks**

Custom hooks are JavaScript functions that contain stateful logic which contains calls to other hooks that can be shared between multiple components.

To extract a custom hook from a component, identify the stateful logic that could be reused elsewhere and move the logic to the new function with the returned state that we wnat to use.

If the logic doesn't contain other calls to hook, use a regular function instead.

## **`useEffect` calls in Custom Hooks**

Hooks run whenever the component is rendered and since they are part of the component body, they need to pure.

Since custom hooks re-render together with the component they recieve the latest props and state.

This means that we can create our a templated Effect hook. To pass the dependencies, we can take an object with the dependencies that we want to pass as its argument.

Note that Effects should only be used when connecting to an external system.

## **Naming conventions**

The common convention is to prefix the custom hooks with `"use"`. Its name should contain information of what the custom hook does.

When synchornizing with an external system, the custom hook name should contain a more technical term for that system.
