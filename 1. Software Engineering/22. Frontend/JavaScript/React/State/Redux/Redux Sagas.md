`redux-saga` is a library that aims to make application side effects (i.e. asynchronous things like data fetching and impure things like accessing the browser cache) easier to manage, more efficient to execute, easy to test, and better at handling failures.

The mental model is that a saga is like **a separate thread in your application that's solely responsible for side effects**. `redux-saga` is a redux middleware, which means this thread can be started, paused and cancelled from the main application with normal redux actions, it **has access to the full redux application state and it can dispatch redux actions as well**.

```js
class UserComponent extends React.Component {  
	...  
	onSomeButtonClicked() {  
		const { userId, dispatch } = this.props  
		dispatch({type: 'USER_FETCH_REQUESTED', payload: {userId}})  
	}  
	...  
}
```

```js  
// worker Saga: will be fired on USER_FETCH_REQUESTED actions  
function* fetchUser(action) {  
	try {  
		const user = yield call(Api.fetchUser, action.payload.userId)  
		yield put({ type: 'USER_FETCH_SUCCEEDED', user: user })  
	} catch (e) {  
		yield put({ type: 'USER_FETCH_FAILED', message: e.message })  
	}  
}

/*  
Starts fetchUser on each dispatched `USER_FETCH_REQUESTED` action.  
Allows concurrent fetches of user.  
*/
function* mySaga() {  
	yield takeEvery('USER_FETCH_REQUESTED', fetchUser)  
}

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

```js
const sagaMiddleware = createSagaMiddleware()  

// mount it on the Store  
const store = configureStore({  
	reducer,  
	middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(sagaMiddleware),  
})

sagaMiddleware.run(mySaga)
```



[https://redux-saga.js.org/docs/api/#effect-creators](https://redux-saga.js.org/docs/api/#effect-creators) 

[https://jaysoo.ca/2016/01/03/managing-processes-in-redux-using-sagas/](https://jaysoo.ca/2016/01/03/managing-processes-in-redux-using-sagas/) 

  

[https://formidable.com/blog/2017/javascript-power-tools-redux-saga/](https://formidable.com/blog/2017/javascript-power-tools-redux-saga/) 

[https://formidable.com/blog/2017/composition-patterns-in-redux-saga/](https://formidable.com/blog/2017/composition-patterns-in-redux-saga/) 

[https://formidable.com/blog/2017/real-world-redux-saga-patterns/](https://formidable.com/blog/2017/real-world-redux-saga-patterns/) 

  
[https://medium.com/onfido-tech/exploring-redux-sagas-cc1fca2015ee](https://medium.com/onfido-tech/exploring-redux-sagas-cc1fca2015ee)