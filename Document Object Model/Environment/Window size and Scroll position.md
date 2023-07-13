# **Window size and Scroll position**

## **I. Window height/width**

- `documentElement.clientHeight` - visible window height w/o scrollbar
- `documentElement.clientWidth` - visible window width w/o scrollbar
- `window.innerHeight` - visible window height
- `window.innerWidth` - visible window width

## **II. Scroll height/width & position**

### **A. Scroll height/width**

- Full scrollHeight/Width
  ```js
  let scrollHeight = Math.max(
  	document.body.scrollHeight,
  	document.documentElement.scrollHeight,
  	document.body.offsetHeight,
  	document.documentElement.offsetHeight,
  	document.body.clientHeight,
  	document.documentElement.clientHeight
  );
  ```
- `window.pageYOffset` - current horizontal scroll position
- `window.pageXOffset` - current vertical scroll position

### **B. Scroll position methods**

- `window.scrollBy(x, y)` - scrolls the page relative to its current position
- `window.scrollTo(x, y)` - scrolls the page relative to the top-left corner
- `elem.scrollIntoView(top)` - scrolls the page to make `elem` visible
  - if `true`, makes `elem` visible at the top
  - if `false`, makes `elem` visible at the bottom
- Turning scrollbar on/off
  - `document.body.style.overflow = 'hidden'` - freezes scroll
  - `document.body.style.overflow = ''` - unfreezes scroll
