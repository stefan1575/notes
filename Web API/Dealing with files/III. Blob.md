# **Blob**

A `Blob` is a file-like object containing buffers, views, strings or other blobs which could be read as text or binary data.

```js
new Blob(blobParts, options);
```

- `blobParts` - an array which contains either blobs, buffers, and view objects, and strings.
- `options`
  - `type` - the MIME type that will be stored (i.e `image/png`)
  - `endings`
    - `transform` - Does not change the newline characters, the default value.
    - `native` - Transforms newline characters into `\r\n` for Windows or `\n` on Unix-based systems.

## **I. Blob to URL object**

### **URL: `createObjectURL`**

The `createObjectURL` method creates a unique string containing a URL referencing specified Blob object.

This allows us to download and upload `Blob` objects.

```js
URL.createObjectURL(object);
```

- `object` - The `Blob`, `File`, or `MediaSource` object.

It is kept in memory until document unload or by calling `revokeObjectURL`.

```js
const blob = new Blob(["Hi"], { type: "text/plain" });
link.href = URL.createObjectURL(blob);
URL.revokeObjectURL(blob);
```

In `fetch` requests, the `Content-Type` header is automatically generated.

## **II. Blob to `Base64`**

### **FileReader: `readAsDataURL`**

The `readAsDataURL` method encodes the given `Blob` or `File` object to a `Base64` string. Unlike `createObjectURL`, it is always kept in memory.

```js
const blob = new Blob(["Hi"], { type: "text/plain" });
const reader = new FileReader();
link.href = reader.readDataAsURL(blob);
```
