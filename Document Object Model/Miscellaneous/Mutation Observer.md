# **`MutationObserver`**

The `MutationObserver` constructor creates an object that observes a DOM element and fires a callback when it mutates.

```js
let observer = new MutationObserver(callback);
```

- `callback` - the function to run after each mutation that occured in the target element. The `MutationRecord` object is passed in the callback.

### **`MutationObserver`: `observe`**

The `observe` method attaches the observer to the target element with the given scope of changes to observe.

```js
observer.observe(target, options);
```

- `target` - the DOM node to watch for changes.
- `options` - an object describing which DOM mutations should be reported to the observer's callback.
  - `childList` - detects addition and removal of its direct children
  - `subtree` - detects changes from all its descendants
  - `attributes` - detects the modification of attribute values.
    - `true` by default if `attributeFilter` is specified.
  - `attributeFilter` - an array of specific attribute names to be monitored. If not included, all attribute changes cause mutation notifications.
  - `characterData` - detects changes to the character data or the value of `node.data` contained in the specified node(s).
  - `attributeOldValue` - stores the previous attribute value before the change.
  - `characterDataOldValue` - stores the character value before the change.

### **`MutationRecord`**

The `MutationRecord` object represents an individual DOM mutation.

- `type` - the type of mutation that occured:
  - `"attribute"` - attribute mutation.
  - `"characterData"` - text node mutation.
  - `"childList"` - child elements added/removed.
- `target` - the node where the mutation occured.
  - For `attributes` and `childList`, the element where the change occured.
  - For `characterData`, the text node.
- `addedNodes/removedNodes` - the nodes that were added/removed.
- `oldValue` - the previous attribute/character value before the change.
- `attribute` - the name of the changed attribute.
