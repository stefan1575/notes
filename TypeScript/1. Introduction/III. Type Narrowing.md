# **Type Narrowing**

Type narrowing is the process of narrowing down a broader type into a more specific type.

### **Control Flow Analysis**

TypeScript uses Control Flow Analysis to infer the variable's type based on the possible values of the current execution path.

```ts
function scope() {
	let x: string | number | boolean;

	x = true; // let x: boolean

	if (Math.random() < 0.5) {
		x = "Hello World"; // let x: string
	} else {
		x = 1; // let x: number
	}

	return x; // let x: string | number
}
```

## **I. Type Guards**

Type guards are built-in functions that check if a value conforms to a specified type and returns a boolean result.

#### **User-Defined Type Guard**

To create a type guard, we should create a function that returns a type predicate denoted by "`: parameterName is Type`".

```ts
type Registered = { name: string };
type Guest = { name: "Guest" };

function isRegistered(user: Registered | Guest): user is Registered {
	return (user as Registered).name !== "Guest";
}
```

When the type is checked in a user-defined type guard, it also determines the type using control flow analysis.

```ts
declare function getUser(): Registered | Guest;
let user = getUser();

if (isRegistered(user)) {
	console.log(`Welcome back, ${user.name}`); // let user: Registered
} else {
	console.log(`Welcome back, ${user.name}`); // let user: Guest
}
```

### **A. `typeof`**

The `typeof` operator returns a string which indicates the operand's value.

TypeScript is aware of how `typeof` operates on different values.

For instance, comparing a value to the string `"object"`. Since comparing both `null` and `object` may return the string `"object"`.

```ts
function sayHi(user: Array<any> | null) {
	if (typeof user === "object") {
		// 'user' is possibly 'null'
		for (let prop of user) {
			console.log(prop);
		}
	}
}
```

### **B. `in`**

The `in` operator checks if a property belongs in an object's prototype chain to guard against `undefined` properties and methods.

### **C. `instanceof`**

The `instanceof` operator checks if an object's `prototype` property has the specified constructor.

This can be used to guarantee that an object belongs to a certain class to guarantee access from its properties and methods.

## **II. Truthiness Narrowing**

Truthiness narrowing is the process of coercing a value to a boolean through conditional statements or direct coercion using `Boolean`/`"!!"`.

If we want to type guard against both `null` and `undefined`, we can compare use the equality operator "`==`" and compare it with either `null` or `undefined`.

## **III. Equality Narrowing**

Equality narrowing uses equality checks and `switch` statements to narrow down types either with variables or literal types.

## **IV. Discriminated Unions**

A discriminated union is a union type containing multiple _object types_ with one common property.

Each common property of the union's members have different values which allows TypeScript to distinguish each type.

```ts
interface Loading {
	state: "loading";
}

interface Failed {
	state: "failed";
	errorCode: number;
}

interface Success {
	state: "success";
}

type LoadingState = Loading | Failed | Success;

function logState(state: LoadingState): string {
	switch (state.state) {
		case "loading":
			return state.state; // state: Loading
		case "failed":
			return state.state; // state: Failed
		case "success":
			return state.state; // state: Success
	}
}
```

## **V. Exhaustiveness Checking**

The `never` type represents values that never occur.

It is naturally encountered as a return type for a function that never returns (infinite loop or error thrown) and a variable type in a type guard that can never be true.

It can only be explicitly assigned as a type member or the last type after all other non-never types have been narrowed in a union type.

```ts
function logState(state: LoadingState): string {
	switch (state.state) {
		case "loading":
			return state.state; // state: Loading
		case "failed":
			return state.state; // state: Failed
		case "success":
			return state.state; // state: Success
		default:
			const _exhaustiveCheck: never = state; // exhaustiveCheck: never
			return _exhaustiveCheck;
	}
}
```

An TypeScript error will occur if all types aren't exhausted before attempting to assign `never`.

```ts
type LoadingState = Loading | Failed | Success;

function logState(state: LoadingState): string {
	switch (state.state) {
		case "failed":
			return state.state; // state: Failed
		case "success":
			return state.state; // state: Success
		default:
			const _exhaustiveCheck: never = state; // Type 'Loading' is not assignable to 'never'
			return _exhaustiveCheck;
	}
}
```
