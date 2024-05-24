Immer uses a special JS tool called a `Proxy` to wrap the data you provide, and lets you write code that "mutates" that wrapped data. But, **Immer tracks all the changes you've tried to make, and then uses that list of changes to return a safely immutably updated value**, as if you'd written all the immutable update logic by hand.

> **You can _only_ write "mutating" logic in Redux Toolkit's `createSlice` and `createReducer` because they use Immer inside! If you write mutating logic in reducers without Immer, it _will_ mutate the state and cause bugs!**

