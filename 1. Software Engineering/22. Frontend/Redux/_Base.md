
A frontend application architecture using Redux consists of

1. A store holding an _immutable_ application’s state.
2. The UI components that use the application state to render itself. These components can be a pure functions, only caring about producing the right presentational content based on the state.
3. Components that can _dispatch_ (send) actions to the store.
4. The state is mutated using _reducers_ which are simple pure functions to produce new state.