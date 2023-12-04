# **Browser default actions**

Many events automatically lead to certain actions performed by the browser.

There are two ways to stop an default action from occuring:

## **I. `onclick="return false"`**

The return value of the event handler is not used and does not affect the result.

An exception is an event handler is assigned using `on<event>` with a value of `return false`:

```html
<!-- Link does not work -->
<a href="https://google.com" onclick="return false">Google</a>
```

This does not work for `addEventListener` however.

## **II. `event.preventDefault()`**

The `event.preventDefault()` method stops the default action of an event when assigned inside an event handler.

### **Checking if `preventDefault` ran**

- `event.defaultPrevented` - returns `true` an element's the default action was prevented.
