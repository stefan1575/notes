# **Indexed Type Access**

The indexed access type denoted by "`TypeName["KeyName"]`" represents the type of another typed object or interface's key.

```ts
type User = { name: string; age: number; isAdmin: boolean };
type Name = User["name"]; // string
```

Since it is a type itself, it can be used as a union type member, operators, interfaces, and other types.

```ts
type User = { name: string; age: number; isAdmin: boolean };
type Union = User["name" | "age"]; // string | number
```

The indexed access type annotation only accepts types as its type argument.

You should use the `typeof` operator or a type alias instead of a variable value since using the latter would result in an error.

### **`ArrayType[number]`**

Normally, you would use specific string or numeric literals as the key in indexed type access.

An array can be indexed with `number` where it is interpreted as its possible property types.

```ts
const UserList = [
	{ name: "John", age: 20 },
	{ name: "Tom", age: 25 },
];

type UserType = [{ name: string; age: number }];

// Variable Reference
type VarType = (typeof UserList)[number];

// Type Alias
type IndexType = UserType[number];
```
