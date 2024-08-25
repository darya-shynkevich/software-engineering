
### Best Practices

Pure functions only perform a calculation and nothing more. It makes your code easier to understand, debug, and allows React to automatically optimize your components and Hooks correctly.

React is responsible for rendering components and Hooks when necessary to optimize the user experience. It is declarative: you tell React what to render in your component’s logic, and React will figure out how best to display it to your user.

- [Never call component functions directly](https://react.dev/reference/rules/react-calls-components-and-hooks#never-call-component-functions-directly)
- [Never pass around Hooks as regular values](https://react.dev/reference/rules/react-calls-components-and-hooks#never-pass-around-hooks-as-regular-values)
    - [Don’t dynamically mutate a Hook](https://react.dev/reference/rules/react-calls-components-and-hooks#dont-dynamically-mutate-a-hook)
    - [Don’t dynamically use Hooks](https://react.dev/reference/rules/react-calls-components-and-hooks#dont-dynamically-use-hooks)

Hooks are defined using JavaScript functions, but they represent a special type of reusable UI logic with restrictions on where they can be called.
- [Only call Hooks at the top level](https://react.dev/reference/rules/rules-of-hooks#only-call-hooks-at-the-top-level)
- [Only call Hooks from React functions](https://react.dev/reference/rules/rules-of-hooks#only-call-hooks-from-react-functions)



https://react.dev/learn
https://www.codecademy.com/learn/react-101

# Templates

1. https://github.com/hey3/starter-for-react
2. https://github.com/alan2207/bulletproof-react

# References:

1. [React documentation]([https://react.dev/learn](https://react.dev/learn))