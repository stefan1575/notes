# **Keyboard Events**

- `keyup` - triggers on keyboard press
- `keydown` - triggers on keyboard release

## **I. `Event` properties**

The `key` property is used for layout-dependant keys, while the `code` property is used for hotkeys.

- `key` - value of the key itself
- `code` - the physical key code on the keyboard, not affected by shift modifiers.
  - Letter keys return `Key<letter>` (i.e. `"KeyA"`)
  - Number keys return `Digit<number>` (i.e. `"Digit0"`)
  - Special keys return their own names (i.e. `"Enter"`)

An exception is the `Fn` key where it doesn't have an associated keyboard event.

## **II. Default actions**

### **Auto-repeat**

When a key is pressed long enough, multiple `keydown` events trigger until a `keyup` event occurs.

The `repeat` property returns `true` if a key is automatically repeating. This can be used for events that trigger on auto-repeat.

### **`preventDefault` limitations**

Normally when using the `preventDefault` method on `keydown` will cancel the key press.

Preventing the default action on `keydown` does not work OS-based special keys.

### **Mobile keyboards**

Keyboard logic may not always work on mobile devices since it may return values such as `"unidentified"`.
