# 1. Dart Basics

## 2. What is the difference between `var`, `final`, and `const`?

**Answer:**

- `var` allows the variable to be reassigned.
- `final` can be assigned only once and can have a runtime value.
- `const` is a compile-time constant.
    
```
var name = "Aster";
name = "John";
final time = DateTime.now();
const pi = 3.14;
```

---

## 3. What is null safety?

**Answer:**

Null safety prevents variables from containing `null` unless explicitly allowed.

```
String name = "Aster";
String? nickname = null;
```

`String` cannot contain null, while `String?` can.

---

## 5. What is `late` in Dart?

**Answer:**

`late` tells Dart that a variable will be initialized later.

```
late String username;

void init() {
  username = "Aster";
}
```

If the variable is accessed before initialization, a runtime error occurs.

---

## 6. What are `Future`, `async`, and `await`?

**Answer:**

They are used for asynchronous programming.

- `Future` represents a value that will be available later.
- `async` marks a function as asynchronous.
- `await` waits for a Future to complete.

```
Future<String> getUser() async {
  final user = await fetchUser();
  return user;
}
```

---

## 7. What is the difference between Future and Stream?

**Answer:**

A `Future` produces one result in the future.

A `Stream` produces multiple values over time.

Examples:

- Future → API request
    
- Stream → WebSocket or realtime database updates
    

---

## 8. What is an isolate?

**Answer:**

An isolate is an independent Dart execution environment with its own memory.

CPU-intensive operations can be moved to another isolate so that the UI isolate remains responsive.

---

## 9. What is an abstract class?

**Answer:**

An abstract class cannot be instantiated directly. It is commonly used to define contracts or shared behavior.

```
abstract class UserRepository {
  Future<User> getUser();
}
```

---

## 11. What is an extension in Dart?

**Answer:**

An extension allows us to add methods or properties to an existing type without modifying the original class.

```
extension StringExtension on String {
  bool get isEmail => contains("@");
}
```

---

## 13. Why does Flutter use Dart?

**Answer:**

Dart provides features that work well with Flutter:

- Strong typing
- Null safety
- Async/await
- JIT compilation during development
- AOT compilation for production
- Good performance

---

## 14. What is a Widget?
Everything in Flutter's UI is built using widgets.

Examples:

```
Text()
Container()
Row()
Column()
Scaffold()
```

---

## 15. What is the difference between StatelessWidget and StatefulWidget?

**Answer:**

`StatelessWidget` does not maintain mutable state.
`StatefulWidget` has a separate `State` object that can change during the widget's lifetime.

---

## 16. What is the widget tree?

**Answer:**

The widget tree is the hierarchical structure of widgets that describes the application's UI.

Example:

```
MaterialApp
└── Scaffold
    ├── AppBar
    └── Body
        └── Column
            ├── Text
            └── Button
```

---

## 17. What are Flutter's three trees?

**Answer:**
### Widget Tree

Describes what the UI should look like.

### Element Tree

Maintains widget instances and their position in the tree.

### Render Tree

Handles layout, painting, and hit testing.

---

## 18. Why are widgets immutable?

**Answer:**

Widgets are immutable descriptions of the UI.

When state changes, Flutter creates new widget configurations and compares them with the existing tree.

Mutable state is maintained separately, such as inside a `State` object or state-management solution.

---

# 3. Widgets and Lifecycle

## 20. What is `initState()`?

**Answer:**

`initState()` is called once when the State object is inserted into the widget tree.

It is commonly used for:

- Initializing controllers
    
- Starting listeners
    
- Initial setup
    

```
@override
void initState() {
  super.initState();

  controller = TextEditingController();
}
```

---

## 21. What is `dispose()`?

**Answer:**

`dispose()` is used to clean up resources when a widget is permanently removed.

For example:

```
@override
void dispose() {
  controller.dispose();
  super.dispose();
}
```

It is important for controllers, streams, animations, and listeners.

---

## 22. What happens when `setState()` is called?

**Answer:**

1. State is changed.
    
2. Flutter marks the widget as needing a rebuild.
    
3. `build()` runs again.
    
4. Flutter compares the new widget configuration with the existing tree.
    
5. Necessary UI updates are performed.
    

---

## 23. Why shouldn't expensive operations be performed inside `build()`?

**Answer:**

`build()` can execute many times.

Putting expensive operations inside it can cause unnecessary computation and performance problems.

Expensive work should be moved to places such as:

- `initState()`
    
- Notifiers
    
- Providers
    
- Repositories
    
- Services
    

---

## 24. What is BuildContext?

**Answer:**

`BuildContext` represents the location of a widget in the widget tree.

It can be used to access:

```
Theme.of(context);
MediaQuery.of(context);
Navigator.of(context);
```

---

## 25. What is `mounted`?

**Answer:**

`mounted` tells us whether a State object is currently associated with a widget in the widget tree.

It is useful after asynchronous operations.

```
if (!mounted) return;

Navigator.of(context).pop();
```

---

# 4. Layout and UI

## 28. What is the difference between Expanded and Flexible?

**Answer:**

Both control how children use available space.

`Expanded` forces the child to fill the available space.

`Flexible` allows the child to take available space without necessarily forcing it to fill everything.

---

## 29. What is `ListView.builder()`?

**Answer:**

`ListView.builder()` lazily creates list items only when needed.

It is useful for large or dynamic lists.

```
ListView.builder(
  itemCount: users.length,
  itemBuilder: (context, index) {
    return Text(users[index].name);
  },
)
```

---

## 30. ListView vs SingleChildScrollView?

**Answer:**

`ListView` is designed for lists and supports lazy item building.

`SingleChildScrollView` renders its child as a whole and is better for smaller content that needs scrolling.

For large lists, I would use `ListView.builder()`.

---

## 31. What are Keys in Flutter?

**Answer:**

Keys help Flutter identify widgets between rebuilds.

They are especially useful when widgets are:

- Reordered
- Inserted
- Removed
- Dynamically generated

---

## 32. What is GlobalKey?

**Answer:**

`GlobalKey` uniquely identifies a widget across the entire widget tree.
It can be used to access a widget's State or preserve state when widgets move.
It should not be overused because it adds complexity and can have performance implications.

---

## 34. What is SafeArea?

**Answer:**

`SafeArea` prevents content from overlapping system UI such as notches, status bars, and navigation areas.

---

## 36. What is Scaffold?

**Answer:**

`Scaffold` provides the basic Material page structure.

It can contain:

- AppBar
- Body
- Drawer
- FloatingActionButton
- BottomNavigationBar

---

# 5. State Management

## 37. What is state?

**Answer:**

State is data that can change over time and affect the UI.

Examples:

- Logged-in user
- Shopping cart
- Selected tab
- Loading state
- API data

---

## 38. Why do we need state management?

**Answer:**

Small applications can use `setState()`.

As applications grow, state may need to be shared between multiple screens and components.

State management helps with:

- Sharing state
    
- Separating business logic
    
- Avoiding prop drilling
    
- Managing asynchronous state
    
- Testing
    

---

## 39. When would you use setState()?

**Answer:**

I would use `setState()` for simple local UI state.

Examples:

- Password visibility
    
- Selected tab
    
- Checkbox state
    
- Simple counter
    

For shared or complex application state, I would use Riverpod or another state-management solution.

---

## 40. What are common Flutter state-management solutions?

**Answer:**

Common solutions include:

- `setState`
- Provider
- Riverpod
- Bloc

The choice depends on the project's complexity, architecture, and team conventions.

---

# 6. Riverpod

## 41. What is Riverpod?

**Answer:**

Riverpod is a state-management and dependency-injection solution for Dart and Flutter.

It allows application state and business logic to exist independently of widgets.

It provides:

- Reactive state
    
- Dependency injection
    
- Testability
    
- Provider composition
    
- Lifecycle management
    
- Compile-time safety
    

---

## 42. What is a Provider in Riverpod?

**Answer:**

A provider exposes a value, dependency, or piece of state that other parts of the application can consume.

```
final apiClientProvider = Provider<ApiClient>((ref) {
  return ApiClient();
});
```

---

## 43. What is `ref`?

**Answer:**

`ref` is used to interact with providers.

Common methods are:

```
ref.watch();
ref.read();
ref.listen();
ref.invalidate();
```

---

## 44. What is `ref.watch()`?

**Answer:**

`ref.watch()` subscribes to a provider.

When the provider changes, the consumer rebuilds.

```
final user = ref.watch(userProvider);
```

Use it when the UI needs to react to changes.

---

## 45. What is `ref.read()`?

**Answer:**

`ref.read()` accesses the current provider value without subscribing to future changes.

It is commonly used when triggering an action.

```
ref.read(counterProvider.notifier).increment();
```

---

## 46. What is `ref.listen()`?

**Answer:**

`ref.listen()` allows us to react to provider changes without rebuilding the widget.

It is useful for side effects such as:

- Showing a Snackbar
    
- Navigation
    
- Dialogs
    
- Logging
    

```
ref.listen(authProvider, (previous, next) {
  // Handle auth changes
});
```

---

## 47. What is Provider used for?

**Answer:**

`Provider` is commonly used for read-only values and dependency injection.

```
final apiClientProvider = Provider<ApiClient>((ref) {
  return ApiClient();
});
```

---

## 48. What is StateProvider?

**Answer:**

`StateProvider` is useful for simple state.

Examples:

- Selected filter
    
- Selected tab
    
- Boolean toggle
    
- Simple integer or string state
    

```
final counterProvider = StateProvider<int>((ref) => 0);
```

For complex business logic, `Notifier` is generally more appropriate.

---

## 49. What is NotifierProvider?

**Answer:**

`NotifierProvider` is used when synchronous state needs business logic.

```
class CounterNotifier extends Notifier<int> {
  @override
  int build() => 0;

  void increment() {
    state++;
  }
}

final counterProvider =
    NotifierProvider<CounterNotifier, int>(
  CounterNotifier.new,
);
```

---

## 50. What is AsyncNotifier?

**Answer:**

`AsyncNotifier` is useful when state depends on asynchronous operations such as API calls or database operations.

```
class UserNotifier extends AsyncNotifier<User> {
  @override
  Future<User> build() async {
    return fetchUser();
  }
}
```

---

## 51. What is AsyncValue?

**Answer:**

`AsyncValue` represents asynchronous state.

It can represent:

- Loading
    
- Data
    
- Error
    

Example:

```
final user = ref.watch(userProvider);

return user.when(
  loading: () => const CircularProgressIndicator(),
  data: (user) => Text(user.name),
  error: (error, stack) => Text("Error"),
);
```

---

## 52. What is autoDispose?

**Answer:**

`autoDispose` allows Riverpod to dispose of provider state when it is no longer being used.

It is useful for temporary screen-specific state.

---

## 53. What is family in Riverpod?

**Answer:**

`family` allows a provider to receive parameters.

```
final userProvider =
    FutureProvider.family<User, String>((ref, userId) {
  return fetchUser(userId);
});
```

Usage:

```
ref.watch(userProvider("123"));
```

It is useful for IDs, search queries, categories, and filters.

---

## 54. What is the difference between watch, read, and listen?

**Answer:**

|Method|Rebuilds|Purpose|
|---|---|---|
|`watch`|Yes|React to state changes|
|`read`|No|Access state or trigger actions|
|`listen`|No|Perform side effects|

---

## 55. How would you handle API loading, success, and error states using Riverpod?

**Answer:**

I would use `AsyncNotifier` with `AsyncValue`.

```
AsyncLoading
     ↓
AsyncData
     OR
AsyncError
```

The UI watches the provider and renders the appropriate state.

---

## 56. How would you refresh API data using Riverpod?

**Answer:**

I can invalidate the provider:

```
ref.invalidate(userProvider);
```

This tells Riverpod that the provider should be recomputed when needed.

---

## 57. How would you share state between multiple screens?

**Answer:**

I would move the shared state into an appropriate Riverpod provider.

Multiple screens can watch the same provider instead of passing state manually through multiple widget levels.

---

## 58. Why shouldn't everything be placed in one Riverpod provider?

**Answer:**

It can create tightly coupled state and cause unrelated parts of the application to rebuild.

I would split state according to responsibility and usage.

---

## 59. Where should business logic live when using Riverpod?

**Answer:**

Business logic should generally live in Notifiers or appropriate domain/service layers rather than inside widgets.

A common architecture is:

```
UI
 ↓
Notifier
 ↓
Repository
 ↓
API / Database
```

---

## 60. How does Riverpod help with dependency injection?

**Answer:**

Dependencies can be exposed through providers.

```
final apiClientProvider = Provider<ApiClient>((ref) {
  return ApiClient();
});
```

Another provider can consume it:

```
final userRepositoryProvider = Provider<UserRepository>((ref) {
  final api = ref.watch(apiClientProvider);

  return UserRepository(api);
});
```

This also makes dependencies easier to replace during testing.

---

## 61. Riverpod vs Bloc. Which would you choose?

**Answer:**

Both are valid.

Bloc strongly emphasizes events and state transitions.

Riverpod provides state management and dependency injection with a more flexible API and generally less boilerplate.

I would choose based on project requirements, team conventions, and existing architecture.

---

## 62. Riverpod vs Provider?

**Answer:**

Riverpod improves dependency management and type safety and does not require accessing providers through `BuildContext`.

It also provides features such as:

- Provider composition
    
- `autoDispose`
    
- `family`
    
- Async providers
    
- Better testing support
    

---

# 7. Networking and APIs

## 63. How do you call a REST API in Flutter?

**Answer:**

I would typically use `http` or Dio.

---

## 64. Why use Dio instead of http?

**Answer:**

Both can make HTTP requests.

Dio provides additional features such as:

- Interceptors
- Request cancellation
- Timeouts
- Form data
- Global configuration
- Request/response handling

---

## 66. How do you handle API errors?

**Answer:**

I separate errors into categories such as:

- Network errors
    
- HTTP errors
    
- Authentication errors
    
- Validation errors
    
- Server errors
    

I convert them into application-level states rather than exposing raw exceptions directly to the UI.

---

## 67. How would you implement pagination?

**Answer:**

A basic flow is:

```
Load first page
      ↓
User reaches near bottom
      ↓
Request next page
      ↓
Append results
      ↓
Repeat
```

I would also track:

- Current page
    
- Loading state
    
- Error state
    
- Whether more data exists
    
- Duplicate request prevention
    

---

## 68. How would you implement search with an API?

**Answer:**

I would debounce the search input.

```
User types
    ↓
Wait ~300ms
    ↓
No new input?
    ↓
Call API
```

This prevents an API request for every keystroke.

---

# 8. Architecture

## 69. How would you structure a large Flutter application?

**Answer:**

I prefer feature-based architecture.

```
lib/
├── core/
├── features/
│   ├── auth/
│   │   ├── data/
│   │   ├── domain/
│   │   └── presentation/
│   │
│   └── orders/
│       ├── data/
│       ├── domain/
│       └── presentation/
│
└── main.dart
```

This keeps features isolated and easier to maintain.

---

## 70. What is Clean Architecture?

**Answer:**

Clean Architecture separates responsibilities into layers.

A common structure is:

```
Presentation
      ↓
Domain
      ↓
Data
```

The goal is to keep business logic independent from UI frameworks and external services.

---

## 71. What is the Repository Pattern?

**Answer:**

A repository provides an abstraction for accessing data.

```
abstract class UserRepository {
  Future<User> getUser();
}
```

The implementation could retrieve data from:

- REST API
    
- Database
    
- Cache
    

The UI does not need to know where the data comes from.

---

## 72. Why separate UI and business logic?

**Answer:**

It improves:

- Maintainability
    
- Testability
    
- Reusability
    
- Readability
    

It also prevents widgets from becoming large and difficult to maintain.

---

## 73. Explain this architecture.

**Answer:**

```
UI
 ↓
State Management
 ↓
Repository
 ↓
Data Source
 ↓
API / Database
```

The UI handles presentation.

State management handles application state and business logic.

The repository abstracts data access.

The data source communicates with APIs or databases.

---

# 9. Navigation

## 74. What is the difference between Navigator.push and pushReplacement?

**Answer:**

`push()` adds a new route to the navigation stack.

`pushReplacement()` replaces the current route.

---

## 75. What is named routing?

**Answer:**

Named routing allows routes to be identified using names.

For larger applications, a routing package such as GoRouter can provide declarative routing and route guards.

---

## 76. How would you protect authenticated routes?

**Answer:**

I would maintain authentication state centrally and use it with the application's routing solution.

```
Auth State
   ↓
Router
   ↓
Authenticated?
   ├── Yes → Home
   └── No  → Login
```

---

# 10. Local Storage

## 77. What is SharedPreferences?

**Answer:**

SharedPreferences is used for simple persistent key-value data.

Examples:

- Theme preference
    
- Language
    
- Simple flags
    

It should not be used for sensitive data.

---

## 78. SharedPreferences vs secure storage?

**Answer:**

SharedPreferences is suitable for simple non-sensitive values.

Secure storage should be used for sensitive information such as authentication tokens.

---

## 79. When would you use SQLite?

**Answer:**

SQLite is useful when an application needs structured local data with querying, relationships, transactions, and persistence.

Examples:

- Orders
    
- Products
    
- Customers
    
- Inventory
    

---

## 80. How would you implement offline support?

**Answer:**

I would use a local database or cache and synchronize with the backend.

```
UI
 ↓
Repository
 ↓
Local DB
 ↓
Sync Queue
 ↓
API
```

The application can continue working with local data when the network is unavailable.

---

# 11. Authentication and Security

## 81. How would you implement authentication?

**Answer:**

A common flow is:

```
Login
 ↓
Backend
 ↓
Access Token
 ↓
Secure Storage
 ↓
Authenticated API Requests
```

The authentication state can be exposed through Riverpod.

---

## 82. Where should authentication tokens be stored?

**Answer:**

Sensitive tokens should be stored using secure platform storage rather than plain SharedPreferences.

---

## 83. What is token refresh?

**Answer:**

When an access token expires, the application can use a refresh token to obtain a new access token without requiring the user to log in again.

With Dio, an interceptor can be used to handle this centrally.

---

## 84. What should never be stored directly in a Flutter app?

**Answer:**

Sensitive backend secrets should never be embedded directly in the application.

Examples:

- Database passwords
    
- Private API keys
    
- Server credentials
    
- Secret signing keys
    

Anything shipped inside an app can potentially be extracted.

---

# 12. Performance

## 85. How do you improve Flutter performance?

**Answer:**

I would:

- Avoid unnecessary rebuilds
    
- Use `const`
    
- Use lazy lists
    
- Optimize images
    
- Avoid expensive work in `build()`
    
- Split large widgets
    
- Profile before optimizing
    

---

## 86. Why can unnecessary rebuilds be a problem?

**Answer:**

Rebuilding isn't inherently bad, but excessive rebuilding can cause unnecessary computation, layout, and painting work.

I would identify the actual bottleneck using profiling tools instead of trying to prevent every rebuild.

---

## 87. What causes UI jank?

**Answer:**

UI jank can happen when expensive work blocks the UI isolate.

Examples:

- Heavy computation
    
- Large image processing
    
- Expensive layout
    
- Excessive rebuilding
    
- Synchronous database operations
    

---

## 88. How would you debug a slow Flutter application?

**Answer:**

I would use Flutter DevTools to inspect:

- CPU usage
    
- Memory
    
- Widget rebuilds
    
- Frame rendering
    
- Network activity
    

I would identify the bottleneck before optimizing.

---

## 89. Why use const widgets?

**Answer:**

`const` allows Flutter to reuse compile-time constant widget instances and reduces unnecessary object creation.

```
const Text("Hello");
```

I use `const` whenever possible.

---

# 13. Testing

## 90. What types of testing are available in Flutter?

**Answer:**

The main types are:

```
Unit Tests
Widget Tests
Integration Tests
```

### Unit Test

Tests individual functions or classes.

### Widget Test

Tests widgets and UI behavior.

### Integration Test

Tests larger application flows.

---

## 91. What should you unit test?

**Answer:**

I would test:

- Business logic
    
- Notifiers
    
- Repositories
    
- Data transformations
    
- Validation
    
- Utility functions
    

---

## 92. How would you test Riverpod?

**Answer:**

I can create a `ProviderContainer` and override dependencies when necessary.

This allows providers and business logic to be tested without rendering the entire UI.

---

## 93. Why is dependency injection useful for testing?

**Answer:**

It allows real dependencies to be replaced with mocks or fakes.

```
Production:
Notifier → RealRepository → API

Test:
Notifier → MockRepository
```

This makes tests isolated and faster.

---

# 14. Practical Interview Scenarios

## 94. Your API takes 5 seconds to respond. What do you do?

**Answer:**

I would:

1. Show a loading state.
    
2. Keep the UI responsive.
    
3. Handle timeout and errors.
    
4. Cache existing data if appropriate.
    
5. Consider pagination or background loading.
    

---

## 95. A ListView with 10,000 items is lagging. What do you do?

**Answer:**

I would:

- Use `ListView.builder()`
    
- Avoid expensive work in `itemBuilder`
    
- Optimize images
    
- Reduce unnecessary rebuilds
    
- Profile the screen
    
- Consider pagination
    

---

## 96. Two screens need the same state. What would you do?

**Answer:**

I would move the shared state into a state-management layer such as Riverpod rather than passing it manually through multiple widget levels.

---

## 97. Your app crashes when navigating back. How would you debug it?

**Answer:**

I would:

1. Check the stack trace.
    
2. Look for use-after-dispose issues.
    
3. Check controllers and listeners.
    
4. Check asynchronous callbacks.
    
5. Check whether `setState()` or provider state is being accessed after disposal.
    

---

## 98. How would you handle a failed network request?

**Answer:**

I would expose an error state through the state-management layer and show an appropriate UI.

```
Loading
   ↓
Success

OR

Loading
   ↓
Error
   ↓
Retry
```

---

## 99. How would you implement infinite scrolling?

**Answer:**

I would monitor the scroll position and request another page when the user approaches the end.

I would ensure that:

- Only one request runs at a time.
    
- New data is appended.
    
- Loading state is shown.
    
- Errors can be retried.
    
- Requests stop when there is no more data.
    

---

## 100. How would you handle offline order creation in a restaurant application?

**Answer:**

I would use an offline-first approach.

```
Create Order
     ↓
Local Database
     ↓
Sync Queue
     ↓
Internet Available
     ↓
Backend
```

The order can be stored locally first and synchronized when connectivity returns.

---

# 15. Advanced Questions

## 101. What is RepaintBoundary?

**Answer:**

`RepaintBoundary` creates a separate painting boundary.

It can prevent changes in one part of the UI from unnecessarily repainting another part.

I would use it when profiling shows that it improves rendering performance.

---

## 102. What is composition vs inheritance in Flutter?

**Answer:**

Flutter generally favors composition.

Instead of creating deep inheritance hierarchies, we build complex UIs by combining smaller reusable widgets.

```
Screen
├── Header
├── ProductList
└── BottomBar
```

---

## 103. FutureBuilder vs Riverpod AsyncNotifier?

**Answer:**

`FutureBuilder` is useful for simple, local asynchronous UI.

`AsyncNotifier` is more suitable when asynchronous state is part of application/business logic and needs to be shared, tested, refreshed, or combined with other providers.

---

## 104. StreamBuilder vs Riverpod StreamProvider?

**Answer:**

`StreamBuilder` manages a stream directly in the widget.

A Riverpod `StreamProvider` moves the stream into the state-management layer and allows multiple parts of the application to consume it.

---

## 105. Why shouldn't API calls be made directly inside `build()`?

**Answer:**

`build()` can run multiple times.

An API call inside `build()` could execute repeatedly and cause:

- Duplicate requests
    
- Performance problems
    
- Unexpected behavior
    

API calls should be handled through appropriate state-management or lifecycle logic.

---

## 106. How would you prevent unnecessary Riverpod rebuilds?

**Answer:**

I would:

- Watch only the required provider.
    
- Split large state objects where appropriate.
    
- Use `select` when only a specific part of state is needed.
    
- Keep unrelated state in separate providers.
    
- Avoid watching state that the widget doesn't actually use.
    

---

## 107. What is dependency injection?

**Answer:**

Dependency injection means providing an object with its dependencies instead of having it create those dependencies internally.

Example:

```
UserNotifier
      ↓
UserRepository
      ↓
ApiClient
```

Riverpod can manage these dependencies.

---

## 108. How would you handle global error handling?

**Answer:**

I would handle errors at appropriate layers.

For example:

```
API Exception
     ↓
Repository
     ↓
Notifier
     ↓
AsyncError
     ↓
UI
```

For unexpected errors, I would also use centralized logging/crash reporting.

---

# 16. Project-Based Questions

## 109. Explain your Flutter project.

**Answer Structure:**

Explain it in this order:

```
1. What problem does the application solve?
2. Main features
3. Architecture
4. State management
5. API/backend
6. Database/local storage
7. Authentication
8. Important technical challenges
9. How you solved them
10. Result
```

---

## 110. Why did you choose Riverpod?

**Answer:**

I chose Riverpod because it provides reactive state management and dependency injection while keeping business logic separate from the UI.

It also makes providers easier to compose and test.

---

## 111. Why didn't you use setState everywhere?

**Answer:**

`setState()` is useful for local UI state, but using it everywhere makes shared application state harder to manage as the application grows.

Riverpod provides a more centralized and testable approach for shared and asynchronous state.

---

## 112. Why did you choose your architecture?

**Answer:**

I wanted clear separation between presentation, business logic, and data access.

This makes the application easier to maintain, test, and extend.

---

## 113. What was the hardest technical problem in your project?

**Answer Structure:**

```
Problem
   ↓
Why it happened
   ↓
What I tried
   ↓
Solution
   ↓
Result
```

The important part is demonstrating how I approached and solved the problem.

---

## 114. How would you scale your Flutter application?

**Answer:**

I would focus on:

- Feature-based architecture
- Separation of responsibilities
- Consistent state management
- Dependency injection
- Repository pattern
- Automated testing
- Error handling 
- Logging
- CI/CD
- Performance monitoring

---

# 17. Rapid-Fire Questions

## 115. What is the entry point of a Flutter application?

**Answer:**

```
void main() {
  runApp(const MyApp());
}
```

---

## 116. What does `runApp()` do?

**Answer:**

It attaches the root widget to the Flutter rendering system and starts building the widget tree.

---

## 117. What is ThemeData?

**Answer:**

`ThemeData` defines the application's visual theme, including colors, typography, buttons, inputs, and AppBar styles.

---

## 118. What is FutureBuilder?

**Answer:**

`FutureBuilder` builds UI based on the state of a Future.

It is useful for simple asynchronous UI operations.

---

## 119. What is StreamBuilder?

**Answer:**

`StreamBuilder` rebuilds the UI when a Stream emits new values.

It is useful for realtime data.

---

## 120. FutureBuilder vs StreamBuilder?

**Answer:**

`FutureBuilder` handles a single asynchronous result.

`StreamBuilder` handles multiple asynchronous results over time.

---

## 121. What is a const constructor?

**Answer:**

A const constructor allows an object to be created as a compile-time constant when all required values are also constant.

```
class MyWidget extends StatelessWidget {
  const MyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return const Text("Hello");
  }
}
```

---

# ⭐ Most Important Questions to Master

Before an actual Flutter interview, make sure you can answer these naturally:

1. What is Flutter?
2. Why Dart?
3. StatelessWidget vs StatefulWidget
4. Widget lifecycle
5. BuildContext
6. Widget, Element, and Render trees
7. What happens when setState is called?
8. Future vs Stream
9. async/await
10. What is Riverpod?
11. `ref.watch()` vs `ref.read()`
12. `ref.listen()`
    
13. Provider vs StateProvider
    
14. Notifier vs AsyncNotifier
    
15. AsyncValue
    
16. autoDispose
    
17. family
    
18. Dependency injection
    
19. Repository pattern
    
20. Clean Architecture
    
21. API architecture
    
22. API error handling
    
23. Authentication
    
24. Secure token storage
    
25. Pagination
    
26. Offline support
    
27. Preventing unnecessary rebuilds
    
28. Flutter performance optimization
    
29. Riverpod testing
    
30. Explain your Flutter project
    

# Interview Follow-Up Pattern

For almost every technical question, expect the interviewer to ask:

> Why?

Then:

> How would you implement it?

Then:

> What happens if it fails?

Example:

**Interviewer:** Why Riverpod?

**You:** Riverpod provides reactive state management and dependency injection while keeping business logic separate from the UI.

**Interviewer:** Why not setState?

**You:** setState is good for local state, but for shared or complex application state, Riverpod gives better separation and testability.

**Interviewer:** How would you use it for an API?

**You:** I would use an AsyncNotifier, call a repository from it, expose AsyncValue to the UI, and handle loading, success, and error states there.