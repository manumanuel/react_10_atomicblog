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
