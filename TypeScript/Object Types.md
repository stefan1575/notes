# **Object Types**

An object can be annotated by an anonymous annotation, interface, or type alias.

### **Optional Properties**

If `strictNullChecks` is enabled, we have to perform a type check before accessing the optional property.

### **Parameter Destructuring**

In TypeScript, there is no way to annotate destructured parameters directly. The object itself has to be annotated.

## **I. `readonly`**

The `readonly` keyword asserts that a property can't be re-assigned during type-checking. The assignment must occur during declaration or creation of the constructor.

Although that the property itself can't be written, its internal contents can still be change if the value is mutable.

```ts
interface Admin {
	readonly admin: {
		name: string;
		age: number;
	};
}

function isAdmin(user: Admin) {
	console.log(user.admin);

	user.admin.age = 20;
	// Cannot assign to 'admin' because it is a read-only property.
	user.admin = "John";
}
```

Since TypeScript uses a structural type system, only the structure of an object is checked to determine if the shapes match regardless if the properties are `readonly`.

If an object has multiple references, `readonly` properties are still mutable if one of its references properties doesn't have the `readonly` keyword.

```ts
interface WritableUser {
	name: string;
	age: number;
}

interface ReadonlyUser {
	readonly name: string;
	readonly age: number;
}

let writableUser: WritableUser = {
	name: "Person McPersonface",
	age: 42,
};

let readonlyUser: ReadonlyUser = writableUser;

writableUser.name = "John";
console.log(readonlyUser.name); // "John"

// Cannot assign to 'name' because it is a read-only property.
readonlyUser.name = "Steve";
```

## **II. Index Signature**

An index signature to define the types for the dynamically created properties of an object. It may be also typed as `readonly`.

It is used if you don't the exact properties of an object during runtime, but the structure of its values.

The syntax for an index signature is an object containing "`[key: KeyType]: ValueType`" where the key can only contain `string`, `number`, `symbol`, and _template literal types_.

When an index signature is used, an object can only contain the given `KeyType` as the type of its keys and `ValueType` as the property values.

```ts
interface MixedDictionary = {
	[index: string]: string | number;
}

const user: MixedDictionary = {
	name: "John";
	age: 20;
};
```

If the dynamically created properties of an object can be potentially `undefined`, TypeScript will still infer it as its `ValueType` unless we include `undefined` as its union member.

### **String key coercion**

When a `string` key that contains a number is indexed as a `number`, it is coerced to a number in both JavaScript and TypeScript.

```ts
interface NumberKeys {
	[key: string]: string;
}

const obj: NumberKeys = {
	"1": "One",
	"2": "Two",
};

// Accessible as a number and string
obj["1"];
obj[1];
```

### **Multiple Index Signatures**

An object can be only indexed with both numbers and strings.

Since indexing a number gets coerced to a string, numeric indexes must return a subtype of the returned string indexer.

```ts
interface Users {
	name: string;
}

interface Admin extends Users {
	password: string;
}

interface Correct {
	[index: string]: Users;

	// The returned type of the numeric indexer is a subtype of the string indexer
	[index: number]: Admin;
}

interface Incorrect {
	[index: string]: Admin;

	// Numeric indexer returned type could potentially be different from the string indexer
	[index: number]: Users;
}
```

## **III. Excess Property Checks**

An object literal may only specify may only specify known properties so an excess property would result in an error.

We can bypass the check using _Type Assertion_ where we assert that the object is the same type "`Obj as TypeName`" or use an _Index Signature_, specifying the set of accepted property types.

### **Assigning the object to a variable**

As long as the object with the assigned variable has a common property with the checked type, the compiler won't give an error.

This technique is typically used for complex object literals that hold state, however the majority of excess property check errors are bugs.

Most of the time it is better to revise the type declaration if you intend to accept other properties.

## **IV. Extending Types**

An extended type holds other properties in addition to the defined type itself.

It is possible to extend interfaces and _intersection types_ (if you want to use a type alias) with multiple types.

```ts
interface X {
	x: number;
}
interface Y {
	y: number;
}

// interface
interface XY extends X, Y {}

// intersection type
type XY = X & Y;
```

## **V. Generic Object Types**

### **A. `unknown`**

A type safe alternative for `any` is `unknown` since it forces us to check the type before accessing the `unknown` typed property.

### **B. Generics**

An interface can be annotated as a generic where the type is known when it is used as _type argument_.

```ts
interface Box<Type> {
	contents: Type;
}

// (property) Box<string>.contents: string
let box: Box<string> = { contents: "Hello" };
```

A type alias annotated as a generic can be used if you want to describe more than just object types.

```ts
type BoxOrNull<Type> = Type | null;
```

### **C. Built-in Generics**

The `Array<T>`, `Map<K, V>`, `Set<T>`, and `Promise<T>` objects can be annotated as generics.

#### **`ReadonlyArray`**

An array can be asserted as read-only by either using the `ReadonlyArray` type or by using the `readonly` keyword depending whether we would want to annotate the read-only type with a generic.

## **VI. Tuple**

The tuple type is a an array where the type checker is aware of every element's type, position, and the array's length.

### **Optional Properties**

A tuple can only have optional properties at the end of the array.

The length of the array with the optional property is inferred as a union type containing the length with and without the property.

The optional property itself is inferred as the annotated type itself and `undefined`.

### **Rest Elements**

A tuple with rest elements has no set length, it is considered a `number` type instead of a literal type. Providing the rest elements is optional when using it as a type argument.

Rest elements are useful if we need a minimum amount of elements.

If a function uses rest parameters annotated as a tuple, we would have to create extra variables to unpack the values.

```ts
function createUser(...args: [string, number, ...string[]]) {
	const [name, age, details] = args;
	// ...
}
```

We can provide the parameters normally and annotate them with types while still using an unpacked tuple as an argument to avoid intermediate variables.

```ts
function createUser(name: string, age: number, ...details: string[]) {
	// ...
}

createUser(...["John", 20, "Male"]);
```

### **`readonly` Tuples**

It is good practice to annotate tuples as `readonly` since they tend to be created and left un-modified in most code.

Tuples with `const` assertions are also inferred as read-only.
