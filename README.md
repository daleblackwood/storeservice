# StoreService

*"Get your Store out of your Component tree"*

StoreService is a convenient wrapper for redux that allows for easy creation
and management of state slices, actions and reducers, seperately from 
components, Higher Order Components (HOCs) and providers. By emulating the
behaviour of React Component state management, and interacting with the redux 
store, each StoreService has the power and consistency of Redux, with the 
convenience of a Component.

*This tech is in development at the moment, I wouldn't recommend it yet.*

### Goals
 - To simplify state management and propagation
 - To be an alternative to HOCs and component->property->component propagation
 - To be a first class Redux citizen
 - To be useful in existing Redux-based toolchains
 - To require no specific React binding, but provide it
 - To be compatable with Preact
 - To be compatable with non-JSX component libraries
 - One dependency: redux

### Why?
Sometimes you may want your state and model management in once place: it can be
time consuming and error-prone to wire in properties from Higher Order 
Components (HOCs) to their children. It'd probably be easier just to avoid the
whole thing and have a dedicated place for model and state management.

### An Alternative To...
StoreService is as an alternative to redux-toolkit and react-redux. It isn't a
drop-in replacement but will work nicely alongside those tools.

### How?

#### Declaring a StoreService
Service declaration should be easy: extend the `StoreService` class and wrap it
in a `StoreService.define()`:
```javascript
const service = StoreService.define(
  class TestService extends StoreService {
    constructor() {
      super("testData", { a: 1, b: 2 });
    }

    increaseA() {
      return this.setState({ a: this.state.a + 1 });
    }
  }
);
```

#### Plugging into a single store (recommended)
The recommended method for using StoreServices is to wrap your top level
reducer and append to middleware using the static method
```javascript
const store = createStore(
  StoreService.getStoreReducer(
    existingReducer
  ),
  applyMiddleware([
    ...existingMiddleware
    ...StoreService.getMiddlewares()
  ])
);
```

#### Plugging into an existing store / stores manually
If you want to try it out or use it on an existing project without tampering
with the store, you can add it to your existing reducers and middleware. It's 
probably best to add StoreServices _after_ your existing store definitions.
```javascript
  const store = createStore(
    combineReducers(
      ...existingReducers
      // all other existing reducers
      c: serviceC.reducer,
      d: serviceD.reducer,
    ),
    applyMiddleware([
      ...existingMiddlewarre
      // all other existing middleware
      serviceC.middleware,
      serviceD.middleware,
    ])
  );
```