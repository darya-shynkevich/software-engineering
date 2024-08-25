The `useSelector` hook lets our component extract whatever pieces of data it needs from the Redux store state.

```js
// The function below is called a selector and allows us to select a value from
// the state. Selectors can also be defined inline where they're used instead of
// in the slice file. For example: `useSelector((state) => state.counter.value)`
export const selectCount = state => state.counter.value
```

If we had access to a Redux store, we could retrieve the current counter value as:

```js
const count = selectCount(store.getState())
console.log(count)
// 0
```

Our components can't talk to the Redux store directly, because we're not allowed to import it into component files. But, `useSelector` takes care of talking to the Redux store behind the scenes for us. If we pass in a selector function, it calls `someSelector(store.getState())` for us, and returns the result.

```js
const count = useSelector(selectCount)
```

```js
const countPlusTwo = useSelector(state => state.counter.value + 2)
```

Any time an action has been dispatched and the Redux store has been updated, `useSelector` will re-run our selector function. If the selector returns a different value than last time, `useSelector` will make sure our component re-renders with the new value.