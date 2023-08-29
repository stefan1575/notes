# **`TextDecoder` and `TextEncoder`**

## **I. `TextDecoder`**

The `TextDecoder` constructor creates an object for decoding binary with the given encoding.

```js
const decoder = new TextDecoder([label], [options]);
```

- `label` - the encoding, `utf-8` by default.
- `options`
  - `fatal` - if `true`, throws a `TypeError` when decoding invalid characters.
  - `ignoreBOM` - if `true`, ignores byte order mark.

### **`decode`**

The `decode` method returns a string containing the decoded text from the buffer.

```js
const str = decoder.decode([input], [options]);
```

- `input` - The buffer or view object to decode.
- `options`
  - `stream` - if `true`, it decoder will remember unfinished characters from the previous chunk and decode them in the next chunk.

## **II. `TextEncoder`**

The `TextEncoder` constructor creates an object for encoding strings in `UTF-8`.

```js
const encoder = new TextEncoder();
```

- `encode(str)` - returns a `Uint8Array` object containing the encoded string
- `encodeInto(str, Uint8Array)` - puts the encoded string in the given `Uint8Array` object.
