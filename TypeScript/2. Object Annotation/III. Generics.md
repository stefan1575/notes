# **Generics**

A generic is a placeholder type that is determined when the variable annotated with a generic is used as a type argument.

```ts
function identity<Type>(arg: Type): Type {
	return arg;
}

identity<string>("Hello");
```

## **I. Restricting type arguments**

When performing an operation on a generic type, it must be compatible to all the types or narrowed down by either using type guards or constraining its accepted types.

For instance, if we only intend to use arrays, we can annotate an array as a generic so that the `length` property can be accessed without type narrowing.

```ts
// Array<Type> can handle complex types and union types versus Type[]
function sayLength<Type>(arg: Array<Type>): Array<Type> {
	console.log(arg.length);
	return arg;
}
```

## **II. Generic Types**

### **A. Parameter/Return Type Annotation**

A functions parameter and return type can be annotated by its type argument as opposed to a fixed value.

```ts
function identity<Type>(arg: Type): Type {
	return arg;
}
```

### **B. Function Type Annotation**

A function's type can be directly annotated by using a function type or a call signature.

```ts
function identity<Type>(arg: Type): Type {
	return arg;
}

// (1) Generic Function Type Annotation
let myIdentity: <Type>(arg: Type) => Type = identity;

// (2) Object Method Type Annotation - Call Signature
let myIdentity: { <Type>(arg: Type): Type } = identity;
```

### **C. Interface Annotation**

An interface can be directly annotated as a generic when used as a type argument effectively locking in the type of the underlying call signature or we can annotate the call signature itself.

```ts
function identity<Type>(arg: Type): Type {
	return arg;
}

// (1) Call Signature Annotation
interface GenericIdentityFn {
	<Type>(arg: Type): Type;
}

let myIdentity: GenericIdentityFn = identity;

// (2) Generic Interface Annotation
interface GenericIdentityFn<Type> {
	(arg: Type): Type;
}

let myIdentity: GenericIdentityFn<number> = identity;
```

### **D. Class Annotation**

A class can be directly annotated as a generic. When used as constructor, it will gain the type of the generic type argument.

This is used to control the type of the created class instances.

```ts
class GenericType<Type> {
	varType: Type;
	add: (x: Type, y: Type) => Type;
}

let str = new GenericType<string>();

str.varType = "Hello";
str.add = function (x, y) {
	return x + " " + y;
};

console.log(str.add(str.varType, "World")); // Hello World
```

A class has two sides to its type, static and instance side. The type argument of a generic class only works for its instances and its static members may not use generic types however.

#### **Constructor Annotation**

When creating a factory function that uses a class's constructor, the class type should be annotated with its constructor denoted by "`new f(): Type`".

```ts
function factory<Type>(fn: new () => Type): Type {
	return new fn();
}
```

One usecase is to use one factory function for classes that use the same constructor.
