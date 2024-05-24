- [`<Fragment>`](https://react.dev/reference/react/Fragment), alternatively written as `<>...</>`, lets you group multiple JSX nodes together.
- [`<Profiler>`](https://react.dev/reference/react/Profiler) lets you measure rendering performance of a React tree programmatically.
- [`<Suspense>`](https://react.dev/reference/react/Suspense) lets you display a fallback while the child components are loading.
- [`<StrictMode>`](https://react.dev/reference/react/StrictMode) enables extra development-only checks that help you find bugs early.

React lets you create components, **reusable UI elements for your app.**
In a React app, every piece of UI is a component.
React components are regular JavaScript functions except:
    1. Their names always begin with a capital letter.
    2. They return JSX markup.

# Passing Props to a Component

> Props are read-only snapshots in time: every render receives a new version of props.
> You can’t change props. When you need interactivity, you’ll need to set state.

React components use _props_ to communicate with each other. Every parent component can pass some information to its child components by giving them props.

```js
function Avatar({ person, size = 100 }) {   // default value
}

IS EQUAL

function Avatar(props) {  
	let person = props.person;  
	let size = props.size;  
}

function Profile(props) {  
	return (  
		<div className="card">  
			<Avatar {...props} />  
		</div>  
	);  
}
```

## Passing JSX as children

```js
function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}
```

You can think of a component with a `children` prop as having a “hole” that can be “filled in” by its parent components with arbitrary JSX. You will often use the `children` prop for visual wrappers: panels, grids, etc.

# Conditional Rendering

```js
if (isPacked) {  
	return <li className="item">{name} ✔</li>;  
}  

return <li className="item">{name}</li>;

IS EQUAL

return (  
	<li className="item">  
		{isPacked ? name + ' ✔' : name}  
	</li>  
);
```

This style works well for simple conditions, but use it in moderation. If your components get messy with too much nested conditional markup, consider extracting child components to clean things up. In React, markup is a part of your code, so you can use tools like variables and functions to tidy up complex expressions.

# Rendering Lists

File names in a folder and JSX keys in an array serve a similar purpose. They let us uniquely identify an item between its siblings. A well-chosen key provides more information than the position within the array. Even if the _position_ changes due to reordering, the `key` lets React identify the item throughout its lifetime.

**Where to get keys**
- **Data from a database:** If your data is coming from a database, you can use the database keys/IDs, which are unique by nature.
- **Locally generated data:** If your data is generated and persisted locally (e.g. notes in a note-taking app), use an incrementing counter, [`crypto.randomUUID()`](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/randomUUID) or a package like [`uuid`](https://www.npmjs.com/package/uuid) when creating items.

**Rules of keys**
- **Keys must be unique among siblings.** However, it’s okay to use the same keys for JSX nodes in _different_ arrays.
- **Keys must not change** or that defeats their purpose! Don’t generate them while rendering.

**Pitfalls**:
- **Do not use index as a key** (default React behaviour is key is unspecified). But the order in which you render items will change over time if an item is inserted, deleted, or if the array gets reordered. Index as a key often leads to subtle and confusing bugs.
- **Do not generate keys on the fly**, e.g. with `key={Math.random()}`. This will cause keys to never match up between renders, leading to all your components and DOM being recreated every time. Not only is this slow, but it will also lose any user input inside the list items. Instead, use a stable ID based on the data.

# Keeping Components Pure

**React assumes that every component you write is a pure function.** This means that React components you write must always return the same JSX given the same inputs.

React’s rendering process must always be pure. Components should only _return_ their JSX, and not _change_ any objects or variables that existed before rendering—that would make them impure!

> Rendering can happen at any time, so components should not depend on each others’ rendering sequence.

# Understanding Your UI as a Tree

- Render trees represent the nested relationship between React components across a single render.

![[Pasted image 20240523204001.png]]
> Render trees help identify what the top-level and leaf components are. Top-level components affect the rendering performance of all components beneath them and leaf components are often re-rendered frequently. Identifying them is useful for understanding and debugging rendering performance.
