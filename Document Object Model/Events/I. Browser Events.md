# **Browser Events**

An event is a signal that something has happened. It can be either triggered by user action or programmatically.

The `EventTarget` interface are objects that can recieve events and event listeners. The commonly used ones are `Element`, `Document` and `Window`.

## **I. Types of Events**

### **Mouse Events**

- `click` - clicking/tapping on an element
- `contextmenu` - right-clicking an element
- `mouseover/mouseout` - cursor hovers in/out on an element
- `mousedown/mouseup` - button is pressed/released on an element
- `mousemove` - cursor is moved

### **Keyboard Events**

- `keydown/keyup` - key is pressed/released

### **Scroll Events**

- `scroll` - triggers on window and element scroll

### **Form element Events**

- `submit` - a `<form>` is submitted
- `focus` - an element recieves focus

### **Document Events**

- `DOMContentLoaded` - HTML document is fully loaded

### **CSS Events**

- `transitioned` - CSS animation finishes

## **II. Event handlers**

Event handlers are JavaScript functions that run when an event occurs. They can be assigned as an HTML attribute or an element property.

To remove a handler, we should set the corresponding handler to `null`.

### **HTML attribute**

```html
<!-- HTML attribute -->
<input value="Button" onclick="output(Clicked)" type="button" />

<script>
	function output(msg) {
		console.log(msg);
	}
</script>
```

### **Element property**

```html
<!-- Element property -->
<input value="Button" id="elem" type="button" />

<script>
	let element = document.getElementById("elem");
	function output(msg) {
		console.log(msg);
	}

	element.onclick = output;
</script>
```

In event handlers, the value of `this` is the element itself.

```html
<!-- outputs "Content" -->
<button onclick="alert(this.innerHTML)">Content</button>
```

## **III. EventTarget: `addEventListener()`**

Reassigning an event handler using as an HTML attribute or an element property just overwrites the existing one.

The `addEventListener` method allows us to add an event listener to any method with the `EventTarget` interface.

The syntax is:

```js
elem.addEventListener(event, handler, [options]);
```

- `event` - the name of event
- `handler` - the assigned function to the event
- `options` - An object that specifies characteristics about the event listener.
  - `once` - if `true`, the event listener will be removed after it is triggered.
  - `capture` - the event will be set on the capture phase
  - `passive` - if `true`, indicates that the handler won't call `preventDefault`. Useful for improving scroll performance

It is the recommended way to add an event because it allows us to add more than one event:

```js
function handler() {
	console.log("Hello");
}

function anotherHandler() {
	console.log("Hi");
}

elem.addEventListener("click", handler);
elem.addEventListener("click", anotherHandler);
```

### **`Event` Object**

The `addEventListener` method only has one parameter and it is reserved for the _event object_ which contains information about the event that occured.

The commonly used ones are:

- `event.type` - the type of event
- `event.target`- reference to the element that triggered the event
- `event.clientX/event.clientY` - coordinates of the mouse pointer relative to the viewport

## **IV. `removeEventListener()`**

The syntax is:

```js
elem.addEventlistener(event, handler, [options] / useCapture);
```

- `options`
  - `capture` - it must be specified if the event has a capture phase, otherwise the event won't be removed
- `useCapture`

#### **Storing functions as a variable**

We should store functions in a variable when assigning them as an event handler if we plan on removing them. Otherwise there is no way to reference the same function object assigned from `addEventListener`:

```js
function handler() {
	console.log("Hello");
}

button.addEventListener("click", handler);
button.removeEventListener("click", handler);
```

## **V. Object: `handleEvent`**

We can also assign objects as event handlers. If an event is triggered, the object's `handleEvent` method is called:

```js
let handler = {
	handleEvent() {
		console.log("Hello");
	},
};

button.addEventListener("click", handler);
```
