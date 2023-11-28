# **Dynamic imports**

The `import()` syntax loads a module dynamically and returns a promise with a value containing a module object that resolves when it is done loading.

```js
import(moduleName);
```

Unlike opposed to static imports which are evaluated at the beginning, loaded and executed immediately, they do not require the `type="module"` attribute.

The returned promise object could be consumed with `then`, `catch` or the `async await` syntax.

```js
// 📁 ./modules/greet.js
export function sayHi() {
	console.log("Hi");
}

export function sayBye() {
	console.log("Bye");
}

export default function sayHey() {
	console.log("Hey");
}

// 📁 ./modules/main.js
// Promise chaining
import("./modules/greet.js").then((moduleObject) => {
	moduleObject.sayHi();
	moduleObject.sayBye();
	module.default();
});

// async await
async function awaitImport() {
	const moduleObject = await import("./modules/greet.js");
	module.sayHi();
	module.sayBye();
	module.default();
}
```
