# **Props**

In React, Props are HTML attributes that is passed inside a JSX tag.

The props that you can pass built-in HTML tags are predefined.

### **Immutability**

Props are immutable, meaning they cannot be mutated once its value is set. When changing changing a prop, a new prop object is created in place of the old one where it will be garbage collected.

React discourages changing props when responding to changes in data or user input because it could lead to unpredictable behavior and performance issues.

## **Custom Props**

When using custom components, you can create your own custom props that accept any JavaScript value as long as its enclosed in curly braces `{}`.

Parent components can pass information to its child components by giving them props.

```jsx
export default function ParentComponent() {
	return <ChildComponent user={{ name: "John", age: 20 }} isAdmin={true} />;
}
```

Child components can read the parent component props by listing the prop names inside curly braces `{}` as a parameter in the child component function.

A default value can be specified and will be used if the prop's value is `undefined`.

```jsx
function ChildComponent({ user, isAdmin = false }) {
    const adminStatus = isAdmin : "Admin" ? "User";
	return (
		<>
			<p>Welcome back, {adminStatus} {user.name}</p>
            <p>You are {user.age} years old.</p>
		</>
	);
}
```

### **Spread Syntax**

Some components forward all their props to their children. If the component doesn't directly use its props, it makes sense to use spread syntax.

```jsx
function IntermediaryComponent(props) {
    return (
        <div className="container">
            <ChildComponent {...props}>
        </div>
    )
}
```

### **Accessing Child Component Props**

The `children` prop is a built-in prop that allows a parent component to access its entire child component including its props.

It is useful for components like wrappers that don't know their children ahead of time.

```jsx
function Card({ children }) {
	return <div className="card">{children}</div>;
}

function Avatar() {
	return <img src="./avatar.png" />;
}

export default function Profile() {
	return (
		<Card>
			<Avatar />
		</Card>
	);
}
```
