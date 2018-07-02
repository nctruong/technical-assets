---
layout: page
title: Redux
permalink: /redux/
---
Redux is a predictable state container for JavaScript apps.

## Basic concepts

#### Dispatch

```react
export const login = () => async (dispatch) => {
  const { json } = await loginToSoundCloud(CLIENT_ID);
  const { oauthToken } = json;
  Cookies.set(COOKIE_PATH, oauthToken);

  dispatch(loginSuccess(oauthToken));
  dispatch(fetchSessionData(oauthToken));
};
const loginSuccess = oauthToken => ({
  type: types.LOGIN_SUCCESS,
  oauthToken,
});
```

- `async` is asynchronous actions.
- dispatched actions need to have type: `loginSuccess`
- reducers listen on `dispatched actions` and map with proper types to get exact data.
- no matter wherever `dispatched` actions involved, reducers always `listen` on these functions to push data to `connected` react components where map state to props - amazing, react uses a lot of callback function. Dispatched function is callback function. Don't care wherever these callbacks involved.

## API

#### mapStateToProps

- connect state from the store to corresponding props
- make it possible to access your reducer state objects from within your React components.
- subscribe to the Redux store and any updates will update props automatically
- the `key is the new prop name` to be used in the React app and the `value is the name of the reducer` function.

#### mapDispatchToProps

### Create a store

```js
import { createStore } from 'redux'
// Reducer
function counterReducer (state = { value: 0 }, action) {
  switch (action.type) {
  case 'INCREMENT':
    return { value: state.value + 1 }
  case 'DECREMENT':
    return { value: state.value - 1 }
  default:
    return state
  }
}

let store = createStore(counter)

// Optional - you can pass `initialState` as a second argument
let store = createStore(counter, { value: 0 })

```

### Use a store

```js
let store = createStore(counter)

// Dispatches an action; this changes the state
store.dispatch({ type: 'INCREMENT' })
store.dispatch({ type: 'DECREMENT' })

// Gets the current state
store.getState()

// Listens for changes
store.subscribe(() => { ... })
```

## React Redux

### Provider

```js
import { Provider } from 'react-redux'

React.render(
  <Provider store={store}>
    <App />
  </Provider>, mountNode)

// The <Provider> component makes the store available in your
// React components. You need this so you can use connect().


```

### Mapping State

```js
import { connect } from 'react-redux'

// A functional React component
function App ({ message, onMessageClick }) {
  return (
    <div onClick={() => onMessageClick('hello')}>
      {message}
    </div>
  )
}
// Maps `state` to `props`:
// These will be added as props to the component.
function mapStateToProps (state) {
  return { message: state.message }
}

// Maps `dispatch` to `props`:
function mapDispatchToProps (dispatch) {
  return {
    onMessageClick (message) {
      dispatch({ type: 'click', message })
    }
  }
}

// Connect :
export default connect(mapStateToProps, mapDispatchToProps)(App)
```

Combining reducers

```js
const reducer = combineReducers({
  counter, user, store
})
//Combines multiple reducers into one reducer function.
// See: combineReducers (redux.js.org)
```

References:

- [Combine Reducers](https://redux.js.org/docs/api/combineReducers.html)
- [Redux Official Docs](https://redux.js.org)


