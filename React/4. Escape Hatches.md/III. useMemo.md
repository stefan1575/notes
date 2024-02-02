# **`useMemo`**

When the state is updated, it triggers a re-render of the entire component. This can affect performance when an expensive operation is always calculated in some part the component.

The `useMemo` hook that caches the result of a calculation between re-renders. When a value in the dependency array changes, it will trigger a re-calculation of the value.

- `calculateValue` - the function calculating the value you want to cache.
- `dependencies` - an array of reactive values to watch for changes.
