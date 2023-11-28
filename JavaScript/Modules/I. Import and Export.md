# **Import and Export**

## **I. Modules**

A module is a file that contains code such as a variable, function, or object to perform a specific task.

They are used to organize and structure codebases to break down larger programs into smaller and more manageable independent chunks that carry single or multiple related tasks.

To create a JavaScript module, a script must have a `type` of `module`:

```html
<script src="script.js" type="module"></script>
```

## **II. `export` and `import`**

### **A. `export`**

The `export` declaration allows a variable, function, or object to be used in another script.

```js
// (1) export before declaration
export function sayHi() {
	console.log("Hello");
}

// (2) export an object containing variables
export { sayHi };
```

### **B. `import`**

The `import` declaration allows a variable, function or object to be used in our own script.

```js
// (1) import specific variables
import {sayHi} from './module.js';

// (2) import all variables as a single object
import * from as object from './module.js';
object.sayHi(); // hello
```

### **C. `as`**

The `as` keyword is used to provide an alias to the imported and exported variable.

```js
// changing export name
export { sayHi as hi };

// changing import name
import { hi as hello } from "./module.js";
```

### **D. Default exports**

A default export is used to export a single variable to be used in another script.

```js
// (1) default export
export default function sayHi() {
	console.log("Hello");
}

// (2) using default as an alias
export {sayHi as default}
```

Although you can choose your own variable name for default exports, it should correspond to the file name to keep the code consistent:

```js
// any varible name is accepted
import module from "./module.js";
```

If there are both named and default exports in a module, we should give the `default` keyword an alias inside the object containing the variables to import.

```js
import { default as module, sayHi } from "./module.js";
```

## **III. Re-import/export**

The `export..from` syntax allows us to import and immediately re-export variables.

This is used to hide the internal structure of the package and use one file instead of searching for others inside the package folder.

```js
// 📁 ./modules/sayHi.js
export function sayHi() {
	console.log("Hi");
}

// 📁 ./modules/sayBye.js
export function sayBye() {
	console.log("Bye");
}

// 📁 ./modules/main.js
import sayHi from "/modules/sayHi.js";
import sayBye from "/modules/sayBye.js";

export { sayHi } from "/modules/sayHi.js";
export { sayBye } from "/modules/sayBye.js";
```

If there are both named and default exports, they need to be re-exported separately.

```js
// 📁 ./modules/greet.js
export default function sayHi() {
	console.log("Hi");
}

export function sayHey() {
    console.log("Hey");
}

export function sayBye() {
    console.log("Bye");
}

// 📁 ./modules/main.js
import sayHi from "/modules/sayHi.js";
import * from "/modules/sayBye.js";

export { * } from "/modules/sayHi.js"; // imports all named exports
export { default } from "/modules/sayHi.js";
```

## **IV. Empty `import` and `export`**

### **Empty `export`**

When a script has an empty `export {}`, it will still execute in the order of an HTML document where it is defined. However the other scripts won't have access to the variables of the file with an empty export since modules have their own scope.

This is used to avoid naming conflicts and to clearly see where each identifier is coming from.

### **Empty `import`**

You won't be able to access any variables when importing a file without any variables. However since the imported file will be evaluated, any code at that is not encapsulated inside a function or variable will be executed as a side effect.
