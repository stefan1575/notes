# **Defining Types**

## **Primitives**

- `any` - value is not type checked
- `string` - string values
- `number` - any integer or floating point number
- `bigint` - very large integers
- `boolean` - the values `true` and `false`
- `symbol` - unique primitive value

### **Objects**

- `Array` - an array of values
  - `TypeName[]` - an array of the specified type or interface

## **I. Type Annotations**

To explicitly define a type annotation we should append `: TypeName` after the variable we want to annotate.

```ts
const hello: string = "Hello World";
```

### **A. Function Annotations**

- **Parameter Type Annotation**

```ts
function sayHi(user: string) {}
```

- **Return Type Annotation**

```ts
function sayHi(): string {}
```

### **B. Object Annotations**

To define an object type, we should list its properties and type.

```ts
function sayHi(user: { name: string }) {}
```

#### **Optional Properties**

We can specify that object's property is optional by adding `?` after the property name.

```ts
function sayHi(user: { name: string; surname?: string }) {}
```

If we try to access the result of an operation performed on the optional property, an error is raised if the value is potentially `undefined`.

```ts
function sayHi(user: { name: string; surname?: string }) {
	// 'user.surname' possibly 'undefined'
	console.log(user.surname.toUpperCase());
}
```

To prevent the compiler from raising an error with an optional property, we should either check if the value is `defined` or use optional chaining.

###

## **II. Type Assertions**

TypeScript isn't always able to specifically determine the type of the value when it can't be inferred.

To specifically or less specifically determine a type, we can use the `as` keyword or the angle-bracket syntax.

```ts
// (1) as TypeName
const link = document.getElementById("link") as HTMLAnchorElement;

// (2) Angle-bracket syntax
const link = <HTMLAnchorElement>document.getElementById("link");
```

### **Literal Types**

In addition to the `string` and `number` types, we can refer to specific numbers and strings.

```ts
let letStr = "Hello";

const constStr = "Hello";
```

When an object is initialized with properties of literal types, they are inferred to be the general `string` and `number` types.

To infer an object property as a literal type, we should add a literal type assertion in either the parameter or argument, or convert the entire object to use type literals.

```ts
declare function sayHi(name: string, gender: "Male" | "Female"): void;

// (1) Literal type assertion - Parameter or Argument
const user = { name: "John", gender: "Male" as "Male" };
sayHi(user.name, user.gender as "Male");

// (2) Converting object to type literal
const user = { name: "John", gender: "Male" } as const;
sayHi(user.name, user.gender);
```

### **`null` and `undefined`**

When the `strictNullChecks` option is enabled, we need explicitly define `null` and `undefined` as the value's type and test if it's not `null` or `undefined` before using methods or properties on that value.

The non-null assertion operator `"!"` asserts that the operand isn't `null` or `undefined`.

```ts
// strictNullChecks enabled
function sayHi(user: { name: string; surname?: string }) {
	console.log(user.surname!.toUpperCase());
}
```

Since it doesn't change the runtime behavior of your code, it should only be used if we are certain that the value can't be `null` or `undefined`.

## **III. Composing Types**

### **A. Unions**

A union is a type annotation with multiple values denoted by `"|"` which separates each type.

They are usually used to specify the allowed set of `string` or `number` literal types.

```ts
type gender = "male" | "female";
```

When performing an operation on a union type, it has to be valid for every member of the union.

An operation has to be performed on each type if they all don't share any common methods.

### **B. Generics**

A generic is a placeholder type that is determined when it is used as a type argument.
