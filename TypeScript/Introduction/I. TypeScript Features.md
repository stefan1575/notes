# **TypeScript Features**

TypeScript is a statically typed superset of JavaScript. It extends JavaScript by adding type annotations which catches type-related errors on compile time instead of run-time.

## **I. Compiling TypeScript**

The `tsc` command compiles TypeScript code into JavaScript with removed type annotations.

The `noEmitOnError` compiler option stops the compiler from emitting JavaScript if there are errors in the TypeScript code.

```powershell
tsc --noEmitOnError script.ts
```

### **Downleveling**

By default, the TypeScript compiler transpiles the emitted JavaScript code to ES3.

The `target` compiler option specifies the JavaScript version where the emitted code should be transpiled.

```powershell
tsc --target es2015 script.ts
```

## **II. Types by Inference**

If a type isn't assigned, TypeScript will automatically generate the type when you create a variable with an assigned value.

```ts
// let helloWorld: string
let helloWorld = "Hello World";
```

It's best not to add type annotations when the type system would end up inferring the same type.

### **Anonymous and arrow functions**

The types of the parameters of anonymous and arrow functions are inferred by their context.

For instance, since the operation is performed in an array of strings in an anonymous function, the parameter type is inferred in the context of the operand.

```ts
// const names: string[]
const names = ["John", "James", "Jim"];

names.forEach(function (name) {
	console.log(name.toUpperCase());
});
```

### **Structural Type System**

TypeScript uses a structural type system which matches objects by shape rather than explicit type declarations.

The shape matching only requires a subset of the object's fields to match.

```ts
interface User {
	name: string;
	age: number;
}

function getUser(user: User) {
	console.log(user.name, user.age);
}

const user = {
	name: "Johnny",
	age: 25,
	sex: "Male",
};

// does not have the User interface
getUser(user);
```

## **III. Configuring strictness**

### **`noImplicitAny`**

If there are no type annotations and the type cannot be inferred, the type falls back as `any`.

The `noImplicitAny` option makes the compiler raise an error if a variable would implicitly take a type of `any`.

### **`strictNullChecks`**

By default, the values `null` and `undefined` are assignable to any other type without having to explictly define them as a type.

The `strictNullChecks` option makes the compiler raise an error if we try to access a variable that potentially `undefined` or `null` property without checking if it is defined first.
