# **Function Types**

## **I. Function Type Annotations**

The parameter of the function type becomes `any` if it isn't specified.

### **A. Function Type Expressions**

Useful for single use function annotations.

```ts
function greetFn(fn: (param: string) => void) {
	fn("Hello World");
}
```

### **B. Function Type Alias**

Useful when you plan on using the same type multiple times.

```ts
type fn = (param: string) => void;
```

### **C. Call Signatures**

Useful for adding properties to a callback in addition to specifying its parameters.

```ts
type fnWithProperty = {
	property: string;
	(param: string): string; // (ParameterName: Type): ReturnValue: Type
};

function executeFn(fn: fnWithProperty) {
	console.log(fn.property); // "Value"
	console.log(fn("Hello World")); // "Hello World"
}

function fn(param) {
	return param;
}

fn.property = "Value";
executeFn(fn);
```

## **II. Constructor Type Annotation**

Constructors can be annotated with type expressions, aliases, and call signatures by simply prepending `new` to the type.

Some constructors can called without `new` such as the `Date` object. It is possible to combine call and construct signatures in the same type.

```ts
interface CallOrConstruct {
    new (param: string): Date;
    (param?: string):
}
```

## **III. Generic Function Annotation**

If the return value of the function is based on its argument type, it will have the same type.

The Generic Function annotation allows the function's parameter to represent its argument type as opposed to specifying a fixed set of values.

It is annotated with "`<Type>`" after the function name, where `Type` is a placeholder that will be replaced by the actual type when the function is called.

```ts
function generic<Type>(arr: Type[]): Type {
	return arr[1];
}

// const str: string
const str = generic(["Alpha", "Beta", "Charlie"]);

// const numb: number
const numb = generic([0, 1, 2]);

// const strNum: string | number
const strNum = generic(["Zero", 0]);
```

You can specify multiple type parameters in a generic function if your function has multiple arguments.

### **Constrained Parameters**

To restrict the accepted arguments of a function, we should extend the generic type parameter with another type or interface.

```ts
interface hasLength {
	length: number;
}

function measureLength<Type extends hasLength>(value: Type): number {
	return value.length;
}

const str = measureLength("John");
const arr = measureLength([1, 2, 3]);
// Argument of type 'number' is not assignable to parameter of type 'hasLength'
const num = measureLength(1);
```

If the constrained generic function returns the same `Type` as its argument, the return value must be the value as its argument and not some value matching its constraint.

#### **Type Parameter Subtype Constraint**

We can constrain another type of a parameter as a subtype of another parameter if we are accessing one of its members.

This is used to ensure that we are not accidentally grabbing a member that doesn't exist on the target `Type`.

```ts
function getProperty<Type, SubType extends keyof Type>(
	obj: Type,
	key: SubType
) {
	return obj[key];
}
let user = {
	name: "John",
	age: 20,
};

getProperty(user, "name");

// Argument of type '"gender"' is not assignable to parameter of type '"name" | "age"'.
getProperty(user, "gender");
```

#### **Specifying `Type` on function call**

If the constrained `Type` is used with multiple arguments, it will gain the type of the first used argument.

We can manually specify the generic types in the function call if we intend to use multiple types with the same interface.

```ts
function concat<Type>(arr1: Type[], arr2: Type[]): any[] {
	return [...arr1, ...arr2];
}

const numbers = [1, 2, 3];
const strings = ["a", "b", "c"];

const numAndStr = concat<number | string>(numbers, strings);
```

## **IV. Optional Parameters**

We can denote that the parameter is optional by appending "`?`" after the parameter.

Unless we specify a default parameter, its function call will become `undefined`.

```ts
function fn(x?: number) {
	console.(typeof x);
}
log
const arg = fn(3); // number
const noArg = fn(); // undefined
```

### **Optional Parameters in Callbacks**

Although JavaScript simply ignores unused parameters, we have to check if the optional parameter is not `undefined` before performing an operation.

### **Optional Generic Parameter**

A generic parameter can also be optional.

If a parameter has a default value, it is inferred as an optional parameter.

## **V. Function Overloading**

A Function Overload refers to the ability to create multiple functions with the same name but with different implementations discriminated by the number of provided arguments.

It isn't supported natively in JavaScript but it could be simulated by `if..else` statements and annotating the corresponding _overload signature_ for each case.

```ts
function createDate(): Date;
function createDate(m: number, d: number, y: number): Date;
function createDate(m?: number, d?: number, y?: number): Date {
	if (m !== undefined && d !== undefined && y !== undefined) {
		return new Date(m, d, y);
	} else {
		return new Date();
	}
}

const currentDate = createDate();
const newYear = createDate(11, 25, 2000);
// No overload expects 1 arguments, but overloads do exist that expect either 0 or 3 arguments.
const date = createDate(1, 25);
```

The overload signature must match the _implementation signature_ which is the types of the parameters and return value.

## **VI. `this`**

The value of `this` is inferred based on the outer object.

If we want more control of what `this` represents, In TypeScript, we can specify the type of `this` inside the parameter of a function.

## **VII. Function Types**

- `void` - inferred when there is no a return value.
  - In JavaScript, a `void` function returns `undefined.`
  - When returning a value of in a function that has an anonymous void function type, the return value is still `void`.
- `object` - a value that isn't a primitive.
- `unknown` - type safe counterpart of `any`.
  - Variables with the `unknown` type are only assignable to `any` and itself without type assertion or narrowing of the `unknown` variable.
- `never` - represents values that never occur.
  - It is only reachable when a union type is exhausted or a function that never returns.
- `Function` - assignable to any function or instances of the JavaScript `Function` object.
  - a function call that has the type `Function` has a return type of `any`.

## **VIII. Rest Parameters, Arguments and Destructuring**

### **Rest Parameters**

When using rest parameters without type annotation, they implicitly gain the type of `any[]`.

### **Rest Arguments**

Array literals are inferred as `type[]`, where the it has no information about the amount of values.

If we are providing rest arguments to a function that expects a certain number of values, we should make it immutable by converting it to a tuple by using the type assertion `as const`.

```ts
const tuple = [2, 3] as const;
Math.pow(...tuple);
```

If we are targeting older runtimes, we should turn on `downlevelIteration` in the `tsconfig.json` file.

### **Parameter Destructuring**

A function that uses parameter destructuring is annotated by using an object containing the parameter values and the annotated types.

```ts
// (1) Object literal
function sum({ x, y }: { x: number; y: number }) {
	return x + y;
}

// (2) Named type
type Tuple = {
	x: number;
	y: number;
};

function sum({ x, y }: Tuple) {
	return x + y;
}
```

A destructured parameter can have a default parameter as well.
