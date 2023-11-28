# **Production Mode**

In `production` mode, webpack optimizes the build for performance and size. It provides features like minification and tree shaking.

Using source maps in `production` mode is useful if we want to debug and run benchmark tests. Only certain source maps can be used in production.

## **I. `webpack-merge`**

The `webpack-merge` package provides a `merge` function that concatenates arrays and merges objects into a new object. If functions are encountered, they are executed and the return values are used in place of the function itself.

This is particularly useful for when merging configuration objects, such as individual settings for `development`, `production`, and a shared configuration in webpack.

```js
// webpack.common.js
const path = require("path");

module.exports = {
	entry: {
		app: "./src/index.js",
	},
	output: {
		path: path.resolve(__dirname, "dist"),
		clean: true,
	},
};
```

```js
// webpack.dev.js
const { merge } = require("webpack-merge");
const common = require("./webpack.common.js");

module.exports = merge(common, {
	mode: "development",
	devtool: "inline-source-map",
	devServer: {
		static: "./dist",
	},
});
```

```js
// webpack.prod.js
const { merge } = require("webpack-merge");
const common = require("./webpack.common.js");

module.exports = merge(common, {
	mode: "production",
});
```

## **II. Build Scripts**

To run the build with the specified mode, we should add the `config` flag with the configuration file that we want to run.

```json
// package.json
{
	"scripts": {
		"start": "webpack serve --open --config webpack.dev.js",
		"build": "webpack --config webpack.prod.js"
	}
}
```

## **III. Minification**

Webpack v4 will minify your code by default in `production` mode using `TerserPlugin`.

We can provide another minification plugin in the `optimization.minimizer` array.
