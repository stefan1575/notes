# **Operators**

## **I. `keyof`**

The `keyof` operator represents numeric or string union type of the annotated object's keys.

```ts
type Point = { x: number; y: number };
type XY = keyof Point; // "x" | "y"
```

### **Index Signatures**

In the case of an object with an index signature as one of its keys, it is represented as its annotated key type.

If the index signature's key is a `string` type, it becomes a union type of `string` and `number` since object keys are coerced to string in JavaScript.

```ts
type Arr = { [n: number]: unknown };
type Obj = { [k: string]: unknown };

type A = keyof Arr; // number
type O = keyof Obj; // string | number
```

## **II. `typeof`**

The `typeof` operator represents the type of a _identifier_ or their properties.

It is useful for getting the type of complex expressions that produce a literal value such as a function.

```ts
function identity(x: string): string {
	return `Hello ${x}`;
}

type IdentityType = ReturnType<typeof identity>;

// 'identity' refers to a value, but is being used as a type here. Did you mean 'typeof identity'?
type IdentityType = ReturnType<identity>;
```

### **Limitations**

The `typeof` operator can only be used on the names of variables and its properties.

```ts
let identity = "Hello";
let identityType = typeof identity;

let user = { name: "John", age: 20 };
let usernameType = typeof user.name;
```

This helps avoid the trap of writing code that is seemingly executing but is intended to be used as type annotation.

```ts
function sayHi() {
	return "Hi";
}

// typeof can only be used on identifiers and their properties
let identity: typeof sayHi();
```
