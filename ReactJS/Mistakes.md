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
