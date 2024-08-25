Usually, you will pass information from a parent component to a child component via props. But passing props can become verbose and inconvenient if you have to pass them through many components in the middle, or if many components in your app need the same information. _Context_ lets the parent component make some information available to any component in the tree below it—no matter how deep—without passing it explicitly through props.

**Context lets you write components that “adapt to their surroundings” and display themselves differently depending on _where_ (or, in other words, _in which context_) they are being rendered.**

**Different React contexts don’t override each other.** Each context that you make with `createContext()` is completely separate from other ones, and ties together components using and providing _that particular_ context. One component may use or provide many different contexts without a problem.

![](Pasted%20image%2020240524002933.png)
## The problem with passing props

[Passing props](https://react.dev/learn/passing-props-to-a-component) is a great way to explicitly pipe data through your UI tree to the components that use it.

But passing props can become verbose and inconvenient when you need to pass some prop deeply through the tree, or if many components need the same prop. The nearest common ancestor could be far removed from the components that need data, and [lifting state up](https://react.dev/learn/sharing-state-between-components) that high can lead to a situation called **“prop drilling”.**

```js
<Section level={3}>  
	<Heading>About</Heading>  
	<Heading>Photos</Heading>  
	<Heading>Videos</Heading>
</Section>
```

### Step 1: Create the context

```js
import { createContext } from 'react';

export const LevelContext = createContext(1);
```

### Step 2: Use the context

```js
export default function Heading({ children }) {  

const level = useContext(LevelContext);  
// ...  
}
```

### Step 3: Provide the context

```js
import { LevelContext } from './LevelContext.js';  

export default function Section({ level, children }) {  
	const level = useContext(LevelContext);
	return (  
		<section className="section">  
			<LevelContext.Provider value={level + 1}>  
				{children}  
			</LevelContext.Provider>  
		</section>  
	);  
}
```

This tells React: “if any component inside this `<Section>` asks for `LevelContext`, give them this `level`.” The component will use the value of the nearest `<LevelContext.Provider>` in the UI tree above it.

## Use cases for context

- **Theming:** If your app lets the user change its appearance (e.g. dark mode), you can put a context provider at the top of your app, and use that context in components that need to adjust their visual look.
- **Current account:** Many components might need to know the currently logged in user. Putting it in context makes it convenient to read it anywhere in the tree. Some apps also let you operate multiple accounts at the same time (e.g. to leave a comment as a different user). In those cases, it can be convenient to wrap a part of the UI into a nested provider with a different current account value.
- **Routing:** Most routing solutions use context internally to hold the current route. This is how every link “knows” whether it’s active or not. If you build your own router, you might want to do it too.
- **Managing state:** As your app grows, you might end up with a lot of state closer to the top of your app. Many distant components below may want to change it. It is common to [use a reducer together with context](https://react.dev/learn/scaling-up-with-reducer-and-context) to manage complex state and pass it down to distant components without too much hassle.

# References:

1. [Scaling Up with Reducer and Context](https://react.dev/learn/scaling-up-with-reducer-and-context)



