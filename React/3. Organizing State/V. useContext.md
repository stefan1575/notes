# **Context**

Lifting state up has two disadvantages:

1. When passing state through multiple components in the middle, where the components in the middle have no actual need for the data.
2. Consuming the state can be repetitive when multiple components need the same information.

React Context allows any nested component to directly consume data from its parent component without having to pass down props manually for every component in the middle.

### **Considering Alternatives**

Before using context we should consider passing `children` inside the parent component if it doesn't need to know about the prop and pass the prop down directly to the child component instead.

This makes it so that the parent component simply renders its children making the component structure more maintainable.

## **I. `createContext`**

The `createContext` method creates a context that components parent components can provide to be read by a nested component.

```jsx
import { createContext } from "react";

export const SomeContext = createContext(defaultValue);
```

## **II. Context Provider**

The Context Provider component is wrapped to the child components that will consume the context.

```jsx
import { useState } from "react";
import { createContext } from "react";

function App() {
	const [state, setState] = useState(value);

	return <SomeContext.Provider value={value}>{children}</SomeContext.Provider>;
}
```

## **III. `useContext`**

The `useContext` hook allows a nested component inside a Context Provider to access the its value.

```jsx
import { useContext } from "react";
import { SomeContext } from "./SomeContext.js";

export default function NestedComponent() {
	const context = useContext(SomeContext);
	// ...
}
```

### **Rendering Context**

Context passes through intermediate components. This means that the value of the component with the context will depend on where it is being rendered.

If you want to override the context of the parent, you have to wrap the children into another context provider with a different value.

```jsx
// ./HeadingLevel.js - Context Value
export const HeadingLevel = createContext(0);


// ./Section.js - Context Provider/Container Component
import { useContext } from "react";
import { HeadingLevel } from "./HeadingLevel.js";

export default function Section({children}) {
    const level = useContext(LevelContext)
    return (
        <section>
            <HeadingLevel.Provider value={level + 1}>
                {children}
            </HeadingLevel.Provider>
        </section>
    )
}

// ./Heading.js - Context Consumer
import { useContext } from "react";
import { HeadingLevel } from "./HeadingLevel.js";

export default function Heading({ children }) {
	const level = useContext(HeadingLevel);
	switch (level) {
		case 1:
			return <h1>{children}</h1>;
		case 2:
            return <h2>{children}</h2>;
		case 3:
			return <h3>{children}</h3>;
		case 4:
			return <h4>{children}</h4>;
		case 5:
			return <h5>{children}</h5>;
		case 6:
			return <h6>{children}</h6>;
		default:
			throw Error("Incorrect Level");
	}
}

// ./App.js
import Heading from 'Heading.js'
import Section from 'Section.js'

export default function App() {
    return (
        <Section>
            <Heading>Text inside uses h1</Heading>
            <Section>
                <Heading>Text inside uses h2</Heading>
                <Section>
                    <Heading>Text inside uses h3</Heading>
                </Section>
            </Section>
        </Section>
    )
}
```
