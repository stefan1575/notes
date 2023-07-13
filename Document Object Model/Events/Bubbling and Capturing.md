# **Bubbling and Capturing**

Event bubbling describes how the browser handles events targeted at nested elements.

## **I. Bubbling**

When an event is triggered on an element, it first runs on the element itself, then the next ancestor:

```html
<!-- alerts the element itself, then the next ancestor -->
<div onclick="alert('elem3')">
	elem3
	<div onclick="alert('elem2')">
		elem2
		<div onclick="alert('elem1')">elem1</div>
	</div>
</div>
```

There are exceptions like `focus` where bubbling doesn't occur.

### **A. Stopping bubbling: `event.stopPropagation()`**

The `stopPropagation` method stops an event from further bubbling up to its ancestors when called _inside_ an event handler:

```html
<!-- div's event does not execute because of stopPropagation -->
<div onclick="alert('does not execute')">
	<button id="btn">Button</button>
</div>

<script>
	function eventHandler(event) {
		alert("event handler executes");
		event.stopPropagation();
	}

	let btn = document.getElementById("btn");
	btn.addEventListener("click", eventHandler);
</script>
```

### **B. Stopping bubbling and subsequent events: `event.stopImmediatePropagation`**

The `stopImmediatePropagation` method stops and event from further bubbling up its ancestors and subsequent created events with the same type from executing.

## **II. Capturing**

When an event has a capturing phase, the event fires on the least nested element and goes its way down to down until the event target is reached.

### **Event propagation phases**

1. Capturing phase - the event propagates its way down to the next descendant
2. Target phase - the event reaches the event target
3. Bubbling phase - the event bubbles its way up to the next ancestor

The event handlers with capture phases execute first before the event handlers without one:

```html
<div id="outer">
	<div id="middle">
		<button id="inner">Button</button>
	</div>
</div>
<!-- Capture phase (outer, middle, inner) -->
<!-- then Bubble phase (inner, middle, outer)  -->
<script>
	let elements = {
		inner: inner,
		middle: middle,
		outer: outer,
	};

	function bubble() {
		alert(`Bubble Phase: ${this.id}`);
	}

	function capture() {
		alert(`Capture Phase: ${this.id}`);
	}

	for (let elem in elements) {
		elements[elem].addEventListener("click", bubble);
		elements[elem].addEventListener("click", capture, { capture: true });
	}
</script>
```

## **III. `event.target` vs. `this`**

- `event.target` - the element that initiated the event
- `this` - the element with the event handler

```html
<!-- event.target: <form>, <div>, <p>  -->
<!-- this: <form> -->
<form
	onclick="alert(`event.target: ${event.target.tagName}, this: ${this.tagName}`)"
	id="form"
>
	FORM
	<div>
		DIV
		<p>P</p>
	</div>
</form>
```

## **IV. Event delegation**

Event propagation enables _event delegation_ where we can assign a single event handler on their common ancestor of the child elements that are handled in the same way.

We can use `event.target` to see where the event occured.

### **Different actions, same ancestor**

Instead of using multiple event handlers for different actions that have the same ancestor, we can use a single object containing the different functions to run and match them based on an identifying factor such as a class value:

```html
<div id="menu">
	<button data-action="show">Show</button>
	<button data-action="hide">Hide</button>
	<div id="content">Content</div>
</div>
<script>
	let menuHandler = {
		handleEvent(event) {
			let action = event.target.dataset.action;
			if (action) {
				this[action]();
			}
		},
		show() {
			content.style.display = "";
		},
		hide() {
			content.style.display = "none";
		},
	};

	let menu = document.getElementById("menu");
	menu.addEventListener("click", menuHandler);
</script>
```

### **Custom attributes**

Aside from common ancestors, we can create our own custom attributes and use a document-wide handler that tracks events, then the handler executes when the event happens on the attributed element:

```html
<button data-toggle>Show/Hide</button>
<div id="content">Content</div>
<script>
	function toggleButton(event) {
		let toggle = event.target.dataset.toggle;
		let content = document.getElementById("content");

		if (toggle != undefined) {
			if (content.style.display === "none") {
				content.style.display = "";
			} else {
				content.style.display = "none";
			}
		}
	}

	document.addEventListener("click", toggleButton);
</script>
```
