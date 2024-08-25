A **thunk** is a specific kind of Redux function that can contain asynchronous logic.

```js
// the outside "thunk creator" function
const fetchUserById = userId => {
  // the inside "thunk function"
  return async (dispatch, getState) => {
    try {
      // make an async call in the thunk
      const user = await userAPI.fetchById(userId)
      // dispatch an action when we get the response back
      dispatch(userLoaded(user))
    } catch (err) {
      // If something went wrong, handle it here
    }
  }
}
```

# References:

1. [Redux Thunk docs](https://redux.js.org/usage/writing-logic-thunks)
2. [What the heck is a thunk?](https://daveceddia.com/what-is-a-thunk/) 
3. [Redux FAQ entry on "why do we use middleware for async?"](https://redux.js.org/faq/actions#how-can-i-represent-side-effects-such-as-ajax-calls-why-do-we-need-things-like-action-creators-thunks-and-middleware-to-do-async-behavior)