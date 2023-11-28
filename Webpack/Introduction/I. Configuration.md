# **Using Module Bundlers**

## **I. Package Managers**

A package is a reusable piece of software which can be downloaded from a global registry into a developer's local environment.

A package manager automates the process of installing, updating, and resolving dependencies of the target package.

The Node Package Manager (npm) is a package manager with its own repository of software and CLI to install the packages.

### **`package.json`**

`package.json` is a configuration file that contains information about the project list of identifiers, entry point, test and build scripts, repository, dependencies, and host specs.

It is automatically created if the initializer in the `npm init` command is omitted.

- `main` - the primary entry point to your program relative to the root of your program, defaults to `index.js`.
- `dependencies` - an object containing the packages and version required to run the project.
- `devDependencies` - an object containing the packages and version required to test and build the project.

When installing packages with the corresponding dependency flag, they are automatically included in the `package.json` file.

```json
// package.json
{
	"name": "webpack-demo",
	"version": "1.0.0",
	"description": "",
	"main": "index.js",
	"scripts": {
		"test": "echo \"Error: no test specified\" && exit 1"
	},
	"keywords": [],
	"author": "",
	"license": "ISC"
}
```

### **npm flags**

- `--save` - adds the installed package to the `dependencies` section.
- `--saveDev` - adds the installed package to the `devDependencies` section.
- `--global` - installs the package globally on your system.
- `--save-exact` - installs the package with the specified version.
- `--watch` - automatically re-runs webpack each time that the source code changes.

## **II. Module Bundlers - webpack**

Module Bundlers are tools that combine multiple scripts and their dependencies into one single JavaScript file.

webpack is primarily used for bundling JavaScript files for browser usage, but it can also bundle HTML, CSS, transpile to the target environment, and compile preprocessors if the corresponding loaders are added.

## **III. `webpack.config.js`**

`webpack.config.js` is a webpack configuration file that describes how the source files should be bundled.

```js
// webpack.config.js
module.exports = {
	mode: "production",
	entry: "./index.js",
	output: {
		filename: "main.js",
		publicPath: "dist",
	},
};
```

- `mode` - contains a value describing the resulting bundle optimizations.
  - `production` - performs various optmizations like minification and tree shaking for deployment.
  - `development` - provides a faster build and adds more information to the output files for debugging purposes.
  - `none` - useful if you want to configure how your code is bundled.
- `entry` - the starting point where the module bundler will build its dependency graph. An object is used as its value if there are multiple entry points.
- `output` - contains an object describing the emitted bundle.
  - `filename` - the file name of the emitted bundle.
  - `publicPath` - the location of the emitted bundle.
- `module` - contains an object with an array of rules describing the file name and the corresponding loaders.
  - `test` - A condition (regex) that specifies which files are included in the rule.
  - `exclude`/`include` - A condition (regex) that specifies which files are included/excluded.
  - `use` - An array of objects describing the loader to use for the affected files. The loaders are executed in reverse array order.
    - `loader` - a string containing the loader.
    - `options` - an object describing the configurations options to the loaders. The available options are depend on the specific loader to use

### **Transpiling to new Features**

To use Babel alongside webpack, we need to install its compiler, the packages containing JavaScript features to transpile, and the corresponding loader for it to work with webpack.

```git
npm install @babel/core @babel/preset-env babel loader --save-dev
```

The JavaScript features to transpile is specified in the `presets` array of the `options` object.

```js
// webpack.config.js
module.exports = {
	module: {
		rules: [
			{
				test: /\.js$/,
				exclude: /node_modules/,
				use: {
					loader: "babel-loader",
					options: {
						presets: ["@babel/preset-env"],
					},
				},
			},
		],
	},
};
```

## **IV. Loading Assets**

### **A. CSS**

To import a css file, the `'css-loader'` needs to be loaded first before the `'style-loader'` in the `use` array.

```js
// webpack.config.js
module.exports = {
	module: {
		rules: [
			{
				test: /\.css$/i,
				use: ["style-loader", "css-loader"],
			},
		],
	},
};
```

### **B. Images and Fonts**

Instead of a loader for the image and font assets, we should give it a `type` of `"asset/resource"`.

```js
// webpack.config.js
module.exports = {
	module: {
		rules: [
			{
				test: /\.(png|svg|jpg|jpeg|gif)$/i,
				type: "asset/resource",
			},
			{
				test: /\.(woff|woff2|eot|ttf|otf)$/i,
				type: "asset/resource",
			},
		],
	},
};
```

The imported or linked image and font will be emitted to the output directory.

The image can be imported and consumed using the `src` attribute of an `<img>` element or linked in a css file using the `url` function.

```js
import Icon from "./icon.jpg";
const image = new Image();
image.src = Icon;
```

To use a font, we should set it as a `src` attribute's value with the `url` function in the `@font-face` declaration.

```css
@font-face {
	font-family: "MyFont";
	src: url("./my-font.woff") format("woff"), url("./my-font.woff2") format("woff2");
}

.element {
	font-family: "MyFont";
}
```

## **V. Output Management**

Multiple JavaScript file entry points are denoted by an object value in the `entry` property of `webpack.config.js`.

```js
// webpack.config.js
const path = require("path");

module.exports = {
	entry: {
		index: "./src/index.js",
		print: "./src/print.js",
	},

	output: {
		filename: "[name].bundle.js",
		path: path.resolve(__dirname, "dist"),
	},
};
```

### **Loading HTML**

The `html-webpack-plugin` is a plugin that generates an HTML file with dynamically added `<script>` tags that reference the output bundles in the `dist` folder.

The `HtmlWebpackPlugin` constructor accepts an `options` object which contains the following properties:

- `title` - the `<title>` of the emitted HTML file. It has to be set in the specified `template` instead if that property is present.
- `filename` - the name of the HTML file. It is `index.html` by default.
- `template` - the path to the template HTML file.
- `inject` - a value that specifies where the scripts should be included in the emitted HTML file.
  - `true`/`"body"` - The default behavior where the scripts are injected in the bottom of the `<body>` element.
  - `false` - scripts will not be automatically inserted
  - `"head"` - injects the scripts in the `<head>` element
- `hash` - if `true`, a unique hash will be added to all included scripts and CSS files. Useful for cache busting.
- `chunks` - only adds the specified chunks (the emitted scripts from the entry points) to include.
- `excludedChunks` - specifies which chunks to exclude.
- `scriptLoading` - by default the emitted scripts are deferred.
  - `"module"` - adds `type="module` to the emitted scripts.
  - `"blocking"` - removes the `defer` attribute.
- `favicon` - specifies the favicon for the emitted HTML file.

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
	entry: {
		index: "./src/index.js",
		print: "./src/print.js",
	},

	plugins: [
		new HtmlWebpackPlugin({
			template: "./src/index.html",
			hash: true,
		}),
	],

	output: {
		filename: "[name].bundle.js",
		path: path.resolve(__dirname, "dist"),
		clean: true,
	},
};
```

### **Cache Busting**

When a browser loads a website it often caches static files like JavaScript and CSS to improve load times for future visits. If you make changes to those files with the same file names, the browser may use the cached (outdated) version of those files.
