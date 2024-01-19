# **Sharing State Between Components**

To share between two components, the component's state has to be moved to their closest common parent and passed down via props.

This is used to synchonize the state of multiple related components.

By initially passing hardcoded data from the common parent, you can immediately verify that the components are recieving props from their parent.

```jsx
import { useState } from "react";

function Card({ title, children, isActive, onClickHandler }) {
	return (
		<section>
			<h2>{title}</h2>
			{isActive ? (
				<p>{children}</p>
			) : (
				<button onClick={onClickHandler}>Show</button>
			)}
		</section>
	);
}

export default function Panel() {
	const [selectedCard, setSelectedCard] = useState(0);

	return (
		<div>
			<Card
				title="Describing the UI"
				onShow={() => setSelectedCard(0)}
				isActive={selectedCard === 0}
			>
				<p>Lorem ipsum dolor sit amet.</p>
			</Card>
			<Card
				title="Adding Interactivity"
				onShow={() => setSelectedCard(1)}
				isActive={selectedCard === 1}
			>
				<p>Lorem ipsum dolor sit amet.</p>
			</Card>
			<Card
				title="Managing State"
				onShow={() => setSelectedCard(2)}
				isActive={selectedCard === 2}
			>
				<p>Lorem ipsum dolor sit amet.</p>
			</Card>
		</div>
	);
}
```

## **Controlled and uncontrolled components**

A controlled components has its own state controlled by its parent component via props, while an uncontrolled component stores its own state internally.
