
All of your application's state is stored in an object inside **a single store**. The only way to change the state tree is to call an **action***, an object describing what happened. To specify how actions transform the state tree, you write pure "**reducers**".

A frontend application architecture using Redux consists of
1. A store holding an _immutable_ application’s state.
2. The UI components that use the application state to render itself. These components can be a pure functions, only caring about producing the right presentational content based on the state.
3. Components that can _dispatch_ (send) actions to the store.
4. The state is mutated using _reducers_ which are simple pure functions to produce new state.

**The patterns and tools provided by Redux make it easier to understand when, where, why, and how the state in your application is being updated, and how your application logic will behave when those changes occur**. Redux guides you towards writing code that is predictable and testable, which helps give you confidence that your application will work as expected.

Redux is more useful when:
- You have large amounts of application state that are needed in many places in the app
- The app state is updated frequently over time
- The logic to update that state may be complex
- The app has a medium or large-sized codebase, and might be worked on by many people

The Redux was designed to tackle problem of shared state. This problem can be also solved by ["lifting state up"](https://react.dev/learn/sharing-state-between-components) to parent components. Another way to solve this problem is **to extract the shared state from the components, and put it into a centralized location outside the component tree**. With this, our component tree becomes a big "view", and any component can access the state or trigger actions, no matter where they are in the tree!

This is the basic idea behind Redux: **a single centralized place to contain the global state in your application, and specific patterns to follow when updating that state to make the code predictable.**
# [Concepts](Concepts.md)

# [Redux Application Data Flow](Redux%20Application%20Data%20Flow.md)

# References:

1. [When (and when not) to reach for Redux](https://changelog.com/posts/when-and-when-not-to-reach-for-redux)
2. [The Tao of Redux, Part 1 - Implementation and Intent](https://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-1/)
3. [Redux FAQ: When should I use Redux?](https://redux.js.org/faq/general#when-should-i-use-redux)
4. [You Might Not Need Redux](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)
5. [Immutability in React and Redux: The Complete Guide](https://daveceddia.com/react-redux-immutability-guide/)