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