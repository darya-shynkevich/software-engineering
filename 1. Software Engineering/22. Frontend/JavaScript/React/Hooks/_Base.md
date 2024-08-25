Hooks are functions that let you “hook into” React state and lifecycle features from function components. 

- ✅ Call them at the top level in the body of a React [function component](https://react.dev/learn/your-first-component)
- ✅ Call them at the top level in the body of a [custom Hook](https://react.dev/learn/reusing-logic-with-custom-hooks)

```js
function Counter() {  
// ✅ Good: top-level in a function component  
const [count, setCount] = useState(0);  

}  

function useWindowWidth() {  
// ✅ Good: top-level in a custom Hook  
const [width, setWidth] = useState(window.innerWidth);  
}
```

It’s **not** supported to call Hooks in any other cases, for example:
- 🔴 Do not call Hooks inside conditions or loops.
- 🔴 Do not call Hooks after a conditional `return` statement.
- 🔴 Do not call Hooks in event handlers.
- 🔴 Do not call Hooks in class components.
- 🔴 Do not call Hooks inside functions passed to `useMemo`, `useReducer`, or `useEffect`.
- 🔴 Do not call Hooks inside `try`/`catch`/`finally` blocks.

By following this rule, you ensure that all stateful logic in a component is clearly visible from its source code.

## State Hooks

[_State_](State%20as%20a%20snapshot.md) lets a component [“remember” information like user input.](https://react.dev/learn/state-a-components-memory) For example, a form component can use state to store the input value, while an image gallery component can use state to store the selected image index.

To add state to a component, use one of these Hooks:
- [`useState`](State%20as%20a%20snapshot.md) declares a state variable that you can update directly.
- [`useReducer`](Reducer.md) declares a state variable with the update logic inside a [reducer function.](https://react.dev/learn/extracting-state-logic-into-a-reducer)

## [Context Hooks](useContext.md)

_Context_ lets a component [receive information from distant parents without passing it as props.](https://react.dev/learn/passing-props-to-a-component) For example, your app’s top-level component can pass the current UI theme to all components below, no matter how deep.

## Ref Hooks

_Refs_ let a component [hold some information that isn’t used for rendering,](https://react.dev/learn/referencing-values-with-refs) like a DOM node or a timeout ID.

## Effect Hooks

You’ve likely performed data fetching, subscriptions, or manually changing the DOM from React components before. We call these operations “side effects” (or “effects” for short) because they can affect other components and can’t be done during rendering.

*The Effect Hook, `useEffect`, adds the ability to perform side effects from a function component. It serves the same purpose as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` in React classes, but unified into a single API.*

_Effects_ let a component [connect to and synchronize with external systems.](https://react.dev/learn/synchronizing-with-effects) This includes dealing with network, browser DOM, animations, widgets written using a different UI library, and other non-React code.

```js
function ChatRoom({ roomId }) {  
	useEffect(() => {  // connects a component to an external system.
		const connection = createConnection(roomId);  
		connection.connect();  
		return () => connection.disconnect();  
	}, [roomId]);
```

When you call `useEffect`, you’re telling React to run your “effect” function after flushing changes to the DOM. By default, React runs the effects after every render — _including_ the first render.

- [`useLayoutEffect`](https://react.dev/reference/react/useLayoutEffect) fires before the browser repaints the screen. You can measure layout here.
- [`useInsertionEffect`](https://react.dev/reference/react/useInsertionEffect) fires before React makes changes to the DOM. Libraries can insert dynamic CSS here.

## Performance Hooks

A common way to optimize re-rendering performance is to skip unnecessary work. For example, you can tell React to reuse a cached calculation or to skip a re-render if the data has not changed since the previous render.

- [`useMemo`](https://react.dev/reference/react/useMemo) lets you cache the result of an expensive calculation.
- [`useCallback`](https://react.dev/reference/react/useCallback) lets you cache a function definition before passing it down to an optimized component.

