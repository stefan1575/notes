# **Fetch**

The `fetch` method fetches a resource from the specified URL. It returns a promise that is fulfilled with a value of a `Response` object.

The returned promise is rejected when it's unable to make an HTTP request.

The syntax is:

```js
const promise = fetch(url, { options });
```

- `url` - the specified URL to access.
- `options` - an object containing custom settings that you want to apply to the request.
