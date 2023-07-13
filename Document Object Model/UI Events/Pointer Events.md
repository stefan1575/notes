# **Pointer Events**

Pointer events handle input from a variety of pointing devices such as a mouse, touchscreen, and pen/stylus.

## **I. Pointer event types**

- `pointerdown` - button is clicked
- `pointerup` - button is released
- `pointermove` - every mouse move over the element
- `pointerover` - triggers when a pointer hovers over an element or its children.
- `pointerout` - triggers when a pointer moves out of an element or its children after it has been hovered over.
- `pointerenter` - triggers when the pointer enters the element with the event.
- `pointerleave` - triggers when the pointer leaves the element with the event.
- `pointercancel` - triggers when the pointer event is forcefully canceled.
- `gotpointercapture` - triggers when an element uses the `setPointerCapture` method.
- `lostpointercapture` - triggers when the capture is released either by `pointerup`, `pointercancel`, or `releasePointerCapture`.

## **II. `Event` properties**

- `button` - returns a value that corresponds to the mouse button.
  - `0` - Left button
  - `1` - Middle button
  - `2` - Right button
  - `3` - X1 button (back)
  - `4` - X2 button (forward)
- `shiftKey` - Shift
- `altKey` - Alt / Opt (Mac)
- `ctrlKey` - Ctrl
- `metaKey` - Cmd (Mac)
- `clientX/clientY` - window-relative x/y coordinates
- `pageX/pageY` - document-relative x/y coordinates

### **Pointer unique properties**

- `pointerId` - returns a unique identifier of the pointer that triggered the event.
- `pointerType` - a string with the pointing device type.
  - returns either `"mouse"`, `"pen"`, or `"touch"`.
- `isPrimary` - returns `true` for the primary pointer (the first finger in multi-touch).
- `pressure` - returns the pressure of the pointer between `0` to `1` for _pen and touch_ devices.
  - returns `0.5` when the pointer is pressed and `0` otherwise.

### **Touch specific properties**

- `width/height` - returns the contact area of the width/height along the corresponding x/y-axis in pixels for touch devices.
  - returns `1` if not supported

### **Pen specific properties**

- `tangentialPressure` - returns the tangential pressure or the force applied in the direction parralel to the surface of the pointer between `0` to `1` for pen devices.
  - returns `0` if not supported
- `tiltX` - returns the angle in degrees between the Y-Z plane
  - when tilted to the right returns 0 to 90
  - when tilted to the left returns 0 to -90
- `tiltY` - returns the angle between the Y-X plane
  - when tilted downwards returns 0 to 90
  - when tilted upwards returns 0 to -90
- `twist` - the clockwise rotation of the pointer around its major axis (a pen perpendicular to the screen) between 0 to 359.
  - returns `0` if not supported

## **III. Multi-touch algorithm**

When there are simultaneous finger presses in a touchscreen, multiple `pointerdown` events are generated with their own coordinates and `pointerId`.

1. At the first and proceeding finger presses, a `pointerdown` event is generated with their own `pointerId` and `isPrimary` values.
2. When the finger is moved and released, the corresponding `pointermove` and `pointerup` events are generated. We can track the released finger using the `pointerId` property.

## **IV. `pointercancel`**

The `pointercancel` event triggers when there is an ongoing pointer event, and then something causes it to be aborted which stops the ongoing event.

For instance:

- Device screen orientation changed
- Hardware event that cancels the pointer events from occuring
- Browser has its own default interaction that overrides the custom behavior
- A css attribute of `touch-action: none`

To prevent this event from occuring, use `preventDefault`.

## **V. Pointer Capture**

Pointer capture allows pointer events to continue recieving pointer events even if the pointer device moves out of the element.

This prevents other events from triggering as opposed to binding listener to `document` where other unrelated events may trigger.

### **Element: `setPointerCapture(pointerId)`**

The `setPointerCapture` method allows the element to keep recieving pointer events even if its outside the element.

```js
const thumb = document.querySelector(".thumb");
const slider = document.getElementById("slider");
thumb.addEventListener("pointerdown", function (event) {
	event.preventDefault();
	thumb.setPointerCapture(event.pointerId);

	const initialX = event.clientX - thumb.getBoundingClientRect().left;

	function pointermoveHandler(event) {
		const position = slider.getBoundingClientRect();
		const leftEdge = position.left;
		const rightEdge = position.left + position.width - thumb.offsetWidth;

		let currentPosition = event.clientX - initialX;

		if (currentPosition < leftEdge) {
			currentPosition = leftEdge;
		}
		if (currentPosition > rightEdge) {
			currentPosition = rightEdge;
		}
		thumb.style.left = currentPosition - leftEdge + "px";
	}
	thumb.addEventListener("pointermove", pointermoveHandler);

	function pointerupHandler() {
		thumb.removeEventListener("pointermove", pointermoveHandler);
		thumb.removeEventListener("mouseup", pointerupHandler);
	}
	thumb.addEventListener("mouseup", pointerupHandler);
});
```

The element designated as the _capture target_ stops recieving pointer events when `elem.releaseCaptureTarget(pointerId)` is called or the `pointerup` event is fired.
