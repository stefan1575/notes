# **Conditional Types**

A conditional type describes a type that is determined whether it extends `OtherType`. It helps us determine relation between the input type and the output type.

```ts
type ConditionalType = Type extends OtherType ? TrueType : FalseType;
```

When used alongside with generics, it describes the return value of a function depending whether the given type argument fulfills the condition of the conditional type.

```ts
type Plusable<T extends number | string> = T extends number ? number : string;

function StrOrNum<T extends number | string>(x: T, y: T): Plusable<T> {
	throw "unimplemented";
}

let num = StrOrNum(1, 2); // let num: number
let str = StrOrNum("Hello", "World"); // let str: string
```

The `FalseType` can be used to constrain the generic `Type`,
either as a default value or by using type extraction to return the excluded types.

### **`infer`**

The `infer` keyword declares a new generic type within the extends clause of a constrained conditional type which is inferred when the type argument passes.

```ts
type ExtractArray<Type> = Type extends Array<infer NewType> ? NewType : Type;
```

If the extended type isn't constrained, it is equivalent to `never`.

The inferred returned type with multiple call signatures is the last signature which is typically the most permissive case.

```ts
function overload(): string;
function overload(n: number): number;
function overload(n?: number): string | number {
	if (n !== undefined) {
		return 123;
	} else {
		return "string";
	}
}

// type TypeArgument: number
type TypeArgument = ReturnType<typeof overload>;
```

### **Distributive Conditional Types**

When providing a union type as a generic to conditional types, the type is `TrueType` is applied to all members of the union provided union.

```ts
type ToArray<Type> = Type extends any ? Type[] : never;

// type NumOrStrArr: ToArray<string> | ToArray<number>
type NumOrStrArr = ToArray<string | number>;
```

This behavior is typically desired. To avoid distributed conditional types, we should surround the extended type with square brackets.

```ts
type ToArray<Type> = Type extends [any] ? Type[] : never;

// type NumOrStrArr: (string | number)[]
type NumOrStrArr = ToArray<string | number>;
```
