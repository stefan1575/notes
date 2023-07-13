# **Mixins**

A mixin is a class that contains methods and properties from other objects through copying or merging the class to the source objects.

For instance, we can copy another objects methods to the class' `prototype` property:

```js
class SetName {
	constructor(name, surname) {
		this.name = name;
		this.surname = surname;
	}
}

let nameMethods = {
	showName() {
		return console.log(`${this.name} ${this.surname}`);
	},
	rename(newName, newSurname) {
		this.newName = name;
		this.newSurname = surname;
		return this;
	},
};

Object.assign(SetName.prototype, nameMethods);

let john = new SetName("John", "Smith");
john.showName(); // John Smith
```
