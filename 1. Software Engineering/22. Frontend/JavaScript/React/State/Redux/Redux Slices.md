**A "slice" is a collection of Redux reducer logic and actions for a single feature in your app**, typically defined together in a single file. The name comes from splitting up the root Redux state object into multiple "slices" of state.

Redux has a function called [`combineReducers`](https://redux.js.org/api/combinereducers) that does this for us automatically. It accepts an object full of slice reducers as its argument, and returns a function that calls each slice reducer whenever an action is dispatched. The result from each slice reducer are all combined together into a single object as the final result. We can do the same thing as the previous example using `combineReducers`:

```js
const rootReducer = combineReducers({  
	users: usersReducer,  
	posts: postsReducer,  
	comments: commentsReducer  
})
```

```js
import { createSlice } from '@reduxjs/toolkit'

export const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0
  },
  reducers: {
    increment: state => {
      // Redux Toolkit allows us to write "mutating" logic in reducers. It
      // doesn't actually mutate the state because it uses the immer library,
      // which detects changes to a "draft state" and produces a brand new
      // immutable state based off those changes
      state.value += 1
    },
    decrement: state => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    }
  }
})

export const { increment, decrement, incrementByAmount } = counterSlice.actions

export default counterSlice.reducer
```

