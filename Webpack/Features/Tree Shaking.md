# **Tree Shaking**

Tree shaking is a dead code elimination technique that eliminates unused exports.

It can only be applied on the ES module syntax since it relies on it static structure.

## **`usedExports`**

The `usedExports` option of the `optmization` property in `webpack.config.js` allows the exported identifiers to be renamed to be minified and allows unused exports to be tree shaken.

It is set to `true` by default in `production` mode.

```js
// webpack.config.js
module.exports = {
	mode: "development",
	optmization: {
		usedExports: true,
	},
};
```

### **Side Effect Annotation**

The `/*#__PURE__*/` annotation is used in JavaScript code to indicate that a function has no side effects and doesn't depend on any external state. This provides a hint to minifiers like Terser (used by webpack in production mode) to safely remove this code if it is not being used.

```js
// Identifier
const variable = /*#__PURE__*/ f();

// Function Call
/*#__PURE__*/ f();
```

## **`sideEffects`**

The `sideEffects` property of `package.json` removes the specified unused exports.

- `false` - removes all unused exports.
- `true` - considers all files to have side effects.
- `[]` - an array containing the files to exclude when tree shaking. It can contain strings that represent the file path and glob patterns.

Unlike `usedExports`, this property is used to specify the specific files to exclude when tree shaking.

```json
// package.json
{
	"sideEffects": false
}
```

When loaders are used in webpack, they process non-JavaScript files and transform them into JavaScript modules that are included in your final bundle. This means that all its unused variables will be subject to tree shaking.
