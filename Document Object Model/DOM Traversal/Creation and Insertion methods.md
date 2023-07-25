# **DOM Modication Methods**

## **I. Creation methods**

- `document.createElement('tag')` - creates element nodes with the given tag
- `document.createTextNode('text')` - creates text nodes with the given text

## **II. Insertion methods**

![Element: Insertion methods](/assets/element-insertion-methods.png)

- `elem.append(node)` - inserts nodes or strings after the last child of `elem`
- `elem.prepend(node)` - inserts nodes or strings before the first child of `elem`
- `elem.before(node)` - inserts nodes or strings at before the beginning of `elem`
- `elem.after(node)` - inserts nodes or strings at the end of `elem`
- `elem.replaceWith(node)` - replaces `elem` with the given nodes or strings

### **`insertAdjacentHTML`/`Text`/`Element`**

- `elem.insertAdjacentHTML(position, HTML)` - inserts HTML based on `position` relative to `elem`.

  - `afterBegin` = `prepend`
  - `beforeEnd` = `append`
  - `beforeBegin` = `before`
  - `afterEnd` = `after`

## **III. Element removal**

- `elem.remove()` - removes `elem` from the DOM.

### **Removing children**

When removing elements by iterating over an index, the call to `remove()` shifts _collections_ which means that elements would be skipped as the index increases.

Instead, we should either properties that don't rely on indexing or clear the contents of `innerHTML` to remove all its children:

```js
clearChildren(element) {
  while (element.firstChild) {
    elem.firstChild.remove();
  }
}

// or
clearHTML(element) {
  element.innerHTML = "";
}
```

## **IV. Cloning Nodes**

- `node.cloneNode(deep)` - returns a duplicate of the node, if `deep` is `true` returns the node and its children.

## **V. `DocumentFragment`**

The syntax is:

```js
new DocumentFragment();
```
