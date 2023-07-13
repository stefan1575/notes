# **Coordinates**

JavaScript coordinates reference either one of the two systems:

### **A. Relative to `window`**

Similar to `position: fixed` where the element's position is calculated relative to the browser window's top-left edge.

When the document is scrolled, the coordinates change based on its position in the browser window.

### **B. Relative to `document`**

Similar to `position: absolute` where the element's position is calculated relative to the entire document's top-left edge.

When the document is scrolled, the position of the element stays the same.

## **I. Element coordinates: `getBoundingClientRect`**

The `getBoundClientRect` method returns an object that contains properties about the size of the element and its position relative to the viewport.

![getBoundingClientRect Properties](/assets/getBoundingClientRect.png)

- `x` - the horizontal position of its _rectangle origin_ or its top-left corner
- `y` - the vertical position of its _rectangle origin_ or its top-left corner
- `width/height` - the element's size
- `left` - the _horizontal position_ relative to its top-left corner or `x`
- `top` - the _vertical position_ relative to its top-left corner or `y`
- `right` - the _horizontal position_ relative to its bottom-right corner or `x + width`
- `bottom` - the _vertical position_ relative to its bottom-right corner or `y + height`

## **II. Document: `elementFromPoint(x, y)`**

The `elementFromPoint` method returns the most nested element at the specified `x, y` coordinates.

This can be used for manipulating elements on the selected point in the window:

```js
let x = document.documentElement.clientWidth / 2;
let y = document.documentElement.clientHeight / 2;

let elem = document.elementFromPoint(x, y);
alert(elem.tagName); // outputs tag that is in the middle of the window
```
