Hooks allow function components to use React features.
### Rules of Hooks

Hooks should generally:
1. Be called at the top level of a component or custom hook.
2. Not be called inside loops or conditions.
3. Not be called inside ordinary JavaScript functions.

```jsx
//Good:
function App() {
    const [count, setCount] = useState(0);
    return <h1>{count}</h1>;
}
//Bad:
if (something) {
    const [count, setCount] = useState(0);
}
```

---

# `useEffect`

`useEffect` is used for synchronizing a component with external systems or performing side effects.

```jsx
import { useEffect } from "react";

useEffect(() => {
    console.log("Component rendered");
}, []);
```

Common uses include:

- Fetching data
- Subscribing to external systems
- Timers
- Browser APIs
- Synchronizing with external state

---

# Cleanup Functions

Some effects need cleanup.

```jsx
useEffect(() => {
    const id = setInterval(() => {
        console.log("tick");
    }, 1000);

    return () => {
        clearInterval(id);
    };
}, []);
```
- Timers
- Event listeners
- Subscriptions
- Connections

---

# `useRef`

`useRef` stores a mutable value that does not cause a re-render when changed.
It is also commonly used to access a DOM element.

```jsx
import { useRef } from "react";

function App() {
    const inputRef = useRef(null);

    const focusInput = () => {
        inputRef.current.focus();
    };

    return (
        <>
            <input ref={inputRef} />

            <button onClick={focusInput}>
                Focus
            </button>
        </>
    );
}
```

---

# `useReducer`

For more complex state logic, `useReducer` can be useful.

```jsx
const initialState = { count: 0 };

function reducer(state, action) {
    switch (action.type) {
        case "increment":
            return {
                count: state.count + 1
            };

        case "decrement":
            return {
                count: state.count - 1
            };

        default:
            return state;
    }
}
```

Then:

```jsx
const [state, dispatch] = useReducer(
    reducer,
    initialState
);
```

And:

```jsx
dispatch({ type: "increment" });
```

---
#  `useMemo`

`useMemo` can cache the result of an expensive calculation.
Do not use it everywhere automatically.
Use performance optimizations when there is a real reason.

---
# Immutability

React state should be treated as immutable.

For arrays, avoid:

```jsx
items.push(newItem); // ❌
//Instead:
setItems(prev => [
    ...prev,
    newItem
]);
//For removing an item:
setItems(prev =>
    prev.filter(item => item.id !== id)
);
//For updating an object:
setUser(prev => ({
    ...prev,
    name: "Alex"
}));
```

The spread operator creates a new object/array rather than mutating the existing state.

---

# Context + State

Context becomes especially powerful when combined with state.

```jsx
const AppContext = createContext(null);

function AppProvider({ children }) {
    const [theme, setTheme] = useState("light");

    const toggleTheme = () => {
        setTheme(prev =>
            prev === "light" ? "dark" : "light"
        );
    };

    return (
        <AppContext.Provider
            value={{ theme, toggleTheme }}
        >
            {children}
        </AppContext.Provider>
    );
}
```

---

# Custom Hooks

A custom hook is a reusable function whose name begins with `use`.

Example:

```jsx
function useCounter() {
    const [count, setCount] = useState(0);

    const increment = () => {
        setCount(prev => prev + 1);
    };

    return {
        count,
        increment
    };
}
function Counter() {
    const { count, increment } = useCounter();

    return (
        <>
            <h1>{count}</h1>
            <button onClick={increment}>
                Add
            </button>
        </>
    );
}
```
---
