
You can add state to a component with a [`useState`](https://react.dev/reference/react/useState) Hook. _Hooks_ are special functions that let your components use React features (state is one of those features). The `useState` Hook lets you declare a state variable. It takes the initial state and returns a pair of values: the current state, and a state setter function that lets you update it.

```js
const [index, setIndex] = useState(0);  
const [showMore, setShowMore] = useState(false);
```

The [`useState`](https://react.dev/reference/react/useState) Hook provides:
1. A **state variable** to retain the data between renders.
2. A **state setter function** to update the variable and trigger React to render the component again.

When you call [`useState`](https://react.dev/reference/react/useState), you are telling React that you want this component to remember something

```js
const [index, setIndex] = useState(0);
```

In this case, you want React to remember `index`.

1. **Your component renders the first time.** Because you passed `0` to `useState` as the initial value for `index`, it will return `[0, setIndex]`. React remembers `0` is the latest state value.
2. **You update the state.** When a user clicks the button, it calls `setIndex(index + 1)`. `index` is `0`, so it’s `setIndex(1)`. This tells React to remember `index` is `1` now and triggers another render.
3. **Your component’s second render.** React still sees `useState(0)`, but because React _remembers_ that you set `index` to `1`, it returns `[1, setIndex]` instead.
4. And so on!

#### How does React know which state to return?

To enable their concise syntax, Hooks **rely on a stable call order on every render of the same component.** This works well in practice because if you follow the rule above (“only call Hooks at the top level”), Hooks will always be called in the same order.

Internally, React holds an array of state pairs for every component. It also maintains the current pair index, which is set to `0` before rendering. Each time you call `useState`, React gives you the next state pair and increments the index. You can read more about this mechanism in [React Hooks: Not Magic, Just Arrays.](https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e)

## State is isolated and private

State is local to a component instance on the screen. In other words, **if you render the same component twice, each copy will have completely isolated state!** Changing one of them will not affect the other.

Unlike props, **state is fully private to the component declaring it.** The parent component can’t change it. This lets you add state to any component or remove it without impacting the rest of the components.

**React preserves a component’s state for as long as it’s being rendered at its position in the UI tree.** If it gets removed, or a different component gets rendered at the same position, React discards its state.

Also, **when you render a different component in the same position, it resets the state of its entire subtree.**

As a rule of thumb, **if you want to preserve the state between re-renders, the structure of your tree needs to “match up”** from one render to another. If the structure is different, the state gets destroyed because React destroys state when it removes a component from the tree.

#### Resetting state at the same position

By default, React preserves state of a component while it stays at the same position.

1. Render components in different positions
2. Give each component an explicit identity with `key`

```js
{isPlayerA ? (
	<Counter key="Taylor" person="Taylor" />
) : (
	<Counter key="Sarah" person="Sarah" />
)}
```

Specifying a `key` tells React to use the `key` itself as part of the position, instead of their order within the parent. This is why, even though you render them in the same place in JSX, React sees them as two different counters, and so they will never share state. Every time a counter appears on the screen, its state is created. Every time it is removed, its state is destroyed. Toggling between them resets their state over and over.

> Remember that keys are not globally unique. They only specify the position _within the parent_.

