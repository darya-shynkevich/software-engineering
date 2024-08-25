[“Rendering”](https://react.dev/learn/render-and-commit#step-2-react-renders-your-components) means that React is calling your component, which is a function. The JSX you return from that function is like a snapshot of the UI in time. Its props, event handlers, and local variables were all calculated **using its state at the time of the render.**

When React re-renders a component:
1. React calls your function again.
2. Your function returns a new JSX snapshot.
3. React then updates the screen to match the snapshot your function returned.

```js
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
      }}>+3</button>
    </>
  )
}

// `number` only increments once per click!

<button onClick={() => {  
	setNumber(0 + 1);  
	setNumber(0 + 1);  
	setNumber(0 + 1);  
}}>+3</button>
```

**Setting state only changes it for the _next_ render.**

**A state variable’s value never changes within a render,** even if its event handler’s code is asynchronous.