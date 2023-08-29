# **`File` and `FileReader`**

## **I. `File`**

The `File` object is a subclass of `Blob` that represents a file on the user's device.

It is typically retrieved from a `FileList` object from an `<input type="file">` element.

```js
const file = new File(fileParts, fileName, [options]);
```

- `fileParts` - an array which contains either blobs, buffers, and view objects, and strings.
- `fileName` - the file name string.
- `options`
  - `type` - a string containing the MIME type.
  - `lastModified` - the timestamp of the last modification.

## **II. `FileReader`**

The `FileReader` constructor creates an object that asynchronously reads `File` and `Blob` objects.

```js
const reader = new FileReader();
```

The following asynchronous methods trigger the `load` event upon completion:

- `readAsText(blob, [encoding])` - converts the `Blob` to the specified encoding, UTF-8 by default.
- `readAsDataURL(blob)` - converts the `Blob` to a `Base64` string.
- `readAsArrayBuffer(blob)` - converts the `Blob` to an `ArrayBuffer`.

### **FileReader: `result`**

The `result` property is returns the converted `Blob` object. It is only available after the `load` event has been triggered by one of the instance methods.

```js
const reader = new FileReader();
const file = document.querySelector('input[type="file"]');
const image = document.querySelector("image");

document.onload = function () {
	image.src = reader.result();
};

reader.readAsDataURL(file.files[0]);
```

If the `Blob` cannot be read, the `FileReader` object returns `error` instead.
