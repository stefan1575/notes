# **Mouse Events**

## **I. Mouse event types**

- `mousedown/mouseup` - button is clicked/released
- `mouseover/mouseout` - pointer hovers over/out
- `mousemove` - every mouse move over the element
- `click` - triggers after `mouseup/mousedown` on left click
- `dblclick` - two clicks within a short timeframe
- `contextmenu` - right click

### **Events order**

In cases where a single action initiates multiple events, the order of mouse events is as follows:

- `mouseup` → `mousedown` → `click` → `dblclick`

## **II. Event properties**

### **A. `button`**

Click related events have the `event.button` property.

This is useful for the `mouseup` and `mousedown` handlers since they trigger on any button.

| Button State        | `event.button` |
| ------------------- | -------------- |
| Left button         | 0              |
| Middle button       | 1              |
| Right button        | 2              |
| X1 button (back)    | 3              |
| X2 button (forward) | 4              |

### **B. Modifiers**

Mouse events also include information about pressed modifier keys which is useful for modifier plus click combinations:

- `shiftKey` - Shift
- `altKey` - Alt / Opt (Mac)
- `ctrlKey` - Ctrl
- `metaKey` - Cmd (Mac)

In Windows, Ctrl corresponds to Cmd in Mac. This means that if we want to support both operating systems we have to check both `(event.ctrlKey || event.metaKey)`.

We should keep mobile devices in mind when doing this however.

### **C. Coordinates**

- `clientX/clientY` - window-relative coordinates
- `pageX/pageY` - document-relative coordinates

## **III. Preventing text selection**

To prevent text selection on double click, we can set the _mousedown attribute_ with a value of `return false`:

```html
<p onmousedown="return false">Doesn't select on double click</p>
```

### **Preventing copying**

We can also prevent the user to copy text with the _copy attribute_ with a value of `return false`. This doesn't prevent the user from copying from the HTML however.

```html
<p oncopy="return false">Ctrl+C doesn't copy</p>
```
