# **Data Update Events**

## **I. `input/select/textarea` Events**

- `change` - triggers when the user modifed the element's value _after_ it loses focus.
- `input` - triggers synchronously every time the element's value has been modified.
  - cannot be used with the `preventDefault` method

## **II. Clipboard Events**

- `cut/copy/paste` - triggers on their respective action

### **Event: `clipboardData`**

The `event.clipboardData` property holds the data that is being manipulated in clipboard operations.

Since these methods modify the behavior of the clipboard events, it is necessary to invoke `preventDefault`.

- `setData(format, data)` - modifies the `clipboardData` based on the given `data`

  - `format` - a string representing the data type
    (i.e `text/plain`, `text/html`, and `text/uri-list`)
  - `data` - the data to replace or modify the `clipboardData` in string format
    ```js
    document.addEventListener("copy", function (event) {
    	event.preventDefault();
    	let uppercase = event.clipboardData.toUpperCase();
    	event.clipboardData.setData("text/plain", uppercase);
    });
    ```

- `getData(format)` - retrieves the data if the data matches the specified `format`, otherwise returns an empty string.
