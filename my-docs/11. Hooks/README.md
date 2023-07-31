# React Hooks 🎣

React Hooks are a powerful feature introduced in React 16.8 that allows developers to add state and other React features to functional components. Prior to the introduction of hooks, state management and other complex logic could only be implemented in class components using lifecycle methods. With hooks, developers can now write more concise and reusable code by leveraging functions instead of classes.

<hr>

Hooks provide several benefits in React development. Firstly, they simplify the management of component state. With the "useState" hook, you can add state to a functional component, eliminating the need for a class-based component. This results in cleaner and more readable code.

Secondly, hooks enable the reuse of logic across components. The "useEffect" hook allows you to perform side effects such as data fetching, subscriptions, or manually changing the DOM after rendering. By encapsulating such logic in a custom hook, you can easily share it across different components, improving code maintainability and reducing duplication.

Another advantage of hooks is the ability to create custom hooks. Custom hooks are functions that combine multiple built-in hooks and encapsulate complex logic into reusable units. This promotes code modularity and makes it easier to share code between projects or within a team.

Here is a list of some commonly used React hooks:


- **<a href="#useState">useState</a>:** Manages state within functional components.
- **<a href="#useEffect">useEffect</a>:** Performs side effects after rendering, such as data fetching or subscriptions.
- **<a href="#useCallback">useCallback</a>:** Memoizes a function to prevent unnecessary re-rendering of components that depend on it.
- **<a href="#useMemo">useMemo</a>:** Memoizes a value to prevent expensive computations on every render.
- **<a href="#useRef">useRef</a>:** Creates a mutable ref object that persists across renders.
- **<a href="#useTransition">useTransition</a>:** Lets you update the state without blocking the UI.
- **<a href="#useContext">useContext</a>:** Lets you read and subscribe to <a href="https://react.dev/learn/passing-data-deeply-with-context">context</a> from your component.
- **<a href="#useReducer">useReducer</a>:** Alternative to useState for managing complex state logic.
- ```useLayoutEffect```: Similar to useEffect but fires synchronously after all DOM mutations.
- ```useImperativeHandle```: Customizes the instance value that is exposed to parent components when using ref.
- ```useDebugValue```: Provides a label for custom hooks in React DevTools.

These hooks cover a wide range of functionality and help streamline the development process in React. 
Below, we'll dive into each React hook in more detail. Let's start exploring each hook and its specific use cases:


<div id="useState"></div>

## useState 🟤
The useState hook is a built-in hook provided by React that allows you to add state to functional components. With this hook, you can create and manage state variables within a functional component, giving it the ability to hold and update data.

The syntax for using useState is as follows:
```jsx
const [state, setState] = useState(initialValue);
```
Let's break down this syntax:

```state``` represents the current value of the state variable.
```setState``` is a function that allows you to update the state variable.
```initialValue``` is the initial value of the state variable.
Now, let's explore some examples to understand how useState works:

Example 1: Counter

```jsx
import React, { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
};
```
In this example, we create a state variable called count and initialize it with 0 using useState(0). The setCount function is used to update the count variable. When the "Increment" button is clicked, the increment function is called, which increments the count by 1. The updated value is then reflected in the component's rendering.

Example 2: Input Field

```jsx
import React, { useState } from 'react';

const InputField = () => {
  const [text, setText] = useState('');

  const handleChange = (event) => {
    setText(event.target.value);
  };

  return (
    <div>
      <input type="text" value={text} onChange={handleChange} />
      <p>Typed text: {text}</p>
    </div>
  );
};
```
In this example, we create a state variable called text and initialize it with an empty string using useState(''). The setText function is used to update the text variable whenever the input field's value changes. The onChange event handler is called, which invokes handleChange and updates the text state with the typed value. The typed text is then displayed below the input field.

These examples demonstrate how useState enables you to add and manage state in functional components. By using the setState function, you can update the state variable, triggering a re-render of the component and reflecting the updated state in the UI.

Remember that you can use multiple useState hooks within a single component to manage multiple independent state variables. Each state variable will have its own corresponding setState function.


<div id="useEffect"></div>

## useEffect ⚪️
*useEffect* is a hook in React that allows you to perform side effects in functional components. Side effects can include things like fetching data from an API, subscribing to events, or updating the document title. It's called a "side effect" because it doesn't directly affect the rendering of your component, but rather deals with other interactions outside of it.

Here's a breakdown of how to use useEffect:

Import the useEffect hook from the React library:

```jsx
import React, { useEffect } from 'react';
```
Use the useEffect hook inside your functional component:

```jsx
function MyComponent() {
  useEffect(() => {
    // Side effect code goes here
  });

  return (
    // JSX code for your component
  );
}
```

The first argument of useEffect is a function that will be called after the component renders. This function is where you can place your side effect code. It runs every time the component renders by default.

```jsx
useEffect(() => {
  // Side effect code
});
```

You can also provide a second argument to useEffect that determines when the effect should run. This is an optional array of dependencies. If any of the dependencies change between renders, the effect will be triggered again. If the dependency array is empty, the effect will only run once when the component mounts, and not again.

```jsx
useEffect(() => {
  // Side effect code
}, [dependency1, dependency2]);
```
Now let's look at some examples:

1. Fetching data from an API:
```jsx
useEffect(() => {
  fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => {
      // Do something with the fetched data
    });
}, []);
```
In this example, the effect runs only once when the component mounts because the dependency array is empty.

2. Subscribing to an event:
```jsx
useEffect(() => {
  const handleClick = () => {
    // Handle the click event
  };

  window.addEventListener('click', handleClick);

  return () => {
    window.removeEventListener('click', handleClick);
  };
}, []);
```
Here, the effect adds an event listener for the click event when the component mounts, and removes it when the component unmounts. The empty dependency array ensures that the effect only runs once.


<div id="useCallback"></div>

## useCallback ⚫️

**useCallback** is a React hook that allows you to optimize the performance of your React components by memoizing a function. In simple terms, it helps you avoid unnecessary re-creation of functions on each render.

When you define a function inside a component, it gets recreated every time the component re-renders, even if the function itself hasn't changed. This can lead to performance issues, especially if you pass that function as a prop to child components, as it may cause unnecessary re-renders in those components too.

Here's where useCallback comes in handy. It memoizes a function and returns a memoized version of it. The memoized version will only change if any of the dependencies provided to useCallback change. If the dependencies remain the same, useCallback will return the same memoized function from the previous render.

Here's an example to illustrate its usage:

```jsx
import React, { useState, useCallback } from 'react';

const CounterButton = ({ onClick }) => {
  console.log('CounterButton rendering');
  return (
    <button onClick={onClick}>
      Click me!
    </button>
  );
};

const Counter = () => {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  console.log('Counter rendering');

  return (
    <div>
      <p>Count: {count}</p>
      <CounterButton onClick={handleClick} />
    </div>
  );
};
```

In the example above, we have a Counter component that renders a CounterButton component. Whenever the Counter component re-renders, it will create a new handleClick function because it references the count state value. However, by using **useCallback**, we can optimize this behavior.

By providing [count] as the dependency array to useCallback, it ensures that the handleClick function will only be re-created if the count value changes. If count remains the same, useCallback will return the previously memoized handleClick function.

This optimization can be helpful when passing handleClick as a prop to child components because it prevents unnecessary re-renders in those components if the dependencies haven't changed.

Remember to include all the dependencies that the function relies on in the dependency array to ensure the correct behavior of **useCallback**.


<div id="useMemo"></div>

## useMemo ⚫️

**useMemo** is another React hook that allows you to optimize the performance of your React components by memoizing a value. It's similar to **useCallback**, but instead of memoizing a function, it memoizes a computed value.

In React, there are situations where you may have expensive computations or calculations that are performed within a component. These computations can take up processing power and slow down your application, especially if they are re-executed on every render, even if the dependencies haven't changed.

Here's where useMemo comes in handy. It allows you to memoize the result of a computation and only recompute it if the dependencies provided to useMemo have changed. If the dependencies remain the same, useMemo will return the previously memoized value from the previous render, saving unnecessary calculations.

Here's an example to illustrate its usage:

```jsx
import React, { useState, useMemo } from 'react';

const ExpensiveComponent = ({ value }) => {
  const expensiveResult = useMemo(() => {
    console.log('Performing expensive computation...');
    // Perform some complex calculations based on the value
    return value * 2;
  }, [value]);

  console.log('ExpensiveComponent rendering');

  return (
    <div>
      <p>Value: {value}</p>
      <p>Expensive Result: {expensiveResult}</p>
    </div>
  );
};

const App = () => {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);
  };

  console.log('App rendering');

  return (
    <div>
      <button onClick={handleClick}>Increment</button>
      <ExpensiveComponent value={count} />
    </div>
  );
};
```

In the example above, we have an **ExpensiveComponent** that performs some computationally expensive calculations based on the value prop it receives. Whenever the **ExpensiveComponent** re-renders, it would recompute the expensive result, even if the value prop hasn't changed. This can impact performance if the calculations are complex.

By using useMemo, we can optimize this behavior. By providing [value] as the dependency array to useMemo, it ensures that the expensive computation will only be re-executed if the value prop changes. If the value remains the same, useMemo will return the previously memoized value.

This optimization can be helpful in scenarios where you have expensive computations, heavy data processing, or complex transformations that depend on certain values. By memoizing the result with useMemo, you can avoid unnecessary recalculations and improve the performance of your components.

Remember to include all the dependencies that the computation relies on in the dependency array to ensure the correct behavior of **useMemo**.


<div id="useRef"></div>

## useRef 🔴
useRef is a React hook that allows you to create a mutable value that persists across component renders. It provides a way to store and access a value that **won't trigger a re-render** when it changes.

Here are a few important things to understand about *useRef*:

1. Creating a Ref:
You can create a ref by calling the useRef hook and passing an initial value as an argument:

```jsx
const myRef = useRef(initialValue);
```

2. Mutable Reference:
The value stored in a ref is mutable, meaning you can update it directly without triggering a re-render of the component.

3. Preserving Value across Renders:
Unlike regular variables or state values, the value stored in a ref persists across component renders. When the component re-renders, the ref will retain its value from the previous render.

4. Accessing the Value:
You can access the current value of a ref using the ```.current``` property. For example:

```jsx
console.log(myRef.current);
```

5. Updating the Value:

You can update the value stored in a ref using the ```.current``` property. For example:

```jsx
myRef.current = newValue;
```

Here's an example to illustrate its usage:

```jsx
import React, { useRef } from 'react';

const TextInput = () => {
  const inputRef = useRef(null);

  const handleClick = () => {
    // Accessing the current value
    console.log(inputRef.current.value);
    
    // Updating the value
    inputRef.current.value = 'New Value';
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={handleClick}>Log Value</button>
    </div>
  );
};
```

In the example above, we create a ref using **useRef** and assign it to inputRef. We then attach this ref to an input element using the ref attribute. This allows us to access the input's value directly through inputRef.current.value.

In the handleClick function, we log the current value of the input to the console. We can also directly update the value of the input by assigning a new value to inputRef.current.value.

useRef is particularly useful when you need to interact with DOM elements directly or store mutable values that won't trigger a re-render. It can also be used to store and access any other mutable data throughout the lifecycle of a component.

Remember that updating the value of a ref using ```.current``` doesn't trigger a re-render. If you want to update the value and cause a re-render, you should use state (useState) instead.


<div id="useTransition"></div>

## useTransition ⚪️

*useTransition* is a React Hook that lets you update the state without blocking the UI.

The useTransition hook allows us to specify some state updates as not as important. These state updates will be executed in parallel with other state updates, but the rendering of the component will not wait for these less important state updates.

```jsx
function App() {
  const [name, setName] = useState("")
  const [list, setList] = useState(largeList)
  const [isPending, startTransition] = useTransition()

  function handleChange(e) {
    setName(e.target.value)
    startTransition(() => {
      setList(largeList.filter(item => item.name.includes(e.target.value)))
    })
  }

  return (
    <>
      <input type="text" value={name} onChange={handleChange} />
      {isPending ? (
        <div>Loading...</div>
      ) : (
        list.map(item => <ListComponent key={item.id} item={item} />)
      )}
    </>
  )
}
```

Calling the *useTransition* hook returns an array with the first value being an *isPending* variable and the second value being the *startTransition* function. The *isPending* variable simply returns true while the code inside the *startTransition* hook is running. Essentially, this variable is true when the slow state update is running and false when it is finished running. The *startTransition* function takes a single callback and this callback just contains all the code related to the slow state update including the setting of the state.

In our case we are wrapping *setList* in our *startTransition* function which tells React that our *setList* state update is of low importance. This means that as soon as all of our normal state updates are finished that the component should rerender even if this slow state update is not finished. Also, if a user interacts with your application, such as clicking a button or typing in an input, those interactions will take higher priority than the code in the *startTransition* function. This ensures that even if you have really slow code running it won’t block your user from interacting with the application.

In conclusion, the *useTransition* hook makes working with slow, computationally intense state updates so much easier since now we can tell React to prioritize those updates at a lower level to more important updates which makes your application seem much more performant to users.


<div id="useContext"></div>

## useContext 🔴

React context provides data to components no matter how deep they are in the components tree. The context is used to manage global data, e.g. global state, theme, services, user settings, and more.

###  How to use the context ? 🤓
Using the context in React requires 3 simple steps: creating the context, providing the context, and consuming the context.

- A. Creating the context

The built-in factory function createContext(default) creates a context instance:

```jsx
// context.js
import { createContext } from 'react';

export const Context = createContext('Default Value');
```

The factory function accepts one optional argument: the **default** value.

- B. Providing the context

**Context.Provider** component available on the context instance is used to provide the context to its child components, no matter how deep they are.

To set the value of context use the **value** prop available on the **<Context.Provider value={value} />** :

```jsx
import { Context } from './context';

function Main() {
  const value = 'My Context Value';
  return (
    <Context.Provider value={value}>
      <MyComponent />
    </Context.Provider>
  );
}
```

Again, what's important here is that all the components that'd like later to consume the context have to be wrapped inside the provider component.

If you want to change the context value, simply update the value prop.

- C. Consuming the context

Consuming the context can be performed in 2 ways.

The first way, the one I recommend, is to use the ```useContext(Context)``` React hook:

```jsx
import { useContext } from 'react';
import { Context } from './context';

function MyComponent() {
  const value = useContext(Context);

  return <span>{value}</span>;
}
```

The hook returns the value of the context: value = useContext(Context). The hook also makes sure to re-render the component when the context value changes.

The second way is by using a render function supplied as a child to Context.Consumer special component available on the context instance:

```jsx
import { Context } from './context';

function MyComponent() {
  return (
    <Context.Consumer>
      {value => <span>{value}</span>}
    </Context.Consumer>
  );
}
```

Again, in case the context value changes, **<Context.Consumer>** will re-call its render function.

<img width="500" alt="Screenshot 2023-07-29 at 8 11 06 AM" src="https://github.com/mutasim77/ReactJS-Notebook/assets/96326525/ec4f7e4c-24e4-49ee-96b3-a7f4b6f2d8cd">

You can have as many consumers as you want for a single context. If the context value changes (by changing the value prop of the provider <Context.Provider value={value} />), then all consumers are immediately notified and re-rendered.

If the consumer isn't wrapped inside the provider, but still tries to access the context value (using useContext(Context) or *<Context.Consumer>)*, then the value of the context would be the default value argument supplied to *createContext(defaultValue)* factory function that had created the context.

### When do you need context? 🤓

The main idea of using the context is to allow your components to access global data and re-render when that global data is changed. Context solves the props drilling problem: when you have to pass down props from parents to children.

You can hold inside the context:

- global state
- theme
- application configuration
- authenticated user name
- user settings
- preferred language
- a collection of services

On the other side, you should think carefully before deciding to use context in your application.

First, integrating the context adds complexity. Creating a context, wrapping everything in a provider, and using the useContext() in every consumer — increases complexity.

Secondly, adding context complicates unit testing of components. During testing, you'll have to wrap the consumer components into a context provider. Including the components that are indirectly affected by the context — the ancestors of context consumers!

### Conclusion 🤓
The context in React lets you supply child components with global data, no matter how deep they are in the components tree.

Using the context requires 3 steps: creating, providing, and consuming the context.

When integrating the context into your application, consider that it adds a good amount of complexity. Sometimes drilling the props through 2-3 levels in the hierarchy isn't a big problem.



<div id="useReducer"></div>

## useReducer 🟤

**useReducer** is one of the additional Hooks that shipped with React v16.8. An alternative to the useState Hook, useReducer helps you manage complex state logic in React applications. When combined with other Hooks like **useContext**, **useReducer** can be a good alternative to **Redux, Recoil, or MobX**. In certain cases, it is an outright better option.

While Redux, Recoil, and MobX are usually the best options for managing global states in large React applications, more often than necessary, many React developers jump into these third-party state management libraries when they could have effectively handled their state with Hooks.

### How does the useReducer Hook work? 🤓

The **useReducer** Hook is used to store and update states, just like the useState Hook. It accepts a reducer function as its first parameter and the initial state as the second. useReducer returns an array that holds the current state value and a dispatch function to which you can pass an action and later invoke it. While this is similar to the pattern Redux uses, there are a few differences.

For example, the useReducer function is tightly coupled to a specific reducer. We dispatch action objects to that reducer only, whereas in Redux, the dispatch function sends the action object to the store. At the time of dispatch, the components don’t need to know which reducer will process the action.

For those who may be unfamiliar with Redux, we’ll explore this concept a bit further. There are three main building blocks in Redux:

- Store: An immutable object that holds the application’s state data
- Reducer: A function that returns some state data, triggered by an action type
- Action: An object that tells the reducer how to change the state. It must contain a type property and can contain an optional payload property


Let’s see how these building blocks compare to managing state with the useReducer Hook. Below is an example of a store in Redux:

```jsx
import { createStore } from 'redux'

const store = createStore(reducer, [preloadedState], [enhancer])

```

In the code below, we initialize state with the useReducer Hook:

```jsx
const initialState = { count: 0 }

const [state, dispatch] = useReducer(reducer, initialState)
```

**useReducer** doesn’t use the (state = initialState, action) => newState **Redux** pattern, so its reducer function works a bit differently. The code below shows how you’d create reducers with React’s useReducer:

```jsx
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}  
```

### Specifying the initial state
The initial state is the second argument passed to the useReducer Hook, which represents the default state:

```jsx
const initialState = { count: 1 }

// wherever our useReducer is located
const [state, dispatch] = useReducer(reducer, initialState, initFunc)
```

If you don’t pass a third argument to **useReducer**, it will take the second argument as the initial state. The third argument, which is the init function, is optional. This pattern also follows one of the golden rules of Redux state management: the state should be updated by emitting actions. Never write directly to the state.

However, it’s worth noting that the Redux state = initialState convention doesn’t work the same way with useReducer because the initial value sometimes depends on props.

### Creating the initial state lazily

In programming, lazy initialization is the tactic of delaying the creation of an object, the calculation of a value, or some other expensive process until the first time it is needed.

As mentioned above, useReducer can accept a third parameter, which is an optional init function for creating the initial state lazily. It lets you extract logic for calculating the initial state outside of the reducer function, as seen below:

```jsx
const initFunc = (initialCount) => {
    if (initialCount !== 0) {
        initialCount=+0
    }
  return {count: initialCount};
}

// wherever our useReducer is located
const [state, dispatch] = useReducer(reducer, initialCount, initFunc);
```

If the value is not 0 already, initFunc above will reset initialCount to 0 on page mount, then return the state object. Notice that this initFunc is a function, not just an array or object.

### The dispatch method

The dispatch function accepts an object that represents the type of action we want to execute when it is called. Basically, it sends the type of action to the reducer function to perform its job, which, of course, is updating the state.

The action to be executed is specified in our reducer function, which in turn, is passed to the useReducer. The reducer function will then return the updated state.

The actions that will be dispatched by our components should always be represented as one object with the type and payload key, where type stands as the identifier of the dispatched action and payload is the piece of information that this action will add to the state. dispatch is the second value returned from the useReducer Hook and can be used in our JSX to update the state:

```jsx
// creating our reducer function
function reducer(state, action) {
  switch (action.type) {
   // ...
      case 'reset':
          return { count: action.payload };
    default:
      throw new Error();
  }
}

// wherever our useReducer is located
const [state, dispatch] = useReducer(reducer, initialCount, initFunc);

// Updating the state with the dispatch functon on button click
<button onClick={() => dispatch({type: 'reset', payload: initialCount})}> Reset </button>
```

Notice how our reducer function uses the payload that is passed from the dispatch function. It sets our state object to the payload, i.e., whatever the initialCount is. Note that we can pass the dispatch function to other components through props, which alone is what allow us to replace Redux with useReducer.

Let’s say we have a component that we want to pass as props to our dispatch function. We can easily do that from the parent component:

```jsx
<Increment count={state.count} handleIncrement={() => dispatch({type: 'increment'})}/>
```

Now, in the child component, we receive the props, which, when emitted, will trigger the dispatch function and update the state:

```jsx
<button onClick={handleIncrement}>Increment</button>
```

### When not to use the useReducer Hook
Despite being able to use the useReducer Hook to handle complex state logic in our app, it’s important to note that there are some scenarios where a third-party state management library like **Redux** may be a better option:

- When your application needs a single source of truth
- When you want a more predictable state
- When state-lifting to the top-level component no longer suffices
- When you need to persist state even after a page refresh
  
With all these benefits, it’s also worth noting that using a library like Redux, as opposed to using pure React with useReducer, comes with some tradeoffs. For example, Redux has a hefty learning curve that is minimized by using **Redux Toolkit**, and it’s definitely not the fastest way to write code. Rather, it’s intended to give you an absolute and predictable way of managing state in your app.