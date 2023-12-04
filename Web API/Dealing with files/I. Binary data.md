# **Binary data**

Binary data refers to data that is represented in machine readable format composed of ones and zeroes.

It is usually encountered in JavaScript when dealing with files and image processing.

## **I. `ArrayBuffer`**

The `ArrayBuffer` object is a fixed-length buffer that stores binary data.

We can create our own buffer with a specified amount of bytes pre-filled with zeroes:

```js
const buffer = new ArrayBuffer(byteLength);
```

## **II. View Objects**

View objects are interfaces that allow us to read and write buffers.

### **A. `TypedArray`**

`TypedArray` objects describe an array-like view of the given buffer in the specified bit format and integer type.

- `Uint8Array` - Each element of the view object consumes a byte, values range from 0 to 255.
  - `Int8Array` - Values can be negative, ranges from -128 to 127.
  - `Uint8ClampedArray` - Negative values are converted to 0 and values greater than 255 are converted to 255.
- `Uint16Array` - Consumes 2 bytes, values range from 0 to 65535.
- `Uint32Array` - Consumes 3 bytes, values range from 0 to 4294967295.
- `Float64Array` - Consumes 4 bytes, with values range from `5.0x10`<sup>`-324`</sup> to `1.8x10`<sup>`308`</sup>.

```js
new Uint8Array(buffer, [byteOffset], [length]);
```

- `byteOffset` - The starting point in bytes of the given buffer to reference.
- `length` - The amount of elements in the view to reference.

Buffers can also be created using `TypedArray` objects:

```js
// (1) Converts array-like to the specified typed array
new Uint8Array(object);

// (2) Converts old typed array values to the new type
new Uint8Array(typedArray);

// (3) Typed array with specified length
new Uint8Array(length);

// (4) Typed array with zero length
new Uint8Array();
```

#### **`TypedArray` methods**

- `typedArray.set(arrayLike, [offset])` - Stores multiple values inside the `TypedArray` object with the given `offset`.
- `typedArray.subarray([start], [end])` - Creates a new view from the `TypedArray` object with the given starting and ending offset where it is exclusive.

### **B. `DataView`**

The `DataView` object describes an array-like view of the given buffer.

```js
new DataView(buffer, [byteOffset], [byteLength]);
```

- `byteOffset` - The starting point of the given buffer to reference.
- `byteLength` - The amount of elements in the byte array to reference.

Unlike `TypedArray` objects, the bit format and integer type is chosen at call time:

```js
const buffer = new ArrayBuffer(10);
const view = new DataView(buffer);

view.setUint8(0, 5);
view.getUint8(0); // 5
```
