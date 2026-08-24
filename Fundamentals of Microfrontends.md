Microfrontends are an architectural style where a large frontend application is divided into **small, independent applications**.

Each microfrontend:
- Owns one feature/module
- Can be developed and deployed independently
- Can use different frameworks (React, Vue, Angular, etc.)

---

# Traditional Monolithic Frontend

```
┌───────────────────────────────┐
│                               │
│ Login                         │
│ Dashboard                     │
│ Profile                       │                     
│                               │
└───────────────────────────────┘

One huge codebase
One deployment
One team
```

Problems:

- Huge bundle size
- Slow development
- Merge conflicts
- Entire app redeployed for one small change
- Difficult scaling

---

# Microfrontend Architecture

```
                    Browser

                       │
                       ▼

               Shell / Container App
                       │
      ┌────────────────┼────────────────┐
      │                │                │
      ▼                ▼                ▼

 Microfrontend A   Microfrontend B   Microfrontend C

    Products           Cart            Payment
 Separate Team      Separate Team    Separate Team
 Separate Build     Separate Build   Separate Build
 Separate Deploy    Separate Deploy  Separate Deploy
```

---

# Benefits

- Independent deployment
- Independent teams
- Easier maintenance
- Faster releases
- Better scalability
- Smaller codebases
- Fault isolation

---

# Challenges

- Shared authentication
- Shared routing
- Shared state
- Design consistency
- Duplicate dependencies
- Communication between microfrontends

---

# Communication Between Microfrontends

Microfrontends often need to exchange data.

Example:

```
Cart updates -> Navbar should update cart count -> Checkout should know updated cart
```

Ways to communicate:

- Props
- Events
- Custom Events
- Shared State
- Context
- LocalStorage

---

# Shared State Architecture

```
Microfrontend A 
      │ updateState()
      ▼
SharedStateService
      ▲
      │ subscribe()
Microfrontend B
```

---

# Shared State Service

```javascript
class SharedStateService {
    constructor() {
        this.state = {};
        this.listeners = [];
    }

    subscribe(listener) {
        this.listeners.push(listener);
    }

    updateState(newState) {
        this.state = {
            ...this.state,
            ...newState
        };
        this.notifyListeners();
    }

    notifyListeners() {
        this.listeners.forEach(listener => {
            listener(this.state);
        });
    }
}

const sharedStateService = new SharedStateService();
export default sharedStateService;
```

---

# Microfrontend A

Updates the state.

```javascript
import sharedStateService from "./SharedStateService.js";
sharedStateService.subscribe((state) => {
    console.log("A received", state);
});

sharedStateService.updateState({
    cartCount: 1
});
```

---

# Microfrontend B

Listens for changes.

```javascript
import sharedStateService from "./SharedStateService.js";
sharedStateService.subscribe((state) => {
    console.log("B received", state);
});
```

Output

```
A received
{
   cartCount:1
}

B received
{
   cartCount:1
}
```

---

# Why Export One Instance?

Notice

```javascript
const sharedStateService = new SharedStateService();
export default sharedStateService;
```

We export **one object**, not

```javascript
export default SharedStateService;
```

Reason:

If every microfrontend creates

```
new SharedStateService()
```

then everyone gets their own copy.

```
A

State = {}

B

State = {}
```

No communication.

Instead

```
One Instance 
Everyone imports
Same State
```

This is called a **Singleton Pattern**.

---

# Build-Time Integration

## Definition

Microfrontends are combined **before deployment**.

```
Developer -> Build -> Combine all Microfrontends -> Deploy
```

---
## Characteristics

- Single deployment
- Faster runtime
- Easy debugging
- Tight coupling

---

## Advantages

- Better performance
- One bundle
- Easy optimization
- Simpler hosting

---

## Disadvantages

- One deployment
- Entire app rebuilt
- Teams less independent

---

# Build-Time Strategies

## 1. Module Federation

Webpack shares modules between applications.

```
Host -> Loads -> Remote App
```

---

## 2. Static Composition

Simply import modules.

```
import Cart from "./cart";
import Product from "./product";
```

Everything is bundled.

---

## 3. Common Build Pipeline

All projects are built together.

Example:

```
Project -> CI/CD -> Build Payment portal -> Build Cart -> Deploy
```

---

# Example

## microfrontendA/build.js

```javascript
function build() {
    console.log("Building A");
}
module.exports = { build };
```

---

## microfrontendB/build.js

```javascript
function build() {
    console.log("Building B");
}
module.exports = { build };
```

---

## buildMicrofrontends.js

```javascript
const A = require("./microfrontendA/build");
const B = require("./microfrontendB/build");

function buildAll() {
    console.log("Starting Build");
    A.build();
    B.build();
    console.log("Finished");
}
buildAll();
```

Output

```
Starting Build
Building A
Building B
Finished
```

---

# Runtime Integration

## Definition

Microfrontends are loaded while the application is already running.

Nothing is combined beforehand.

The browser loads pieces when required.

---

## Flow

```
User Opens Website -> Shell Application -> Loads Dashboard -> Later -> Loads Cart -> Later -> Loads Payment
```

---

# Advantages

- Independent deployment
- Lazy loading
- Faster updates
- Better scalability
- Teams fully independent

---

# Disadvantages

- Slightly slower initial integration
- Version compatibility
- More complex architecture

---

# Runtime Loading Example

## index.html

```html
<body>
<div id="container"></div>
<button id="loadB">
Load Microfrontend B
</button>

<script type="module">
async function loadMicrofrontend(name, containerId){
    const container = document.getElementById(containerId);
    const script = document.createElement("script");
    script.src = `./${name}/bundle.js`;
    script.type = "module";
    container.appendChild(script);
}

loadMicrofrontend("microfrontendA","container");

document
.getElementById("loadB")
.addEventListener("click",()=>{
    loadMicrofrontend("microfrontendB","container");
});
</script>
</body>
```


```
Browser -> Load A 
when user click -> Load B, only B is downloaded then
```

---

# Build-Time vs Runtime

| Feature | Build-Time | Runtime |
|----------|------------|----------|
| Integration | During build | While application runs |
| Deployment | Single | Independent |
| Loading | Entire app | Lazy loading |
| Performance | Faster startup | Smaller initial load |
| Team Independence | Lower | Higher |
| Complexity | Lower | Higher |
| Updates | Rebuild everything | Deploy individual module |

---

# Complete Example Structure

```
project/
│
├── shell/
│      index.html
│
├── shared/
│      SharedStateService.js
│
├── microfrontendA/
│      bundle.js
│      build.js
│
├── microfrontendB/
│      bundle.js
│      build.js
│
└── buildMicrofrontends.js
```

---

# Typical Workflow

```
Developer

Develop Individual Feature
Test Independently
Choose Integration Strategy
Build-Time OR Runtime
Deploy
User Opens Website
Shell Loads Required Microfrontends
Microfrontends Communicate
Shared State Synchronizes Data
```

---
# Docker Integration

Docker can package each microfrontend into its own container, allowing independent builds and deployments.

## Architecture

```text
                    Browser
                       │
                       ▼
                 Shell / Nginx
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
   Products MF      Cart MF       Payment MF
    Container      Container       Container
```

## Dockerfile

```dockerfile
FROM node:22-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM nginx:alpine

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

## Docker Compose

Run multiple microfrontends together:

```yaml
services:
  shell:
    build: ./shell
    ports:
      - "3000:80"

  products:
    build: ./products
    ports:
      - "3001:80"

  cart:
    build: ./cart
    ports:
      - "3002:80"
```

Start:

```bash
docker compose up
```

## Docker + Runtime Integration

Each microfrontend can be deployed independently:

```text
Products → Docker Image → Container → Deploy
Cart     → Docker Image → Container → Deploy
Payment  → Docker Image → Container → Deploy
```

The shell loads the required microfrontend at runtime.

## Docker + Nginx

Nginx can route requests to different microfrontend containers:

```text
/products → Products Container
/cart     → Cart Container
/payment  → Payment Container
```

## Docker + CI/CD

```text
Git Push -> CI/CD -> Docker Build -> Docker Image -> Container Registry -> Deploy
```

## Benefits

- Independent deployments
- Consistent environments
- Isolated dependencies
- Easy scaling
- Works well with CI/CD
## Complete Flow

```text
Developer -> Git -> CI/CD -> Docker Image -> Container Registry -> Docker Container -> Shell / Nginx -> Microfrontends
```

# Summary

- **Microfrontend:** A small, independently developed frontend application that forms part of a larger application.
- **Shell/Container:** The main application responsible for loading and orchestrating microfrontends.
- **Shared State Service:** A singleton object used for communication and state sharing between microfrontends.
- **Build-Time Integration:** Microfrontends are combined into one application during the build process.
- **Runtime Integration:** Microfrontends are loaded dynamically while the application is running.
- **Module Federation:** Allows applications to share modules at runtime without rebuilding everything.
- **Static Composition:** Directly imports and bundles all microfrontends during the build.
- **Common Build Pipeline:** Builds all microfrontends together in a single CI/CD pipeline.
- **Lazy Loading:** Downloads a microfrontend only when it is needed, improving initial load performance.
- **Singleton Pattern:** Ensures all microfrontends use the same shared service instance for consistent communication.
