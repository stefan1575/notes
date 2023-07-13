# **Mouse Hover Events**

## **I. `relatedTarget`**

### **`mouseover`**

The `mouseover` event occurs when a pointer hovers over an element or its children.

- `event.target` - the hovered element itself
- `event.relatedTarget` - the element that the mouse left (`relatedTarget` → `target`)

### **`mouseout`**

The `mouseout` event occurs when a pointer moves out of an element or its children after it has been hovered over.

- `event.target` - the element that the mouse left
- `event.relatedTarget` - the hovered element itself (`target` → `relatedTarget`)

The `relatedTarget` property can be `null` if it either left or came from the window. Using `event.relatedTarget.tagName` may result in an error.

### **Element skipping**

In case of fast mouse movements, the pointer can jump multiple elements at once.

We should use both `mouseover` and `mouseout` since the only thing we know for certain is that a pointer entered and exited the element.

## **II. `mouseout` element traversal**

The `mouseout` event also triggers when the pointer enters one of its child elements.

When this happens, the `event` will trigger on the previously hovered element instead of the child element:

![mouseout child element traversal](/assets/mouseout-child-element-traversal.png)

This is because the mouse cursor can only be over a _single_ element at a time (including its descendant) - the most nested element and topmost element in the z-index.

### **Event bubbling**

When the `mouseout` event triggers on one of its child elements, a new `mouseover` event may trigger in the child element itself because the event handler of the ancestor bubbles up:

- `mouseover`: parent → `mouseout`: parent → `mouseover`: child

![mouseout element traversal](/assets/mouseout-child-element-bubbling.png)

To avoid this behavior, we can check `relatedTarget` if the mouse is still inside the element or use the `mouseenter` and `mouseleave` events.

## **III. `mouseenter`/`mouseleave`**

- `mouseenter` - triggers when the pointer enters the element with the event
- `mouseleave` - triggers when the pointer leaves the element with the event

The main difference is that:

1. The element's descendants are treated as the element itself
2. The `mouseenter`/`mouseleave` events do not bubble

### **Event delegation**

Since the `mouseenter`/`mouseleave` events do not bubble, we can't use event delegation with them.

As a result, we can't get information about the transitions in its children since it only triggers to the parent element as a whole.

We should use the `mouseover`/`mouseout` if we want to use event delegation.
