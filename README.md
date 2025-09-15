# React Context API

    Context API is a way to share state(data) between multiple components in a react application
    without having to pass props manually at every levels (prop drilling)
    In other words, it can consider as a global store

## Basic flow

    Create a context -> createContext [from React]
    Provide Context -> Wrap components inside a Context.Provider
    Consume Context -> use UseContext() or Context.Consumer

### Where to use context

    When we have data that need to be accessed by many nested components
    Eg: theme(dark/light), authentication (user info, login state), lang, cart

# Performance Optimization techniques

1. Prevent wasted renders
   - memo, useMemo, useCallback, passing elements as children or regular prop
2. Improve app speed and responsiveness
   - useMemo, useCallback, useTransition
3. Reduce bundle size
   - using fewer 3rd party packages
   - code splitting & lazy loading

# Re-render happens in 3 diff situations

1. State changes
2. Context changes
3. Parent re-renders
   - create a false impression that changing props re-renders a component.
     But this is not true

Note: A **render** doesn't mean that DOM actually get updated, it just means
component function gets called. But this can be an expensive operation.
**Wasted Render** means, a render that didn't produce any change in DOM

# MEMO Function

- Used to create a component that will **NOT RE-RENDER when its parents re-renders**, as long as the **PROPS stay the SAME** between renders
- A memoized component still **re-renders when its own state changes** or when
  **a context that its subscribed to the changes**

### Issue with Memo

- In react, everything re-created on every render (including objects & functions)
- In JS, 2 objects or functions that look the same are actually different
  ie {} != {}
- So if an **object/function passed as a props**, child component will always see them as **new prop on each render**
- so if props are different between re-renders memo will not works

# useMemo & useCallbacks - 2 new Hooks

- used to memoize values (useMemo) and functions (useCallback) between renders
- values passed into useMemo & useCallback will be **stored in memory** (ie cached) and **returned in subsequent re-renders** as long as dependencies/props stay the same
- useMemo & useCallback has **a dependency Array**(like useEffect); whenever one dependency changes, the value will be re-created.

- useMemo Hook is calling instantly
  eg: const val = useMemo(() => {
  return { values to be memoized }
  }, [sideEffect variables])
- useCallback Hook is just define function def, not calling instantly
  eg: const memoizedFtn=useCallback(functionToBeMemoized{}, [sideEffect variables])

### use cases

    - memoized props to prevent wasted renders [together with memo]
    - Memoizing values to avoid expensive re-calculations on every render
    - Memoizing values that are used in dependency array of another hook

# useEffect Dependency Array Rules

- Every **state variable, prop & context value** used inside the effect **must** be included in the dependency array
- All **reactive values** must be included. ie **Any function/variable** that reference any other reactive value
- Dependencies choose themselves, **NEVER** ignore the exhaustive-deps ESLint rule
- **DO NOT** use **objects or arrays** as dependencies (objects are re-created on each render and react sees new objects as different ie {} !== {})

# Remove unnecessary dependencies

### Removing function dependencies

    - move function into the effect
    - if we need function in xle places, memoize it(useCallback)
    - if the function doesn't reference any reactive values, move it out of the component

### Avoid Object dependencies

    - instead of including the entire object, include **only the properties** you need ie primitive values
    - if that does't work, use same strategies as for functions ie move/memoize object

### Other strategies

    - If we have xle related reactive values as dependencies try using a reducer ie useReducer
    - no need to include setState(from useState) and dispatch(from useReducer) in the dependencies as react guarantees to be stable across renders

### When not to use an Effect

    - Responding to a user event, an event handler function should be used instead
    - Fetching data on component mount, fine in small apps but in real-world app a library like React Query should be used
    - Synchronizing  state changes with one another, try to use derived state/ event handlers
