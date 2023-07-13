# **Form Submission**

The `submit` event triggered on the `<form>` element when it's submitted.

This event is triggered when:

1. A `click` event triggers on an element inside the `<form>` with either:
   - A `<button>` with the default `type` value of `submit`.
   - An `<input>` with an `type` value of `submit` or `image`.
2. The user presses <kbd>Enter</kbd>.
   - A `click` event also triggers when this happens.

## **`submit` method**

The `form.submit` method submits the given `<form>` without triggering the `submit` event.

This means that the event handlers of the `<form>` will not trigger.
