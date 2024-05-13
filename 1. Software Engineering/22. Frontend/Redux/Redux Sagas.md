_Sagas_, are declarative and well organized way to express the side-effects. They are useful when you need a process to coordinate with multiple action creators and side-effects.

Sagas are implemented as ES6 [Generator functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*) that _yield_ objects to the redux-saga middleware. The yielded objects are a kind of instruction to be interpreted by the middleware. When a Promise is yielded to the middleware, the middleware will suspend the Saga until the Promise completes. These generators make the asynchronous flows easy to read, write and test.

```JS
/*  
Starts fetchUser on each dispatched `USER_FETCH_REQUESTED` action.  
Allows concurrent fetches of user.  
*/  
function* mySaga() {  
yield takeEvery('USER_FETCH_REQUESTED', fetchUser)  
}
```

```js
/*  
Alternatively you may use takeLatest.  
  
Does not allow concurrent fetches of user. If "USER_FETCH_REQUESTED" gets  
dispatched while a fetch is already pending, that pending fetch is cancelled  
and only the latest one will be run.  
*/  
function* mySaga() {  
yield takeLatest('USER_FETCH_REQUESTED', fetchUser)  
}
```