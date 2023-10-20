# **Classes**

## **I. Class Members**

### **A. Fields**

When a class field is declared without an assigned value, the `any` type is inferred.

They will gain only gain a type when they a value initialized through inference or when the field itself is annotated.

```ts
class MaleUser {
	// Typing through annotation and inference
	name: string;
	gender = "Male";
}
```

#### **`--strictPropertyInitialization`**

By default, class fields can be defined without an assigned value.

The `strictPropertyInitialization` enforces that a class property needs to be defined either through a constructor or initializer.

Initialization at the class constructor is necessary when the property relies on its parameters which are only available at the time of object creation.

If a class property is defined outside of a constructor or intializer, we can use the _definite assignment assertion operator_ "`!`" after the property to assert that the property is already initialized.

#### **`readonly`**

In class properties, the `readonly` modifier is prefixed before the class field.

### **B. Constructors**

Constructors are simlar to regular functions where we can add parameters with type annotations, default values, and overloads.

They cannot be annotated as a generic or have return type annotations since it is always returned as its class instance type. Only the outer class declaration can be annotated as a generic.

### **C. Methods**

Class methods can use all the same type annotations as a function.

Inside a method body, it is necessary to access its own class fields and other methods with the `this` keyword. Otherwise it will reference try to reference variables the scope outside of the class itself.

### **D. Getters and Setters**

If a `get` exists but no `set`, the returned property is inferred as `readonly`.

If the setter parameter type isn't annotated, it is inferred from the return type from the getter. The setter's parameter must be annotated if it returns a different type from the getter.

They must both have the same _member visibility_.

### **E. Index Signatures**

Index Signatures in classes work the same as other object types.

If we plan on storing indexed data, it is not recommended to use an index signature in a class since it may contain methods and we need to add the possible types as its union members.

## **II. Inheritance**

### **A. `implements`**

The `implements` clause checks if the shape of the class is the same as the given interface.

Multiple implemented inferfaces are separated by a comma.

```ts
class A implements B, C {
	// ...
}
```

#### **Function Parameter Type Checking**

When an object or class is type checked with an object type(object type alias or interface) that has a function, it does not enforce the if the function either has an empty parameter, or isn't narrowed to the same parameter type as the object type.

The parameter type is only enforced when you are ommiting arguments where a function expects more and if it has a different parameter type than the implemented function.

#### **Optional Properties**

When an interface has an optional property, the class that implements the same interface doesn't automatically create the property.

### **B. `extends`**

The derived class inherits all the properties and methods from the extended base class in addition to its defined members.

It can override the base class and its methods. The `super` keyword is used to access its inherited methods within the derived class.

#### **Class Member Initialization**

In JavaScript, even if derived class fields override their base class fields, the base constructor will still run and access its own fields whether or not they share the same constructor.

If a constructor is overridden, the base constructor will run before the derived one runs. This is because the derived field initializations haven't ran yet.

#### **Overrided Method Type Checking**

If a derived class overrides a method, it must have the parameter type and return type as its the base class.

#### **Type-only Field Declarations**

By default `useDefineForClassFields` is `true` if the `target` is `ES2022` or higher. Otherwise we need to manually set the flag to `true` to be able to use `define` class fields. This flag only used older JavaScript versions.

If we want to re-declare a more accurate type for an inherited field, we should prefix `define` then annotate the type to indicate that there is no runtime behavior effect for the given field declaration.

## **III. Visibility Modifiers**

The following visibility modifiers are only enforced during type checking.

For `protected` and `private` members, this means that JavaScript constructs are still able to access the annotated members. They also can be accessed through the square bracket notation denoted by "`instance["memberName"]`".

#### **Parameter Properties**

Aside from class fields, visibility modifiers alongside with `readonly` can also be used as a constructor parameter. This turns the provided argument into class property.

### **A. `public`**

A `public` class member can be accessed anywhere in its instances and subclasses. It is the default visibility modifier and is only declared for style and readability reasons.

### **B. `protected`**

A `protected` member can only be accessed within its declared class and subclasses. This means that the derived class can still access the base class with the protected member.

When overwritting a `protected` class member, its visibility will be changed to the declared one. If the member isn't overwritten, it can't be accessed.

```ts
class Base {
	protected name = "Base";
}

class Derived1 extends Base {
	name = "Derived";
}

class Derived2 extends Base {}

const derived1 = new Derived1();
console.log(derived1.name); // Derived

const derived2 = new Derived2();

// Property 'name' is protected and only accessible within class 'Base' and its subclasses.
console.log(derived2.name);
```

### **C. `private`**

A `private` member can only be accessed within its own class. It cannot be accessed from its subclasses and own instances.

Unlike `protected` members, you cannot overwrite a `private` member to increase its visibility.

```ts
class Base {
	private name = "Base";
}

// Property 'name' is private in type 'Base' but not in type 'Derived'
class Derived extends Base {
	name = "Derived";
}
```

A `private` member can still be accessed if it is referenced within its defined class.

```ts
class Base {
	private name = "Base";
	public method() {
		console.log(this.name);
	}

	public reference = this.name;
}

class Derived extends Base {}

const derived = new Derived();
derived.method(); // Base
console.log(derived.reference); // Base
derived.name; // Cannot be accessed directly
```

#### **Private Fields**

To make JavaScript enforce private members, we should make a private field denoted by prefixing "`#`" after the field name.

```js
class Base {
	#name = "Base";

	method() {
		console.log(this.#name);
	}
}

class Derived extends Base {}

const derived = new Derived();
```

A weakmap will be used in place of private fields when targeting ES2021 or lower.

### **D. `static`**

Static members can only be accessed within the constructor object itself where the annotated members can also use visibility modifiers.

```ts
class MyClass {
	static name = "MyClass";
}

console.log(MyClass.name); // MyClass

const instance = new MyClass();
// Property 'name' does not exist on type 'MyClass'. Did you mean to access the static member 'MyClass.name' instead
console.log(instance.name);
```

They can be also inherited but only used within subclass constructor objects.

```ts
class Base {
	static className = "MyClass";
}

class Derived extends Base {}

console.log(Derived.className); // name
```

#### **Static Namespace**

When annotating a field member as a `static` member, we cannot use `Function` prototype name reserved properties such as `name` and `length`.

#### **Static Initialization Blocks**

Static initialization blocks are evaluated when the class itself is defined.

It is used to write initialization code that is influenced by its global context and change its configuration data.

## **IV. Generic Classes**

A class can be annotated as a generic where its type is inferred based on the given constructor argument.

```ts
class StringOrNumber<Type> {
	content: Type;
	constructor(arg: Type) {
		this.content = arg;
	}
}

// const instance: StringOrNumber<string>
const instance = new StringOrNumber("str");
```

Classes can also use generic constraints and defaults.

## **V. `this` context**

### **JavaScript Runtime behavior**

Since TypeScript doesn't change the runtime behavior of JavaScript, the variable prepended with `this` gains the environment in the object that is used to call the function.

If we want to use its original environment, we can use arrow functions. This does not allow us to use access the previously overwritten methods via `super` since arrow functions don't have their own `super` bindings.

### **`this` parameter**

When `this` is used as a parameter in TypeScript, it checks if the object that is used to call the function is the same as the annotated type. It is erased during runtime.

### **`this` type**

In classes, the `this` type refers to the type of the current class that is used to call the function.

When used as a parameter type, it checks if the given argument matches the same shape as the class where the method resides.

```ts
class Base {
	content = "Base";

	isSameClass(otherClass: this) {
		return otherClass.content === this.content;
	}
}

class Derived extends Base {
	content = "Derived";
	otherContent = "";
}

const base = new Base();
const derived = new Derived();

derived.isSameClass(base);
```

### **`this` type Guard**

When `this` is used as a type guard in a return position and type narrowed, the target object denoted by "`this is Type`" would be narrowed down to the specified `Type`.

```ts
class Base {
	isDerived1(): this is Derived1 {
		return this instanceof Derived1;
	}
	isDerived2(): this is Derived2 {
		return this instanceof Derived2;
	}
	isDerived3(): this is Derived3 {
		return this instanceof Derived3;
	}
}

class Derived1 extends Base {
	content1 = "Derived1";
}
class Derived2 extends Base {
	content2 = "Derived2";
}
class Derived3 extends Base {
	content3 = "Derived3";
}

const derived: Base = new Derived1();

if (derived.isDerived1()) {
	derived.content1; // const derived: Derived1
} else if (derived.isDerived2()) {
	derived.content2; // const derived: Derived2
} else if (derived.isDerived3()) {
	derived.content3; // const derived: Derived3
}
```

Since a class constructor returns an object, we can provide the an object with a specified property to check its existence with type narrowing.

```ts
class MyClass<Type> {
	content?: Type;

	hasContent(): this is { content: Type } {
		return this.content !== undefined;
	}
}

const myClass = new MyClass();
myClass.content = "myClass"; // MyClass<unknown>.content?: unknown

if (myClass.hasContent()) {
	myClass.content; // content: unknown
}
```

## **VI. Class Instance Annotation**

The `InstanceType<Type>` utility type annotates a variable with the given `Type` as the class instance. It checks whether it has the same shape as the given instance.

```ts
class MyClass {
	name: "";
}

type MyClassInstance = InstanceType<typeof MyClass>;

const instance: MyClassInstance = new MyClass();
```

## **VII. Class Shape Annotation**

An `abstract` class allows us to create `abstract` members which have to be implemented by the extended derived class.

It cannot be instantiated with `new` and will inherit its _concrete_ members.

```ts
abstract class Base {
	abstract method(): string;

	abstract field: string;

	sayHi() {
		console.log("Hi");
	}
}

class Derived extends Base {
	method() {
		return "";
	}

	field = "";
}

const derived = new Derived();
derived.sayHi(); // inherited from Base
```

Abstract instances can't be used as a type annotation because they cannot be instantiated. However, it can be as the returned object of a _constructor signature_ since class types describe the shape of an object.

```ts
function createInstance(constructor: new () => Base) {
	const instance = new constructor();
	instance.sayHi();
}

createInstance(Derived);
// cannot assign abstract constructor to a non-abstract constructor type
createInstance(Base);
```

## **VIII. Class Inference**

Since TypeScript has a structural type system, classes are compared by their members. This means that classes can be typed with a non-inherited class or subclass as long as the constructor or object returns the same shape.

In a structural type system, a type with no members (such as an empty class or object) can be used as a type annotation for anything else. This means that it is not recommended to create an empty class.
