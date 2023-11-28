# **Development Mode**

In `development` mode, webpack optimizes the build for development speed with hot module replacement and debugging with source maps.

## **I. Source Maps**

A source map provides a way of mapping the code of the compressed file to its original position in the source file.

This allows us to view the original source code in the developer tools when the browser is running instead of the bundled code.

The `devtool` property of the `module.exports` object determines how the source map is generated. Its value describes a trade-off between build speed and the quality of the source map.

- `"source-map"` - Emits a full source map as a separate file, higher quality but slower build speed.
- `"cheap-source-map"` - Emits a source map that only maps line numbers.
- `"inline-source-map"` - Instead of a separate file, the source map is included inline in the generated bundle
- `"nosources-source-map"` - Used to map stack traces on the client without exposing all the source code.
- `"eval"` - Yields faster builds but has lower quality source maps.

## **II. Development Tools**

### **1. Watch Mode**

Webpack's watch mode automatically recompiles the changed files that are in the dependency graph.

When a file is saved, the command in the `"watch"` property in the `"scripts"` object is automatically executed.

```json
// package.json
{
	"scripts": {
		"watch": "webpack --watch"
	}
}
```

### **2. `webpack-dev-server`**

The `webpack-dev-server` package automatically recompiles changed files in the dependency graph and serves it in a web browser which reloads with the updated code.

Its behavior can be configured using the `devServer` property of the `module.exports` object.

- `static` - a string representing the directory from which static files will be served. By default, it is set to the same directory as `webpack.config.js`.
- `watchFiles` - an array of strings describing the directories/files to watch for changes. By default, the `webpack-dev-server` only watches the changes in files that are part of the dependency graph created by the specified entry points.

```js
// webpack.config.js
module.exports = {
	devServer: {
		static: "./dist",
		watchFiles: ["src/*.html"],
	},
};
```

If we want to run a bundle with multiple entry points, `optimization.runtimeChunk` must be set to `"single"`.

```js
// webpack.config.js
module.exports = {
	optimization: {
		runtimeChunk: "single",
	},
};
```

The `webpack serve --open` command starts a development server and opens your default web browser to preview the application.

```json
// package.json
{
	"scripts": {
		"start": "webpack serve --open",
		"build": "webpack"
	}
}
```

### **3. `webpack-dev-middleware`**

The `webpack-dev-middleware` package allows us to use our own custom Express.js server if we need more control over our server setup.
