# React.js Notes — Basics to Advanced

> **Purpose:** A single evolving React.js reference based on the lessons and screenshots provided so far.  
> More screenshots can be added later and this file can be extended without changing the structure.

---

# 1. What is React?

React is a JavaScript library for building user interfaces, especially interactive web applications.

The main idea is to build the UI from **small, reusable components**.

Instead of writing one huge HTML page:

```text
App
 ├── Header
 ├── Navigation
 ├── Content
 │    ├── Card
 │    ├── Card
 │    └── Card
 └── Footer
```

Each part can be a React component.

### Why React?

- Component-based development
- Reusable UI
- Declarative programming
- Efficient UI updates
- State management
- Event handling
- Easy composition of complex interfaces

---

# 2. A Basic React Component

A React component can be a JavaScript function that returns JSX.

```jsx
function Heading() {
    return <h1>Hello World</h1>;
}

export default Heading;
```

Then it can be used inside another component:

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

### Important

Component names should normally start with an uppercase letter.

```jsx
<Heading />
```

React treats `Heading` as a component.

Lowercase names are generally interpreted as HTML elements:

```jsx
<div />
<p />
<button />
```

---

# 3. JSX

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

# 4. JavaScript Expressions Inside JSX

Use **curly braces `{}`** when you want to put a JavaScript expression inside JSX.

```jsx
function Heading() {
    let title = "This is some heading text";

    return <h1>{title}</h1>;
}
```

Without curly braces:

```jsx
<h1>title</h1>
```

React displays the literal text:

```text
title
```

With curly braces:

```jsx
<h1>{title}</h1>
```

React evaluates the JavaScript expression and displays:

```text
This is some heading text
```

### Rule

```jsx
{ JavaScript expression }
```

Examples:

```jsx
<h1>{name}</h1>

<p>{2 + 3}</p>

<p>{user.name}</p>

<p>{isLoggedIn ? "Welcome" : "Please log in"}</p>
```

---

# 5. JSX Attributes

HTML attributes are used in JSX too, but some names are different.

```jsx
<img src={avatar} />
```

For CSS classes, React uses:

```jsx
<div className="card">
```

instead of:

```html
<div class="card">
```

---

# 6. Rendering Images / Assets

An image can be imported as an asset.

Example:

```jsx
import cat from "./assets/images/cat.jpg";

function ShowAnimal() {
    return (
        <div>
            <img
                src={cat}
                alt="A picture of a cat"
            />
        </div>
    );
}

export default ShowAnimal;
```

The important idea is:

```jsx
import cat from "./assets/images/cat.jpg";
```

and then:

```jsx
<img src={cat} alt="A picture of a cat" />
```

### Another path-based approach

Depending on the React/build setup, assets may also be referenced through a path or imported dynamically.

The safest modern approach in bundler-based React applications is generally to import assets:

```jsx
import logo from "./assets/logo.png";
```

---

# 7. Components and Reusability

Suppose we need three cards.

Instead of writing the entire card three times:

```jsx
<div>
    <h2>First card</h2>
    <h3>First description</h3>
</div>

<div>
    <h2>Second card</h2>
    <h3>Second description</h3>
</div>
```

we create one reusable component.

```jsx
function Card(props) {
    return (
        <div className="card">
            <h2>{props.h2}</h2>
            <h3>{props.h3}</h3>
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
                h2="First card's h2"
                h3="First card's h3"
            />

            <Card
                h2="Second card's h2"
                h3="Second card's h3"
            />

            <Card
                h2="Third card's h2"
                h3="Third card's h3"
            />
        </div>
    );
}
```

This is one of the most important ideas in React:

> **Build once, reuse many times.**

---

# 8. Props

**Props** means properties.

Props allow a parent component to pass data to a child component.

Parent:

```jsx
function App() {
    return (
        <Card
            h2="Hello"
            h3="Welcome"
        />
    );
}
```

Child:

```jsx
function Card(props) {
    return (
        <div>
            <h2>{props.h2}</h2>
            <h3>{props.h3}</h3>
        </div>
    );
}
```

The data flow is:

```text
App
 |
 | h2="Hello"
 | h3="Welcome"
 ↓
Card
 |
 ↓
props.h2
props.h3
```

### Props are read-only

A child should not directly modify its props.

Think:

```text
Parent owns data
      ↓
Child receives data
```

---

# 9. Props with Objects

Instead of passing individual values separately, data can be stored in an object.

```jsx
const data = {
    heading: "99% off all items!",
    callToAction: "Everything must go!"
};
```

Then:

```jsx
<PromoHeading
    heading={data.heading}
    callToAction={data.callToAction}
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

# 10. Destructuring Props

Instead of:

```jsx
function Card(props) {
    return <h2>{props.title}</h2>;
}
```

we can destructure:

```jsx
function Card({ title }) {
    return <h2>{title}</h2>;
}
```

For multiple props:

```jsx
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

# 11. The `children` Prop

React automatically provides a special prop called `children`.

Example:

```jsx
function Button({ children }) {
    return (
        <button>
            {children}
        </button>
    );
}
```

Usage:

```jsx
<Button>
    Click me
</Button>
```

Here:

```text
children = "Click me"
```

Another example:

```jsx
function Card({ children }) {
    return (
        <div className="card">
            {children}
        </div>
    );
}
```

Usage:

```jsx
<Card>
    <h2>Hello</h2>
    <p>This is inside the card.</p>
</Card>
```

### Why `children` is useful

It allows us to create wrapper/layout components without knowing exactly what content will be placed inside them.

---

# 12. Props Drilling

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

```text
App
  ↓
Header
  ↓
Wrapper
  ↓
Section
  ↓
Container
  ↓
Button
```

If only `Button` needs the data, passing it through every level can become annoying.

This leads to **Context API**.

---

# 13. State

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

Here:

```text
num       → current state value
setNum    → function used to update state
0         → initial value
```

---

# 14. `useState`

The basic syntax is:

```jsx
const [state, setState] = useState(initialValue);
```

Example:

```jsx
const [num, setNum] = useState(0);
```

Initially:

```text
num = 0
```

After:

```jsx
setNum(1);
```

React updates the state and re-renders the component.

---

# 15. State Example — Counter

```jsx
import { useState } from "react";

function App() {
    const [num, setNum] = useState(0);

    return (
        <div>
            <h1>Current Number: {num}</h1>

            <button onClick={() => setNum(num + 1)}>
                Add 1
            </button>

            <button onClick={() => setNum(num - 1)}>
                Subtract 1
            </button>
        </div>
    );
}

export default App;
```

Flow:

```text
Initial state
num = 0

       ↓ click Add 1

setNum(1)

       ↓

React re-renders

       ↓

num = 1
```

---

# 16. Updating State

Do not directly modify state like this:

```jsx
num = num + 1; // ❌
```

Use the state setter:

```jsx
setNum(num + 1); // ✅
```

For updates based on the previous value, prefer the functional form:

```jsx
setNum(prevNum => prevNum + 1);
```

This is especially useful when multiple updates may happen close together.

---

# 17. State Can Store Different Types

State can store numbers:

```jsx
const [count, setCount] = useState(0);
```

Strings:

```jsx
const [name, setName] = useState("");
```

Booleans:

```jsx
const [isOpen, setIsOpen] = useState(false);
```

Arrays:

```jsx
const [items, setItems] = useState([]);
```

Objects:

```jsx
const [user, setUser] = useState({
    name: "Alex",
    age: 20
});
```

---

# 18. Event Handling

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
```

Do not immediately execute it:

```jsx
onClick={handleClick()} // ❌
```

For a function with arguments:

```jsx
<button onClick={() => handleClick("hello")}>
    Click me
</button>
```

---

# 19. Common React Events

Some commonly used events:

```jsx
onClick
onDoubleClick
onMouseEnter
onMouseLeave
onMouseOver
onMouseDown
onMouseUp
onContextMenu
onDrag
onDrop
```

There are also keyboard and form events:

```jsx
onKeyDown
onKeyUp
onChange
onSubmit
onFocus
onBlur
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

# 20. Conditional Rendering

React can render different UI depending on a condition.

### Ternary operator

```jsx
condition ? valueIfTrue : valueIfFalse
```

Example:

```jsx
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

If:

```text
hour = 14
```

then:

```text
14 >= 6  → true
14 <= 18 → true
```

So React renders:

```jsx
<Daytime />
```

---

# 21. Conditional Rendering with State

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
darkModeOn = false
      ↓
Light Mode is On

click
      ↓
setDarkModeOn(prev => !prev)
      ↓
darkModeOn = true
      ↓
Dark Mode is On
```

---

# 22. Logical AND Rendering

If we only want something to appear when a condition is true:

```jsx
{isLoggedIn && <Dashboard />}
```

Meaning:

```text
isLoggedIn === true
        ↓
render Dashboard
```

If false, nothing is rendered.

---

# 23. Variables in Components

Normal JavaScript variables can be created inside components.

```jsx
function Heading() {
    let title = "This is some heading text";

    return <h1>{title}</h1>;
}
```

But a normal variable is **not state**.

This:

```jsx
let count = 0;
```

does not cause React to re-render when changed.

For UI data that changes and should update the screen, use state:

```jsx
const [count, setCount] = useState(0);
```

---

# 24. State vs Normal Variables

| Normal Variable | State |
|---|---|
| `let count = 0` | `useState(0)` |
| React does not track changes | React tracks changes |
| Changing it doesn't trigger render | Setter triggers render |
| Useful for temporary calculations | Useful for UI data |

Example:

```jsx
let name = "Alex";
```

versus:

```jsx
const [name, setName] = useState("Alex");
```

---

# 25. Passing State Through Props

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

The data flow is:

```text
State in App
    ↓
props
    ↓
Heading
    ↓
UI
```

---

# 26. Lifting State Up

If multiple components need the same changing data, move the state to their closest common parent.

Example:

```text
       App
      /   \
   Input  Display
```

If both `Input` and `Display` need the same value, keep the state in `App`.

```jsx
function App() {
    const [name, setName] = useState("");

    return (
        <>
            <Input name={name} setName={setName} />
            <Display name={name} />
        </>
    );
}
```

This creates a single source of truth.

---

# 27. Passing Functions as Props

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

# 28. Context API

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

# 29. Consuming Context

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

# 30. Context Provider

The Provider makes the value available to descendants.

```jsx
<MealsContext.Provider value={{ meals }}>
    {children}
</MealsContext.Provider>
```

Think of it as:

```text
MealsProvider
      |
      | context value
      ↓
    App
      |
      ↓
 MealsList
      |
      ↓
useMealsListContext()
```

The nested component does not need every parent to manually pass `meals`.

---

# 31. Context vs Props

### Props

Best when data is needed by a direct child or a small part of the tree.

```text
Parent
  ↓
Child
```

### Context

Useful when many components at different levels need the same data.

```text
Provider
 ├── Header
 ├── Main
 │    └── ProductList
 │         └── Product
 └── Footer
```

Context can prevent unnecessary prop drilling.

---

# 32. Lists and Rendering Multiple Components

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

# 33. Keys

When rendering a list:

```jsx
{items.map(item => (
    <Card key={item.id} />
))}
```

Use a stable unique identifier when possible.

Prefer:

```jsx
key={item.id}
```

Avoid using array indexes as keys when the list can be reordered, inserted into, or deleted from.

---

# 34. Component Composition

React components can be combined to form larger components.

Example:

```jsx
function Main() {
    return (
        <Header>
            <Wrapper>
                <Button />
            </Wrapper>
        </Header>
    );
}
```

This is **composition**.

Rather than making components know about every possible child, allow them to receive children.

---

# 35. A Component Tree

A real application might look like:

```text
App
├── Navbar
│   ├── Logo
│   └── Navigation
├── Hero
├── ProductList
│   ├── ProductCard
│   ├── ProductCard
│   └── ProductCard
├── Footer
└── Modal
```

React applications are essentially trees of components.

Understanding the component tree makes props, state, context and rendering much easier.

---

# 36. React Rendering Mental Model

Think in this order:

```text
State / Props change
        ↓
React renders component
        ↓
JSX is evaluated
        ↓
React determines UI changes
        ↓
Browser UI updates
```

You normally describe **what the UI should look like** for the current state rather than manually changing DOM elements.

---

# 37. Declarative vs Imperative Thinking

### Imperative

Tell the browser exactly what to do:

```text
Find button
Change text
Hide element
Change class
```

### Declarative

Describe what the UI should be:

```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
```

React handles the DOM updates.

---

# 38. Functional Components

Modern React primarily uses function components.

```jsx
function App() {
    return <h1>Hello React</h1>;
}
```

Components can use hooks such as:

```jsx
useState
useEffect
useContext
useReducer
useMemo
useCallback
useRef
```

---

# 39. Hooks — Core Idea

Hooks allow function components to use React features.

Example:

```jsx
import { useState } from "react";
```

Then:

```jsx
const [count, setCount] = useState(0);
```

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
```

Bad:

```jsx
if (something) {
    const [count, setCount] = useState(0);
}
```

---

# 40. `useEffect`

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

Basic dependency patterns:

```jsx
useEffect(() => {
    // runs after renders
});
```

```jsx
useEffect(() => {
    // runs on mount
}, []);
```

```jsx
useEffect(() => {
    // runs when count changes
}, [count]);
```

---

# 41. Cleanup Functions

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

The returned function performs cleanup.

This matters for things such as:

- Timers
- Event listeners
- Subscriptions
- Connections

---

# 42. Forms

React forms commonly use state.

```jsx
function Form() {
    const [name, setName] = useState("");

    return (
        <input
            value={name}
            onChange={e => setName(e.target.value)}
        />
    );
}
```

This is called a **controlled input**.

The value is controlled by React state.

---

# 43. Handling Form Submission

```jsx
function Form() {
    const [name, setName] = useState("");

    const handleSubmit = event => {
        event.preventDefault();

        console.log(name);
    };

    return (
        <form onSubmit={handleSubmit}>
            <input
                value={name}
                onChange={e => setName(e.target.value)}
            />

            <button type="submit">
                Submit
            </button>
        </form>
    );
}
```

`event.preventDefault()` prevents the browser's normal form submission behavior.

---

# 44. `useRef`

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

# 45. `useReducer`

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

Mental model:

```text
UI
 ↓
dispatch(action)
 ↓
reducer
 ↓
new state
 ↓
render
```

---

# 46. `useMemo`

`useMemo` can cache the result of an expensive calculation.

```jsx
const result = useMemo(() => {
    return expensiveCalculation(data);
}, [data]);
```

Do not use it everywhere automatically.

Use performance optimizations when there is a real reason.

---

# 47. `useCallback`

`useCallback` can preserve a function reference between renders.

```jsx
const handleClick = useCallback(() => {
    console.log("clicked");
}, []);
```

It is mainly useful when function identity matters, for example when working with memoized child components.

---

# 48. `React.memo`

`React.memo` can prevent a component from re-rendering when its props have not changed.

```jsx
const Card = React.memo(function Card({ title }) {
    return <h2>{title}</h2>;
});
```

Again, this is a performance tool, not something every component needs.

---

# 49. Immutability

React state should be treated as immutable.

For arrays, avoid:

```jsx
items.push(newItem); // ❌
```

Instead:

```jsx
setItems(prev => [
    ...prev,
    newItem
]);
```

For removing an item:

```jsx
setItems(prev =>
    prev.filter(item => item.id !== id)
);
```

For updating an object:

```jsx
setUser(prev => ({
    ...prev,
    name: "Alex"
}));
```

The spread operator creates a new object/array rather than mutating the existing state.

---

# 50. Conditional UI + State + Events

These three concepts frequently work together.

Example:

```jsx
function Toggle() {
    const [isOn, setIsOn] = useState(false);

    const handleClick = () => {
        setIsOn(prev => !prev);
    };

    return (
        <div>
            <h1>
                {isOn ? "ON" : "OFF"}
            </h1>

            <button onClick={handleClick}>
                Toggle
            </button>
        </div>
    );
}
```

The complete chain:

```text
User clicks
    ↓
Event handler runs
    ↓
State setter runs
    ↓
State changes
    ↓
Component renders again
    ↓
Conditional JSX changes
```

This is one of the most important React patterns.

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

Any descendant can access:

```jsx
const { theme, toggleTheme } =
    useContext(AppContext);
```

This can eliminate large amounts of prop drilling.

---

# 52. Custom Hooks

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
```

Use it:

```jsx
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

Custom hooks allow logic to be shared without duplicating it across components.

---

# 53. Data Fetching

A typical client-side data request can be performed with `fetch`.

```jsx
useEffect(() => {
    async function loadData() {
        const response = await fetch("/api/products");
        const data = await response.json();

        setProducts(data);
    }

    loadData();
}, []);
```

A production application should also consider:

```text
loading
error
success
empty state
cancellation/race conditions
caching
```

For larger applications, a dedicated data-fetching library can sometimes simplify server-state management.

---

# 54. Loading and Error States

Example:

```jsx
if (loading) {
    return <p>Loading...</p>;
}

if (error) {
    return <p>Something went wrong.</p>;
}

return <ProductList products={products} />;
```

A good UI should not assume that data always arrives successfully.

---

# 55. Component Responsibilities

A good component should ideally have a clear responsibility.

Bad:

```text
App
 ├── authentication
 ├── API calls
 ├── giant form
 ├── every card
 ├── every modal
 ├── every piece of business logic
 └── 1000 lines of JSX
```

Better:

```text
App
 ├── Navbar
 ├── Dashboard
 │    ├── Stats
 │    ├── UserList
 │    └── UserCard
 └── Footer
```

Keep components understandable and composable.

---

# 56. Suggested React Project Structure

A common structure:

```text
src/
├── components/
│   ├── Button.jsx
│   ├── Card.jsx
│   └── Navbar.jsx
│
├── pages/
│   ├── Home.jsx
│   └── Dashboard.jsx
│
├── hooks/
│   ├── useAuth.js
│   └── useFetch.js
│
├── context/
│   └── AppContext.jsx
│
├── services/
│   └── api.js
│
├── assets/
│   └── images/
│
├── App.jsx
└── main.jsx
```

The exact structure depends on the project.

---

# 57. React Data Flow

The default mental model is:

```text
Parent
  ↓
Props
  ↓
Child
```

For changing data:

```text
State
  ↓
Render
  ↓
User interaction
  ↓
Event handler
  ↓
State update
  ↓
Render again
```

For shared application data:

```text
Context Provider
       ↓
     Context
       ↓
  Components
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
```

Use:

```jsx
onClick={handleClick} // ✅
```

or:

```jsx
onClick={() => handleClick()} // ✅
```

---

### Mistake 2 — Mutating state

```jsx
items.push(item); // ❌
```

Use:

```jsx
setItems(prev => [...prev, item]); // ✅
```

---

### Mistake 3 — Forgetting keys

```jsx
items.map(item => (
    <Card />
))
```

Prefer:

```jsx
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

### Mistake 5 — Using normal variables for UI state

```jsx
let count = 0;
count++;
```

This does not tell React to re-render.

Use:

```jsx
const [count, setCount] = useState(0);
```

---

### Mistake 6 — Calling hooks conditionally

```jsx
if (loggedIn) {
    useState(false); // ❌
}
```

Hooks should be called consistently at the top level.

---

# 60. The Big Picture

The React concepts learned so far connect like this:

```text
                    React
                      │
          ┌───────────┴───────────┐
          │                       │
      Components               JSX
          │                       │
          ├──────────────┐        │
          │              │        │
        Props          State      │
          │              │        │
          │              ↓        │
          │         useState      │
          │              │        │
          └──────┬───────┘        │
                 ↓                │
             Event Handlers       │
                 │                │
                 ↓                │
            State Updates         │
                 │                │
                 ↓                │
             Re-render            │
                 │                │
                 └──────→ JSX ←───┘

For shared data:

Context
   ↓
Provider
   ↓
useContext
   ↓
Components
```

---

# 61. Recommended Learning Order

Follow this order rather than trying to memorize everything at once:

```text
1. JavaScript fundamentals
        ↓
2. JSX
        ↓
3. Components
        ↓
4. Props
        ↓
5. Children
        ↓
6. Events
        ↓
7. State / useState
        ↓
8. Conditional rendering
        ↓
9. Lists + keys
        ↓
10. Forms
        ↓
11. Lifting state
        ↓
12. Props drilling
        ↓
13. Context API
        ↓
14. useEffect
        ↓
15. Data fetching
        ↓
16. useRef
        ↓
17. useReducer
        ↓
18. Custom hooks
        ↓
19. Performance optimization
        ↓
20. Routing
        ↓
21. Authentication
        ↓
22. API architecture
        ↓
23. Testing
        ↓
24. Production deployment
```

---

# 62. Quick Revision Sheet

## JSX

```jsx
<h1>{variable}</h1>
```

JavaScript expressions go inside `{}`.

## Component

```jsx
function App() {
    return <h1>Hello</h1>;
}
```

## Props

```jsx
<Card title="Hello" />
```

```jsx
function Card({ title }) {
    return <h2>{title}</h2>;
}
```

## Children

```jsx
<Card>
    <p>Hello</p>
</Card>
```

```jsx
function Card({ children }) {
    return <div>{children}</div>;
}
```

## State

```jsx
const [count, setCount] = useState(0);
```

## Event

```jsx
<button onClick={handleClick}>
    Click
</button>
```

## Conditional rendering

```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
```

## List

```jsx
items.map(item => (
    <Card key={item.id} />
))
```

## Context

```jsx
const Context = createContext(null);
```

```jsx
<Context.Provider value={data}>
    {children}
</Context.Provider>
```

```jsx
const data = useContext(Context);
```

## Effect

```jsx
useEffect(() => {
    // side effect
}, []);
```

---

# 63. What the Screenshots Covered

The supplied screenshots demonstrate these practical React concepts:

- Basic React components
- JSX
- JavaScript variables inside JSX
- Curly braces / JSX expressions
- Asset imports and image rendering
- Reusable components
- Multiple components
- Props
- Passing strings through props
- Passing object properties as props
- `children`
- Props drilling
- Event handlers
- `onClick`
- Mouse events such as `onMouseOver`
- Conditional rendering
- Ternary operators
- State
- `useState`
- Updating state
- State-driven UI
- Toggle behavior
- Cards
- Rendering lists of UI
- Context API
- Context providers
- `useContext`
- Custom context hooks
- Basic component composition
- React development workflow / browser preview

The screenshots also show the progression from simple components toward state management and Context API.

---

# 64. One Mental Model to Remember

When building a React feature, ask:

```text
1. What component should own this data?
2. Is this data changing?
3. If it changes, should the UI update?
4. If yes → probably state.
5. Does a child need the data?
6. Pass it as props.
7. Do many distant components need it?
8. Consider Context.
9. Does the user interact with it?
10. Add an event handler.
11. Does the UI change after the interaction?
12. Update state.
13. Is repeated UI involved?
14. Use a component + map().
15. Is the UI different depending on a condition?
16. Use conditional rendering.
```

That mental model will take you surprisingly far.

---

# 65. Topics to Add From Future Screenshots

This document is intentionally structured to keep growing.

Future screenshots can be added under the appropriate sections for topics such as:

- `useEffect` in depth
- API calls
- React Router
- Nested routes
- Authentication
- Context patterns
- `useReducer`
- Forms and validation
- Custom hooks
- `useRef`
- Performance
- Memoization
- Error boundaries
- Lazy loading
- Suspense
- Testing
- State-management libraries
- Server state
- React architecture
- Production deployment
- Security considerations
- Advanced component patterns

---

# Final React Mental Model

```text
                COMPONENTS
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
      PROPS       STATE      CHILDREN
        │           │
        │           ↓
        │        EVENTS
        │           │
        │           ↓
        │      STATE UPDATE
        │           │
        └───────→ RE-RENDER
                    │
                    ↓
                   JSX
                    │
                    ↓
                    UI

       For shared state/data:
                    │
                    ↓
                 CONTEXT
                    │
                    ↓
              useContext()
```

**Core principle:**

> **React UI is a function of state and props.**

```text
UI = f(props, state)
```

Learn to follow the data, and React starts feeling much less mysterious.
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

### 6. `React.cloneElement()`

Used to create a modified copy of an existing React element.

From your Radio Group example:

```jsx
React.cloneElement(child, {
  onChange,
  checked: child.props.value === selected
});
```

So the parent can inject/change props on its children.

```text
Original child
     ↓
cloneElement()
     ↓
Child + additional props
```

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

### 8. React Testing Library

The last part introduces **testing React components**.

Important imports:

```jsx
import {
  fireEvent,
  render,
  screen
} from "@testing-library/react";
```

- `render()` → renders the component for testing
    
- `screen` → finds elements
    
- `fireEvent` → simulates user interactions
    

Example:

```jsx
fireEvent.click(submitButton);
```

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

### 10. Testing Different Scenarios

Your feedback-form example tests two situations:

```text
Score < 5
    ↓
Feedback required
    ↓
Submit with feedback
```

and:

```text
Score > 5
    ↓
Feedback not required
    ↓
Submit without feedback
```

The idea is:

**Don't just test whether something works — test different possible user situations.**

---

# 🧠 Final Cheat Sheet

|Concept|Remember|
|---|---|
|`useEffect`|Run side effects|
|Dependency `[]`|Run after initial render|
|Cleanup function|Remove listeners/timers/subscriptions|
|HOC|Add reusable behavior to components|
|`children`|Components/elements passed inside another component|
|`React.Children.map()`|Iterate over children|
|`cloneElement()`|Add/modify props on children|
|React Testing Library|Test React UI|
|`render()`|Render component for testing|
|`screen`|Find elements|
|`fireEvent`|Simulate interaction|
|`jest.fn()`|Mock a function|
|`expect()`|Assert expected behavior|

### The big idea of this last set:

**React isn't just about displaying UI. It's about reusing behavior, managing side effects, composing components, and testing how users interact with them.**