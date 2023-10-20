# **Template Literal Types**

The template literal type creates a new string literal type by concatenating the provided strings and interpolated type literals.

When a union type is interpolated within a template literal type, the resulting type is a union of every possible string literals that can be represented by each member.

For large string unions, it is generally recommended to calculate ahead of time (usually in a separate environment) since it is computationally expensive and slows down the compiler.

## **Generic Function with Type Literals**

Template literal types are useful when annotating a function that expects an `string` argument with a common format.

They can be used dynamically by creating a generic function type alias or call signature that contains a template literal type where the parameter and interpolated type is annotated with the same generic.

The interpolated types must be restricted to be a `string` since they return string literal types.

## **II. Template Utility Types**

- `Uppercase<StringType>` - converts the string to uppercase.
- `Lowercase<StringType>` - converts the string to lowercase.
- `Capitalize<StringType>` - converts the first character to uppercase.
- `Uncapitalize<StringType` - converts the first character to lowercase.
