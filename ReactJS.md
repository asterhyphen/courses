### Why React?
Component-based development, Reusable UI, State management

---

## Basic React Component
JS fn that returns JSX content

```jsx
function Heading() {
    return <h1>Hello World</h1>;
}

export default Heading;
```

Use it elsewhere

```jsx
function App() {
    return (
        <div>
            <Heading />
        </div>
    );
}

export default App;
```


> [!NOTE] Component names should normally start with an uppercase letter like ` <Heading /> ` as Heading is component and lower case is html elem.


---

# JSX

JSX allows us to write HTML-like syntax inside JavaScript.

```jsx
function App() {
    return (
        <div>
            <h1>Hello World</h1>
            <p>Welcome to React.</p>
        </div>
    );
}
```

JSX is not exactly HTML. It is syntax that gets transformed into JavaScript.

---

# JS Expressions Inside JSX

Use **curly braces `{}`** when you want to put a JavaScript expression inside JSX.

```jsx
function Heading() {
    let title = "This is some heading text";
    return <h1>{title}</h1>;
}
```

Examples:

```jsx
<h1>{name}</h1>
<p>{2 + 3}</p>
<p>{user.name}</p>
<p>{isLoggedIn ? "Welcome" : "Please log in"}</p>
```

---

# JSX Attributes

HTML attributes are used in JSX too, but some names are different like for example:

```jsx
<div className="card">
<div class="card"> //this is wrong
```

---

# Components and Reusability

Suppose we need three cards.
Instead of writing the entire card three times:

```jsx
<div>
    <h2>First card</h2>
    <h3>First description</h3>
</div>
<div>
    <h2>Second card</h2>...
```

we create one reusable component.

```jsx
function Card(props) {
    return (
        <div className="card">
            <h2>{props.h2Val}</h2>
            <h3>{props.h3Val}</h3>
            <b>{props.samosa}</b>
        </div>
    );
}
```

Then reuse it:

```jsx
function App() {
    return (
        <div>
            <Card
                h2Val="First card's h2"
                h3Val="First card's h3"
                samosa="hello"
            />

            <Card
                h2Val="Second card's h2"
                h3Val="Second card's h3"
            />
        </div>
    );
}
```

This is one of the most important ideas in React:
> **Build once, reuse many times.**

---

# Props

**Props** means properties.
Props allow a parent component to pass data to a child component.

Parent:

```jsx
function App() {
    return (
        <Card
            h2Val="Hello"
            h3Val="Welcome"
        />
    );
}
```

Child:

```jsx
function Card(props) {
    return (
        <div>
            <h2>{props.h2Val}</h2>
            <h3>{props.h3Val}</h3>
        </div>
    );
}
```


---

# Props with Objs

Instead of passing individual values separately, data can be stored in an object.

```jsx
const user = {
    name: "Ahmed",
    age: "19
};
```

Then:

```jsx
<PromoHeading
    heading={user.name}
    callToAction={user.age}
/>
```

The child:

```jsx
function PromoHeading(props) {
    return (
        <>
            <h1>{props.heading}</h1>
            <h2>{props.callToAction}</h2>
        </>
    );
}
```

This is useful when a component needs several related pieces of data.

---

# Destructuring Props

```jsx
// Instead of:
function Card(props) {
    return <h2>{props.title}</h2>;
}
// we can destructure:

function Card({ title }) {
    return <h2>{title}</h2>;
}

// For multiple props:

function Card({ title, description }) {
    return (
        <div>
            <h2>{title}</h2>
            <p>{description}</p>
        </div>
    );
}
```

This is mostly a JavaScript syntax improvement.

---

# The `children` Prop

React provides a special prop called `children` for content placed inside a component.

```jsx
function Button({ children }) {
    return <button>{children}</button>;
}
```

Usage:

```jsx
<Button>Click me</Button>
```

Here, `children` is `"Click me"`.

It can also contain multiple elements:

```jsx
function Card({ children }) {
    return <div className="card">{children}</div>;
}
```

```jsx
<Card>
    <h2>Hello</h2>
    <p>This is inside the card.</p>
</Card>
```

### Why use `children`?

It lets wrapper components accept **any content** placed inside them.

---

# Props Drilling

Props drilling happens when data needs to travel through several components just to reach a deeply nested component.

Example:

```text
App
 ↓ props
Header
 ↓ props
Wrapper
 ↓ props
Button
```

Example:

```jsx
function Main(props) {
    return <Header msg={props.msg} />;
}

function Header(props) {
    return <Wrapper msg={props.msg} />;
}

function Wrapper(props) {
    return <Button msg={props.msg} />;
}

function Button(props) {
    return <button>{props.msg}</button>;
}
```

The problem becomes worse as the component tree grows.
If only `Button` needs the data, passing it through every level can become annoying.
This leads to **Context API**.

---

# State

Props come from a parent.
**State belongs to a component and can change over time.**
Example:

```jsx
function App() {
    const [num, setNum] = React.useState(0);
    return (
        <h1>Current Number: {num}</h1>
    );
}
```
---

# `useState`

The basic syntax is:

```jsx
//syn: const [state, setState] = useState(initialValue);
const [num, setNum] = useState(0);
// so num = 0
//later
setNum(1);
```

React updates the state and re-renders the component.

---

#  State Example — Counter

```jsx
import { useState } from "react";

function App() {
    const [num, setNum] = useState(0);

    return (
        <div>
            <h1>Current Number: {num}</h1>
            <button onClick={() => setNum(num + 1)}>
                Increment
            </button>
            <button onClick={() => setNum(num - 1)}>
                Decrement
            </button>
        </div>
    );
}

export default App;
```

---

#  Updating State

```jsx
num = num + 1; // ❌
setNum(num + 1); // ✅
```

For updates based on the previous value, prefer the functional form:

```jsx
setNum(prevNum => prevNum + 1);
```

This is especially useful when multiple updates may happen close together.

---

# State Can Store Different Types

State can store 

```jsx
const [count, setCount] = useState(0); //numbers
const [name, setName] = useState(""); //Strings
const [isOpen, setIsOpen] = useState(false); //Booleans
const [items, setItems] = useState([]); //Arrays

const [user, setUser] = useState({ //objects
    name: "Alex",
    age: 20
});
```

---

# Event Handling

React uses event handler props.

Example:

```jsx
<button onClick={handleClick}>
    Click me
</button>
```

The function:

```jsx
function handleClick() {
    console.log("Clicked");
}
```

### Important

Pass the function:

```jsx
onClick={handleClick}
onClick={handleClick()} // ❌

<button onClick={() => handleClick("hello")}> //with args
    Click me
</button>
```

---

# Common React Events

Some commonly used events:

```jsx
onClick
onDoubleClick
onMouseEnter
onMouseLeave
onMouseOver
onKeyDown //keyboard events
onKeyUp
onChange
onSubmit
```

Example:

```jsx
function Button() {
    const handleMouseOver = () => {
        console.log("Mouse over");
    };

    return (
        <button onMouseOver={handleMouseOver}>
            Move mouse here
        </button>
    );
}
```

---

# Conditional Rendering

```jsx
condition ? valueIfTrue : valueIfFalse
function CurrentImage() {
    const hour = new Date().getHours();
    return (
        <>
            {hour >= 6 && hour <= 18
                ? <Daytime />
                : <Nighttime />
            }
        </>
    );
}
```

---

# Conditional Rendering with State

State can control what is displayed.

```jsx
function ModeToggler() {
    const [darkModeOn, setDarkModeOn] = useState(false);

    const handleClick = () => {
        setDarkModeOn(prev => !prev);
    };

    return (
        <div>
            {darkModeOn
                ? <h1>Dark Mode is On</h1>
                : <h1>Light Mode is On</h1>
            }

            <button onClick={handleClick}>
                Toggle
            </button>
        </div>
    );
}
```

Flow:

```text
darkModeOn = false so Light Mode is On then click and setDarkModeOn(prev => !prev) then darkModeOn = true now Dark Mode is On
```
---
# Passing State Through Props

A parent can own state and pass it to a child.

```jsx
function App() {
    const [word, setWord] = useState("Eat");

    return (
        <div>
            <Heading message={word + " at Little Lemon"} />
        </div>
    );
}
```

Child:

```jsx
function Heading(props) {
    return <h1>{props.message}</h1>;
}
```



---

# Passing Functions as Props

Functions can also be passed as props.

Parent:

```jsx
function App() {
    const handleClick = () => {
        console.log("Clicked!");
    };

    return <Button onClick={handleClick} />;
}
```

Child:

```jsx
function Button({ onClick }) {
    return (
        <button onClick={onClick}>
            Click me
        </button>
    );
}
```

This allows a child to trigger behavior controlled by its parent.

---

# Context API

Context API helps share data across components without manually passing props through every level.

Basic setup:

```jsx
import React from "react";

const MealsContext = React.createContext();

function MealsProvider({ children }) {
    const meals = [
        "Baked Beans",
        "Baked Sweet Potatoes",
        "Baked Potatoes"
    ];

    return (
        <MealsContext.Provider value={{ meals }}>
            {children}
        </MealsContext.Provider>
    );
}

export default MealsProvider;
```

---

# Consuming Context

A component can read the context using `useContext`.

```jsx
const mealsContext = React.useContext(MealsContext);
```

A custom hook makes this cleaner:

```jsx
export const useMealsListContext = () => {
    return React.useContext(MealsContext);
};
```

Then:

```jsx
function MealsList() {
    const { meals } = useMealsListContext();

    return (
        <ul>
            {meals.map(meal => (
                <li key={meal}>{meal}</li>
            ))}
        </ul>
    );
}
```

---

# Context Provider

The Provider makes the value available to descendants.

```jsx
<MealsContext.Provider value={{ meals }}>
    {children}
</MealsContext.Provider>
```

The nested component does not need every parent to manually pass `meals`.


---

# Lists and Rendering Multiple Components
React can render arrays using `.map()`.

```jsx
const meals = [
    "Baked Beans",
    "Baked Sweet Potatoes",
    "Baked Potatoes"
];

function MealsList() {
    return (
        <div>
            {meals.map(meal => (
                <div key={meal}>
                    {meal}
                </div>
            ))}
        </div>
    );
}
```

The `key` helps React identify individual items.

---

# Hooks — Core Idea

Hooks allow function components to use React features.
### Rules of Hooks

Hooks should generally:

1. Be called at the top level of a component or custom hook.
2. Not be called inside loops or conditions.
3. Not be called inside ordinary JavaScript functions.

Good:

```jsx
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

# 51. Context + State

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

# 58. Props vs State vs Context

| Concept | Purpose |
|---|---|
| Props | Pass data from parent to child |
| State | Store changing component data |
| Context | Share data across a component subtree |
| Children | Pass nested JSX into a component |
| Events | Respond to user interaction |
| Hooks | Reuse React functionality/logic |

---

# 59. Common Mistakes

### Mistake 1 — Calling an event handler immediately

```jsx
onClick={handleClick()} // ❌
onClick={handleClick} // ✅
onClick={() => handleClick()} // ✅
```

---
### Mistake 2 — Mutating state

```jsx
items.push(item); // ❌
setItems(prev => [...prev, item]); // ✅
```

---

### Mistake 3 — Forgetting keys

```jsx
items.map(item => (
    <Card />
)) //no
items.map(item => (
    <Card key={item.id} />
))
```

---

### Mistake 4 — Trying to change props

```jsx
props.name = "New name"; // ❌
```

Props should be treated as read-only.

---

### Mistake 5 — Calling hooks conditionally

```jsx
if (loggedIn) {
    useState(false); // ❌
}
```

Hooks should be called consistently at the top level.

---
## React Advanced — Summary of This Set

### 1. `useState`

Used to store **state that can change** inside a component.

```jsx
const [restaurantName, setRestaurantName] = useState("Lemon");

setRestaurantName("Little Lemon");
```

- `restaurantName` → current value
    
- `setRestaurantName` → changes value
    
- Updating state causes the component to **re-render**.
    

### 2. Array Destructuring

`useState()` returns an array containing two values.

```js
const [value, setValue] = useState(10);
```

Same idea as:

```js
const values = useState(10);
const value = values[0];
const setValue = values[1];
```

---

### 3. Updating Object State

When state is an object, preserve the existing properties:

```jsx
setGiftCard(prevState => ({
  ...prevState,
  valid: false,
  text: "Your coupon has been used."
}));
```

`...prevState` copies the old object, then you overwrite the properties that changed.

---

### 4. `useEffect`

Used for **side effects** — things that happen outside normal rendering.

Examples:

- API calls
- Logging
- Timers
- Subscriptions
- DOM interactions

```jsx
useEffect(() => {
  console.log(total);
}, []);
```

Dependency array:

```jsx
useEffect(() => {}, []);
```

- `[]` → runs after initial render
    
- `[value]` → runs when `value` changes
    
- no dependency array → runs after every render
    

**Important:** Don't use `useEffect` for ordinary calculations that can happen during rendering.

---

### 5. Fetching Data with `useEffect`

Typical pattern:

```jsx
const [user, setUser] = useState([]);

useEffect(() => {
  fetch(url)
    .then(response => response.json())
    .then(data => setUser(data));
}, []);
```

Flow:

**Component renders → `useEffect` runs → API request → data arrives → state updates → component re-renders.**

---

### 6. `useReducer`

Useful when state becomes **more complex**.

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

Reducer:

```jsx
const reducer = (state, action) => {
  if (action.type === "ride")
    return { money: state.money + 10 };

  if (action.type === "fuel")
    return { money: state.money - 50 };
};
```

Then:

```jsx
dispatch({ type: "ride" });
```

Think:

**useState:** "Change this value."

**useReducer:** "Perform this action on my state."

---

### 7. `useState` vs `useReducer`

|`useState`|`useReducer`|
|---|---|
|Simple state|Complex state|
|Strings, numbers, booleans|Objects/arrays with multiple transitions|
|Easy setup|More setup|
|Simple updates|Structured state logic|

There isn't a strict rule like "3 properties = useReducer." Choose whichever makes the code easier to understand and maintain.

---

### 8. Context API

Used to share data between components **without passing props through every level**.

Example:

```jsx
const ThemeContext = createContext();

<ThemeContext.Provider value={{ theme, toggleTheme }}>
  {children}
</ThemeContext.Provider>
```

Then another component can access it:

```jsx
const { theme } = useContext(ThemeContext);
```

Good for things like:

- Theme
    
- Logged-in user
    
- Language
    
- Global application data
    

---

### 9. Controlled Components / Forms

React can control form inputs using state:

```jsx
const [name, setName] = useState("");

<input
  value={name}
  onChange={e => setName(e.target.value)}
/>
```

The important pattern is:

**Input → state → input**

This makes React the source of truth.

---

### 10. `preventDefault()`

Forms normally reload/submit the page.

```jsx
const handleSubmit = e => {
  e.preventDefault();
};
```

This prevents the browser's default form behavior so you can handle submission yourself.

---

### 11. Conditional Form Logic

You can validate state before submitting:

```jsx
if (Number(score) <= 5 && comment.length <= 10) {
  alert("Please provide a longer comment.");
  return;
}
```

Then reset:

```jsx
setComment("");
setScore("10");
```

---

### 12. JavaScript `.map()`

Extremely important for React lists.

```jsx
const topDesserts = data.map(dessert => {
  return {
    content: `${dessert.title} - ${dessert.description}`,
    price: dessert.price
  };
});
```

`map()` **transforms every item into a new array**.

Rendering:

```jsx
{meals.map((meal, index) => (
  <h2 key={index}>{meal}</h2>
))}
```

---

### 13. `.filter()` + `.sort()` + `.map()`

You can chain array operations:

```js
data
  .filter(item => item.calories < 500)
  .sort((a, b) => a.calories - b.calories)
  .map(item => ...)
```

Meaning:

**Filter → Sort → Transform → Render**

---

### 14. React Keys

When rendering lists:

```jsx
items.map(item => (
  <div key={item.id}>{item.name}</div>
))
```

`key` helps React identify which list items changed.

Prefer a **stable unique ID** rather than using the array index when possible.

---

## 🧠 The Big Picture

The whole set is basically teaching you how React manages **data and behavior**:

```text
useState
   ↓
Store changing data
   ↓
useEffect
   ↓
Handle side effects / API calls
   ↓
useReducer
   ↓
Manage complicated state logic
   ↓
Context
   ↓
Share state across components
   ↓
Controlled Forms
   ↓
Manage user input
   ↓
map / filter / sort
   ↓
Transform and render data
```

### ⭐ Most important things to remember

**`useState`** → component state  
**`useEffect`** → side effects  
**`useReducer`** → complex state logic  
**`useContext`** → shared state  
**`map()`** → render/transform lists  
**`key`** → identify list elements  
**`preventDefault()`** → stop default form submission  
**Controlled input** → React state controls the input


## React Advanced — Last Set Summary

### 1. `useEffect` for side effects

`useEffect` lets you run code after React renders.

```jsx
useEffect(() => {
  document.title = toggle
    ? "Welcome to Little Lemon"
    : "Using the useEffect hook";
}, []);
```

Common uses:

- Updating the document title
    
- API calls
    
- Event listeners
    
- Timers
    
- Subscriptions
    

---

### 2. `useEffect` + Event Listeners

Example from your mouse-position exercise:

```jsx
useEffect(() => {
  const handleMousePositionChange = (e) => {
    setMousePosition({
      x: e.clientX,
      y: e.clientY
    });
  };

  window.addEventListener("mousemove", handleMousePositionChange);

  return () => {
    window.removeEventListener("mousemove", handleMousePositionChange);
  };
}, []);
```

The important pattern:

```text
useEffect
   ↓
Add event listener
   ↓
Component is active
   ↓
Cleanup function
   ↓
Remove event listener
```

**Cleanup is important** so you don't leave unnecessary listeners running.

---

### 3. Higher-Order Components (HOC)

A HOC is a function that takes a component and returns a **new enhanced component**.

```jsx
const withMousePosition = (WrappedComponent) => {
  return (props) => {
    // state + logic

    return (
      <WrappedComponent
        {...props}
        mousePosition={mousePosition}
      />
    );
  };
};
```

Think:

```text
Component
    ↓
HOC adds behavior/data
    ↓
Enhanced Component
```

This allows multiple components to reuse the same behavior.

---

### 4. Passing Data Through HOCs

The HOC can inject data as props:

```jsx
<WrappedComponent
  {...props}
  mousePosition={mousePosition}
/>
```

Then:

```jsx
const PanelMouseLogger = ({ mousePosition }) => {
  ...
};
```

So the HOC handles the logic, while the component uses the data.

---

### 5. Component Composition + `children`

You can build reusable components that receive other components/elements through `children`.

```jsx
<RadioGroup>
  <RadioOption value="1">Option 1</RadioOption>
  <RadioOption value="2">Option 2</RadioOption>
</RadioGroup>
```

Inside:

```jsx
React.Children.map(children, child => {
  ...
});
```

This lets a parent component dynamically work with its children.

---

### 7. `React.Children.map()`

Useful when working with `children`:

```jsx
React.Children.map(children, (child) => {
  return ...
});
```

It safely handles React's `children` structure instead of assuming it's always a normal array.


---

### 9. Mock Functions

Jest can create mock functions:

```jsx
const handleSubmit = jest.fn();
```

Then you can check whether it was called:

```jsx
expect(handleSubmit).toHaveBeenCalledWith({
  score,
  comment
});
```

Very useful for testing whether your component correctly calls functions.

---