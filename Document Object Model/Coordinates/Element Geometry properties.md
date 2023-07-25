# **Element Geometry properties**

## **I. Element size**

![Element size properties](/assets/element-size-properties.png)

The elementBesides its content size, an element's properties also encompass the space it occupies, except for the margin.

In cases where an element has a scrollbar, some browsers may reserve space for it by subtracting it from the content width.

## **II. Element - Geometry properties**

![Element geometry properties](/assets/element-geometry-properties.png)

Geometry is calculated only for displayed elements. Thus, if an element is not appended in the document or has a `display: none` property, it returns zero.

These properties should be used if we want to retrieve an elements height/width for accurate calculations.

### **A. Ancestral Geometry Properties: `offsetParent` & `offsetLeft/Top`**

#### **`offsetParent`**

The `offsetParent` read-only property returns a reference to the element that either contains:

1. the nearest _positioned ancestor element_ which is a non-static position value like `absolute`, `relative`, `fixed`, or `sticky`.
2. `<table>` properties it returns `td`, `th`, `table` .
3. `body` if there are no _positioned ancestor element_.

It returns `null` when either the element has a property of `display: none` or `position: fixed` or the element itself is `<body>` or `<html>`

#### **`offsetLeft/Top`**

The `offsetLeft/Top` read-only property returns the number of pixels relative to the upper-left corner of its `offsetParent` as an integer.

```html
<div style="position: relative" id="ancestor element">
	<div style="position: absolute; left: 10px; top: 10px" id="elem">Content</div>
</div>
<script>
	console.log(elem.offsetParent.id); // ancestor element
	console.log(elem.offsetLeft); // 10 (not px)
	console.log(elem.offsetTop); // 10
</script>
```

### **B. Outer element size: `offsetWidth/Height`**

The `offsetWidth/Height` read-only property returns the full size of the element as an integer, which includes its padding, scrollbar, and border.

### **C. Border size: `clientTop/Left`**

- `clientTop` - a read-only property that returns the width of the top border in pixels height as an integer.
- `clientLeft` - a read-only property that returns the width of the left border in pixels as an integer. It includes the left scrollbar rendered by overflow from right to left text directions.

### **D. Content size: `clientWidth/Height`**

The `clientWidth/Height` read-only property returns the content size and its padding as an integer, excluding the scrollbar.

### **E. Non-visible content size: `scrollWidth/Height`**

The `scrollWidth/Height` read-only property returns the content size, padding, and the content not visible on the screen due to overflow.

### **F. Current scroll position: `scrollLeft/Top`**

- `scrollTop` - gets or sets the number of pixels that an element's content is scrolled horizontally.
  - It becomes `0` if set to a negative value.
  - It becomes the maximum value if set to a value greater than the element.
- `scrollLeft` - gets or sets the number of pixels that an element's content is scrolled vertically.
  - If the text direction is rendered from right to left, it starts at `0` and becomes increasingly negative.
  - It becomes `0` if set to a negative value (or values greater than `0` for right-to-left elements).
  - It becomes the maximum value if set to a value greater than the element.
