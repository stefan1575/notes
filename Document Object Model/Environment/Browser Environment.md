# **Browser Environment**

A _host environment_ is a platform where JavaScript code is ran, typically in a browser, web-server, or another host. It provides its own objects and functions in addition to the core language.

In a web browser, it allows us to control the elements of a webpage through the `window` object.

## **I. BOM (Browser Object Model)**

BOM represents the objects provided by the browser _(host environment)_ for working with everything except the `document`. We can manipulate the behavior of its elements through the `window` object.

### **`window`**

`window` is the global object for JavaScript code which represent the current browser window or tab and provides methods to control it. It serves as the main entry point to the BOM.

![Browser Environment](/assets/host-environment.png)

For instance, we can access one of its properties:

```js
alert(window.innerHeight); // interior height of window in pixels
```

## **II. DOM (Document Object Model)**

The DOM represents all page content as objects that can be modified such as the `document` structure, style, and content.

### **`document`**

The `document` object represents the web page loaded in the browser and serves as the "main" entry point to the DOM.

By accessing it, we can to create and change anything in the web page:

```js
// change background to red
document.body.style.background = "red";

// revert the background back after 1s
setTimeout(() => (document.body.style.background = ""), 1000);
```

#### **_CCSOM (CSS Object Model)_**

CCSSOM is a rarely used object model to modify the CSS rules in JavaScript.
