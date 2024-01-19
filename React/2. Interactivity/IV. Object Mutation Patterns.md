# **Object Value Mutation Patterns**

In React, state object values should not be directly mutated because only `set` functions trigger a re-render. They should be replaced with a new object or a copy of an existing one when invoking `set` functions.

## **I. Copying Objects**

A common pattern is to use the spread syntax to copy existing state object properties and specify the property of the state to override.

### **Computed property names `[]`**

If multiple components share the same re-rendering behavior, an identifier with a common format can be used as the computed property name to prevent duplication.

```jsx
import { useState } from "react";

export default function Form() {
	const [user, setUser] = useState({
		firstName: "John",
		lastName: "Smith",
	});

	function handleChange(event) {
		setUser({
			...user,
			[event.target.name]: event.target.value,
		});
	}

	return (
		<>
			<label>
				First Name:
				<input
					name="firstName"
					value={user.firstName}
					onChange={handleChange}
				/>
			</label>
			<label>
				Last Name:
				<input name="lastName" value={user.lastName} onChange={handleChange} />
			</label>
		</>
	);
}
```

## **II. Nested Objects**

The reason why the spread syntax only copies one level deep is because object properties are actually separate objects pointing to each other.

If we want to copy a "nested object" using the spread syntax, its property name has to be specified alongside the new object with its pre-existing properties.

```jsx
const obj = {
	firstName: "John",
	lastName: "Smith",
	userInfo: {
		userName: "JohnSmith",
		password: "12345",
	},
};

const copy = {
	...obj,
	...obj.userInfo,
};
```

### **`useImmer`**

The `useImmer` hook has similar behavior with `useState` but replaces its setter function with a callback function that recieves a `draft` object containing a copy of the state object.

The `draft` object allows us to use the mutating syntax and automatically copies existing state properties.

```bash
npm install use-immer
```

It is useful for complex state objects with many nested properties or values since we don't need to copy all of them which reduces boilerplate code.

```jsx
import { useImmer } from "use-immer";

export default function App() {
	const [user, setUser] = useImmer({
		firstName: "John",
		lastName: "Smith",
		userInfo: {
			username: "JohnSmith",
			password: "password",
		},
	});

	function updateUsername() {
		setUser((draft) => {
			draft.userInfo.username = event.target.value;
		});
	}
	function updatePassword() {
		setUser((draft) => {
			draft.userInfo.password = event.target.value;
		});
	}

	return (
		<>
			<label>
				Username:
				<input value={user.userInfo.username} onChange={updateUsername} />
			</label>
			<label>
				Password:
				<input value={user.userInfo.password} onChange={updatePassword} />
			</label>
		</>
	);
}
```

## **III. Copying Arrays**

A new array should be copied when performing methods that mutate the array.

|           | Mutates the Array          | Returns a new Array  |
| --------- | -------------------------- | -------------------- |
| adding    | `push`, `unshift`          | `concat`, `[...arr]` |
| removing  | `pop`, `shift`, `splice`   | `filter`, `slice`    |
| replacing | `splice`, `arr[i] = value` | `map`                |
| sorting   | `reverse`, `sort`          |                      |

Alternatively, the `immer` method can be used to freely mutate the array.

### **Adding/Removing new properties**

We can add new elements to an array by using the spread syntax at the start/end of an array to add existing elements alongside the new element we want to insert.

To remove an array element using the `filter` method, we should pass down a reference from the element that we may remove.

```jsx
import { useState } from "react";

export default function List() {
	const [inputValue, setInputValue] = useState("");
	const [counter, setCounter] = useState(0);
	const [listArray, setListArray] = useState([]);

	function handleChange(event) {
		setInputValue(event.target.value);
	}

	function handleAddListItem() {
		setCounter(() => counter + 1);
		setListArray(() => {
			return [
				...listArray,
				{
					id: counter,
					content: inputValue,
				},
			];
		});
	}

	function handleRemoveListItem(id) {
		setListArray(listArray.filter((item) => item.id !== id));
	}

	return (
		<>
			<input value={inputValue} onChange={handleChange} />
			<button onClick={handleAddListItem}>Add</button>
			<ul>
				{listArray.map((item) => (
					<li key={item.id}>
						{item.content}{" "}
						<button
							onClick={() => {
								handleRemoveListItem(item.id);
							}}
						>
							Remove
						</button>
					</li>
				))}
			</ul>
		</>
	);
}
```

### **Inserting properties between an array**

To insert an element between an array, we should use the spread syntax together with the `slice` method with the starting index index `0` and another index as its insertion point as arguments, then the item that we want to insert, and then the rest of the original array using `slice` with the provided insertion point as its argument.

```jsx
import { useState } from "react";

const InitialListItems = [
	{
		id: "01",
		content: "List Item #1",
	},
	{
		id: "02",
		content: "List Item #2",
	},
	{
		id: "03",
		content: "List Item #3",
	},
];

export default function List() {
	// ...
	const insertionPoint = 1;

	function handleInsertListItem() {
		setCounter(() => counter + 1);
		setListArray(() => {
			return [
				...listArray.slice(0, insertionPoint),
				{
					id: counter,
					content: inputValue,
				},
				...listArrray.slice(insertionPoint),
			];
		});
	}
	// ...
}
```

## **IV. Updating objects inside arrays**

When updating state with nested objects, you need its value has to be overwritten with a copy of the state and all its nested objects alongside the property values that you want to update.

The `map` method alongside the spread syntax allows us to create a new copy alongside the object that we want to update. We can immediately return the currently mapped property to leave it intact while overriding the object we want to update denoted by a condition.

For instance, two rendered components reference the same object in its initial state. If the `set` function mutates the initial object, it will affect the values of the other component's state.

```jsx
import { useState } from "react";

const initialList = [
	{
		id: "01",
		content: "List Item #1",
		isChecked: false,
	},
	{
		id: "02",
		content: "List Item #2",
		isChecked: false,
	},
	{
		id: "03",
		content: "List Item #3",
		isChecked: false,
	},
];

export default function List() {
	const [list1, setList1] = useState([...initialList]);
	const [list2, setList2] = useState([...initialList]);

	function handleToggleList1(id, isChecked) {
		setList1(
			list1.map((obj) => {
				if (obj.id === id) {
					return { ...obj, isChecked: !isChecked };
				} else {
					return obj;
				}
			})
		);
	}

	function handleToggleList2(id, isChecked) {
		setList2(
			list2.map((obj) => {
				if (obj.id === id) {
					return { ...obj, isChecked: !isChecked };
				} else {
					return obj;
				}
			})
		);
	}
	return (
		<>
			<p>List #1</p>
			<ul>
				{list1.map((obj) => (
					<li key={obj.id}>
						<label>
							<input
								type="checkbox"
								checked={obj.isChecked}
								onChange={() => {
									handleToggleList1(obj.id, obj.isChecked);
								}}
							></input>
							{obj.content}
						</label>
					</li>
				))}
			</ul>
			<p>List #2</p>
			<ul>
				{list2.map((obj) => (
					<li key={obj.id}>
						<label>
							<input
								type="checkbox"
								checked={obj.isChecked}
								onChange={() => {
									handleToggleList2(obj.id, obj.isChecked);
								}}
							></input>
							{obj.content}
						</label>
					</li>
				))}
			</ul>
		</>
	);
}
```

### **Updating nested objects using `useImmer`**

The `useImmer` hook can be used alongside the `find` method to create a reference to the object we want to update and use the mutation syntax to

```jsx
function handleToggleList1(id, isChecked) {
	setList1((draft) => {
		const toggledObject = draft.find((obj) => {
			return obj.id === id;
		});

		toggledObject.isChecked = !isChecked;
	});
}
```
