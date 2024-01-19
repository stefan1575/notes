# **Mapped Types**

In computer science, the term "map" means transforming one thing to another or commonly refers to transforming a list of items into a different list of transformed items.

A mapped type is generic object type with an index signature where its key types are annotated with the given type argument in the generic.

The index signature's key types are retrieved using `keyof` with values that are typed with a union type of possible values.

When used as a type argument, the value types of the mapped type properties are changed to the union type of the provided mapped type.

```ts
type User = {
	username: string;
	password: string;
};

type ChangePermissions<Type> = {
	[Property in keyof Type]: boolean;
};

// type UserPermissions = {
//     username: boolean;
//     password: boolean;
// }
type UserPermissions = ChangePermissions<Type>;

const regularUser: UserPermissions = {
	username: false,
	password: true,
};
```

## **I. Mapping Modifiers**

There are two modifiers that can be applied during mapping which are "`readonly`" and "`?`" which can be removed by prefixing it with "`-`".

```ts
type Mutable<Type> = {
	-readonly [Property in keyof Type]: Type[Property];
};

type Locked = {
	readonly username: string;
	readonly password: string;
};

type Unlocked = Mutable<Locked>;
```

```ts
type Require<Type> = {
	[Property in keyof Type]-?: Type[Property];
};

type OptionalField = {
	name?: string;
	surname?: string;
};

type RequiredField = Require<OptionalField>;
```

## **II. Key Remapping via `as`**

The generic property type of the index signature in the mapped type can be re-mapped using the "`as`" keyword.

This can be used to create new property names that are derived from the provided generic type argument.

```ts
type Getters<Type> = {
	[Property in keyof Type as `get${Capitalize<
		string & Property
	>}`]: () => Type[Property];
};

interface User {
	name: string;
	surname: string;
}

type UserMethods = Getters<User>;
```

### **Filtering keys**

Keys can be filtered by assigning the undesired type with `never` in a conditional type.

```ts
type RemovePassword<Type> = {
	[Property in keyof Type as Exclude<Property, "password">]: Type[Property];
};

interface User {
	username: string;
	password: string;
	birthdate: string;
}

type PublicUser = RemovePassword<User>;
```
