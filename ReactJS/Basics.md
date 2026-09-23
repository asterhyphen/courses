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

