**Global state that is needed across the app should go in the Redux store. State that's only needed in one place should be kept in component state.**

**In a React + Redux app, your global state should go in the Redux store, and your local state should stay in React components.**

**Most form state probably shouldn't be kept in Redux**
### Actions

An **action** is a plain JavaScript object that has a `type` field. **You can think of an action as an event that describes something that happened in the application**.

```js
const addTodoAction = {  
	type: 'todos/todoAdded',  
	payload: 'Buy milk'  
}
```

### Action Creators

An **action creator** is a function that creates and returns an action object. We typically use these so we don't have to write the action object by hand every time:

```js
const addTodo = text => {  
	return {  
		type: 'todos/todoAdded',  
		payload: text  
	}  
}
```

### Reducers

>  **reducers are _never_ allowed to mutate the original / current state values!**

A **reducer** is a function that receives the current `state` and an `action` object, decides how to update the state if necessary, and returns the new state: `(state, action) => newState`. **You can think of a reducer as an event listener which handles events based on the received action (event) type.**

- They should **only calculate the new state value based on the `state` and `action` arguments** => *it's easier to understand how that code works, and to test it.*
- They are **not allowed to modify the existing `state`**. Instead, they must make _immutable updates_, by copying the existing `state` and making changes to the copied values.
	- *It causes bugs, such as the UI not updating properly to show the latest values*
	- *It makes it harder to understand why and how the state has been updated*
	- *It makes it harder to write tests*
	- *It breaks the ability to use "time-travel debugging" correctly*
	- *It goes against the intended spirit and usage patterns for Redux*
- They **must not** do any asynchronous logic, calculate random values, or **cause** other **"side effects"**

The logic inside reducer functions typically follows the same series of steps:
- Check to see if the reducer cares about this action
    - If so, make a copy of the state, update the copy with new values, and return it
- Otherwise, return the existing state unchanged

### Store

The current Redux application state lives in an object called the **store**.

The store is created by passing in a reducer, and has a method called `getState` that returns the current state value:

```js
import { configureStore } from '@reduxjs/toolkit'  
  
const store = configureStore({ reducer: counterReducer })  
  
console.log(store.getState())  
// {value: 0}
```

### Dispatch

The Redux store has a method called `dispatch`. **The only way to update the state is to call `store.dispatch()` and pass in an action object**. The store will run its reducer function and save the new state value inside, and we can call `getState()` to retrieve the updated value:

```js
store.dispatch({ type: 'counter/increment' })  
  
console.log(store.getState())  
// {value: 1}
```

**You can think of dispatching actions as "triggering an event"** in the application. Something happened, and we want the store to know about it. Reducers act like event listeners, and when they hear an action they are interested in, they update the state in response.

We typically call action creators to dispatch the right action:

```js
const increment = () => {  
	return {  
		type: 'counter/increment'  
	}  
}  
  
store.dispatch(increment())  
  
console.log(store.getState())  
// {value: 2}
```

The `useDispatch` hook does that for us, and gives us the actual `dispatch` method from the Redux store:

```js
const dispatch = useDispatch()

<button
  className={styles.button}
  aria-label="Increment value"
  onClick={() => dispatch(increment())}
>
  +
</button>
```

### [[Selectors]]

**Selectors** are functions that know how to extract specific pieces of information from a store state value. As an application grows bigger, this can help avoid repeating logic as different parts of the app need to read the same data:

```js
const selectCounterValue = state => state.value  
  
const currentValue = selectCounterValue(store.getState())  
console.log(currentValue)  
// 2
```

Selectors will re-run whenever the Redux store is updated, and if the data they return has changed, the component will re-render
