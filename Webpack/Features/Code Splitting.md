# **Code Splitting**

Code splitting is the practice of splitting code into various bundles which can be loaded in demand or parallel. This allows an application to load the code it needs at a given point in time and load the other bundles on demand.

## **I. Entry Points**

If there are multiple entry points, each path is denoted by the `import` option of each entry point.

### **Preventing Duplication**

#### **A. `dependOn`**

The `dependOn` option of an entry point takes a string or an array of strings describing other entry points that are its dependencies. It loads the dependency first and shares the modules between each entry point to prevent duplicate imports.

```js
// webpack.config.js
const path = require("path");
module.exports = {
	entry: {
		index: {
			import: "./src/index.js",
			dependOn: "sharedModule",
		},
		module: {
			import: "./src/module.js",
			dependOn: "sharedModule",
		},
		sharedModule: "lodash",
	},
	output: {
		filename: "[name].bundle.js",
		path: path.resolve(__dirname, "dist"),
	},
	optimization: {
		runtimeChunk: "single",
	},
};
```

It is better to use an entry point with multiple imports when possible because webpack will perform better optimizations and guarantee execution order for scripts with `async` tags.

```js
// webpack.config.js
module.exports = {
	// entry point with multiple imports
	entry: {
		page: ["./src/index.js", "./src/module.js"],
	},
};
```

#### **B. `SplitChunksPlugin`**

The `SplitChunksPlugin` extracts the common dependencies between multiple entry points into an existing chunk or an entirely new chunk.

Duplicate imports are removed by if the `chunk` option of `optimization.splitChunks` is set to `"all"`.

```js
// webpack.config.js
module.exports = {
	optimization: {
		splitChunks: {
			chunks: "all",
		},
	},
};
```
