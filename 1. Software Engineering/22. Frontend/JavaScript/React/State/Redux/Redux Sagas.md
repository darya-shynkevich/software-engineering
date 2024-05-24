What a Saga does is actually compose all  Effects together to implement the desired control flow. The most basic example is to sequence yielded Effects by putting the yields one after another. You can also use the familiar control flow operators (`if`, `while`, `for`) to implement more sophisticated control flows.

An Effect is an object that contains some information to be interpreted by the middleware. You can view Effects like instructions to the middleware to perform some operation (e.g., invoke some asynchronous function, dispatch an action to the store, etc.).

1. `take(pattern)` Creates an Effect description that instructs the middleware to wait for a specified action on the Store. The Generator is suspended until an action that matches `pattern` is dispatched.
   The middleware provides a special action `END`. If you dispatch the END action, then all Sagas blocked on a `take` Effect will be terminated regardless of the specified pattern. If the terminated Saga has still some forked tasks which are still running, it will wait for all the child tasks to terminate before terminating the Task.
2. `takeMaybe(pattern)` Same as `take(pattern)` but does not automatically terminate the Saga on an `END` action. Instead all Sagas blocked on a `take` Effect, will get the `END` object.
3. `takeEvery(pattern, saga, ...args)` Spawns a `saga` on each action dispatched to the Store that matches `pattern`.
   `takeEvery` is a high-level API built using `take` and `fork`.
4. `takeLatest(pattern, saga, ...args)` Forks a `saga` on each action dispatched to the Store that matches `pattern`. And automatically cancels any previous `saga` task started previously if it's still running.
   Each time an action is dispatched to the store. And if this action matches `pattern`, `takeLatest` starts a new `saga` task in the background. If a `saga` task was started previously (on the last action dispatched before the actual action), and if this task is still running, the task will be cancelled.
    can be useful to handle AJAX requests where we want to only have the response to the latest request.
5. `takeLeading(pattern, saga, ...args)`Spawns a `saga` on each action dispatched to the Store that matches `pattern`. After spawning a task once, it blocks until spawned saga completes and then starts to listen for a `pattern` again.
   In short, `takeLeading` is listening for the actions when it doesn't run a saga.
6. `put(action)` Creates an Effect description that instructs the middleware to schedule the dispatching of an action to the store. This dispatch may not be immediate since other tasks might lie ahead in the saga task queue or still be in progress.
   You can, however, expect the store to be updated in the current stack frame (i.e. by the next line of code after `yield put(action)`) unless you have other Redux middlewares with asynchronous flows that delay the propagation of the action.
7. `putResolve(action)` Just like [`put`](https://redux-saga.js.org/docs/api/#putaction) but the effect is blocking (if promise is returned from `dispatch` it will wait for its resolution) and will bubble up errors from downstream.
8. `call(fn, ...args` Creates an Effect description that instructs the middleware to call the function `fn` with `args` as arguments.
9. `call([context, fn], ...args)` Same as `call(fn, ...args)` but supports passing a `this` context to `fn`. This is useful to invoke object methods.
10. `join(task)` Creates an Effect description that instructs the middleware to wait for the result of a previously forked task. `join` will resolve to the same outcome of the joined task (success or error). If the joined task is cancelled, the cancellation will also propagate to the Saga executing the join effect. Similarly, any potential callers of those joiners will be cancelled as well.
		`task: Task` - A [Task](https://redux-saga.js.org/docs/api/#task) object returned by a previous `fork`
11. `select(selector, ...args)` Creates an effect that instructs the middleware to invoke the provided selector on the current Store's state (i.e. returns the result of `selector(getState(), ...args)`).
	    If `select` is called without argument (i.e. `yield select()`) then the effect is resolved with the entire state (the same result of a `getState()` call).

## Effect combinators

### `race(effects)`

Creates an Effect description that instructs the middleware to run a _Race_ between multiple Effects (this is similar to how [`Promise.race([...])`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise/race) behaves).

When resolving a `race`, the middleware automatically cancels all the losing Effects.

### `all([...effects]) - parallel effects`

Creates an Effect description that instructs the middleware to run multiple Effects in parallel and wait for all of them to complete.

