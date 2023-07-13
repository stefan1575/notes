# **Drag and Drop with mouse events**

## **Drag and Drop algorithm**

1. `mousedown`

   - Disable the built-in drag event to prevent conflict with our own behavior.
   - Make the element `absolute` and the topmost element.

   ```js
   let ball = document.getElementById("ball");
   ball.addEventListener("mousedown", function (event) {
       // (1) disable the built-in drag event to stop conflict
       ball.ondragstart = function () {
           return false;
       };

       // (2) make the ball movable and the most visible element
       ball.style.position = "absolute";
       ball.style.zIndex = 1000;
       document.body.append(ball);
   ```

2. `mousemove`

   - Update the element position using the event's current `pageX` and `pageY` coordinates _after_ `mousedown` has been triggered.
   - Attach the event listener on `document` in case of fast mouse movements instead of `elem`

   ```js
   // (3) get the initial pointer position on mousedown
   let initialX = event.clientX - ball.getBoundingClientRect().left;
   let initialY = event.clientY - ball.getBoundingClientRect().top;

   function mousemoveHandler(event) {
   	ball.style.left = event.pageX - initialX + "px";
   	ball.style.top = event.pageY - initialY + "px";
   }
   document.addEventListener("mousemove", mousemoveHandler);
   ```

3. `mouseup` - remove the unneeded handlers to stop the event from running
   ```js
       // (5) stop the event handlers otherwise the ball follows the cursor indefinitely
       function mouseupHandler() {
           document.removeEventListener("mousemove", mousemoveHandler);
           ball.removeEventListener("mouseup", mouseupHandler);
       }
       ball.addEventListener("mouseup", mouseupHandler);
   });
   ```

## **Drop targets**

Drop targets are events that trigger when a draggable element is on top of your target element.

It needs to be set on the draggable element itself because mouse events such as `mouseover/mouseup` only happen on top of the element.

To create a drop target:

- Temporarily hide the dragged element to check the element behind.
- Set a global variable to track the drop target's state.
- Only modify the drop target if the state has changed.
- When the state has changed, update the state to reflect the current state.

```js
let dropTarget = null;
function mousemoveHandler(event) {
	ball.style.left = event.pageX - initialX + "px";
	ball.style.top = event.pageY - initialY + "px";
	// (5) temporarily disable ball to check the element behind
	ball.hidden = true;
	let elemBelow = document.elementFromPoint(event.clientX, event.clientY);
	ball.hidden = false;
	if (!elemBelow) return;

	// (6) run if currentDropTarget from previous dropTarget changed
	let currentDropTarget = elemBelow.closest(".droppable");
	if (dropTarget != currentDropTarget) {
		// When inside, 1st condition is true and 2nd condition becomes null
		if (dropTarget) {
			dropTarget.style.background = "";
		}
		// When outside, 1st condition is null and 2nd condition becomes true
		dropTarget = currentDropTarget;
		if (dropTarget) {
			dropTarget.style.background = "pink";
		}
	}
}
document.addEventListener("mousemove", mousemoveHandler);
```
