# useState 

is a React Hook that enables functional components to manage and update state, which is data that can change over time and trigger UI re-renders.

## Here's a breakdown of useState:

Purpose: It allows functional components to "remember" data and react to changes in that data by re-rendering. Before hooks, only class components could manage state using this.state and this.setState.
Import: You must import useState from React:
JavaScript
```js
    import React, { useState } from 'react';
```
Usage: useState is called inside a functional component and returns an array with two elements, typically destructured for convenience:
JavaScript
```ja
    const [stateVariable, setStateVariable] = useState(initialState);
```
stateVariable: This is the current value of your state.
setStateVariable: This is a function used to update the stateVariable. Calling this function will trigger a re-render of the component with the new state value.
initialState: This is the initial value you want to assign to your state. It can be any data type (string, number, boolean, array, object, etc.).
Updating State: To change the value of stateVariable, you must call the setStateVariable function with the new value. Directly modifying stateVariable will not trigger a re-render and is an anti-pattern.
JavaScript
```js
    // Example: Incrementing a counter
    const [count, setCount] = useState(0);

    const increment = () => {
      setCount(count + 1); // Updates the 'count' state and triggers a re-render
    };
```
Re-renders: When setStateVariable is called, React re-renders the component with the updated state, ensuring the UI reflects the latest data.
useState is a fundamental building block for creating interactive and dynamic user interfaces in React functional components.