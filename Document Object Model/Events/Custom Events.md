# **Custom Events**

## **I. Creating custom events**

### **`Event` constructor**

The syntax is:

```js
new Event(type[, options]);
```

- type - a string containing either the event type or our our own custom event
- options - An object that specifies characteristics about the event listener.
  - `bubbles` - if `true`, the event bubbles
  - `cancelable` - if `true`, the event's default action may be prevented
  - `composed` - if `true`, the propagation path will start from the element residing in the shadow DOM instead of being retargeted to the shadow host element.

### **B. `CustomEvent` constructor**

It is the same as the `Event` constructor with one additional property in the options object.

- `detail` - a property with a value of an object that contains our own custom data which can be read using the `event.detail` property.

We should use the `CustomEvent` constructor to be clear that it is a custom event.

### **C. UI Event constructors**

For instance, we can create our own event with our own specified characteristics:

```js
new MouseEvent("click", {
	clientX: 100,
	clientY: 100,
});
```

- type - a string containing one of the specific event names
- options - an object that has characteristics specific to the event type

## **II. Running custom events: `dispatchEvent`**

The `elem.dispatchEvent()` method fires events created by constructors.

Calling it is the last step to firing the event, where the event should already be created using a constructor and initialized using `addEventListener`.

## **III. `event.preventDefault()`**

Custom events don't have any default browser actions, so calling `preventDefault()` doesn't have any effect.

However, you can use the `defaultPrevented` property to signal that the event has been handled and further processing should be stopped:

```js
const customEvent = new CustomEvent("my-event", {
	cancelable: true,
	detail: { eventType: "CustomEvent" },
});

const elem = document.getElementById("elem");

elem.addEventListener("my-event", function (event) {
	event.preventDefault();
	console.log(event.detail);
});

elem.dispatchEvent(myEvent);

// captured the event in the variable
if (customEvent.defaultPrevented) {
	console.log("Default action should not be performed");
}
```

## **IV. Event execution order**

Events are processed in a queue, where it waits for the current event to finish processing before proceeding to the next one.

This does not apply for events that are initiated using another event. The new event handler is processed first before the current event resumes.

```js
let elem = document.getElementById("elem");

function eventHandler() {
	alert("1");

	let customEvent = new CustomEvent("my-event", { bubbles: true });
	elem.dispatchEvent(customEvent);

	alert("2");
}
// "1", "nested", "2"
elem.addEventListener("click", eventHandler);
elem.addEventListener("my-event", () => alert("nested"));
```

We can use a zero delay `setTimeout` if we want the event to run last:

```js
let elem = document.getElementById("elem");

function eventHandler() {
	alert("1");

	let customEvent = new CustomEvent("my-event", { bubbles: true });
	setTimeout(() => elem.dispatchEvent(customEvent));

	alert("2");
}
// "1", "2", "nested"
elem.addEventListener("click", eventHandler);
elem.addEventListener("my-event", () => alert("nested"));
```
