---
lineNumbers: true
layout: cover
highlighter: shiki
---

# React + Zustand

<img class="w-6 inline" src="https://github.githubassets.com/images/icons/emoji/unicode/1f43b.png"> Bear necessities for state management in React

---

# Introduction
<br/>

- A small, fast and scalable bearbones state-management solution using simplified flux principles. 
- Has a comfy api based on hooks, isn't boilerplatey or opinionated.

**Features**
- You CAN use the Redux devtools
- Zustand is not only for React
- It’s 100% un-opinionated
- Zustand offers awesome built-in middleware
	- Persist
	- devtools
	- sessionStorage
	- subscribeWithSelector

---

# RTK: Create a Redux Store​

```js
import { configureStore } from '@reduxjs/toolkit'

export const store = configureStore({
  reducer: {},
})
```

---

# RTK: Provide the Redux Store to React​

```js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import { store } from './app/store'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

---

# RTK: Create a Redux State Slice​

```js
import { createSlice } from '@reduxjs/toolkit'

const initialState = {
  value: 0,
}

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    }
  },
})

export const { increment, decrement } = counterSlice.actions

export default counterSlice.reducer
```

---

# RTK: Add Slice Reducers to the Store​

```js
import { configureStore } from '@reduxjs/toolkit'
import counterReducer from '../features/counter/counterSlice'

export default configureStore({
  reducer: {
    counter: counterReducer,
  },
})
```

---

# RTK: Use Redux State and Actions in React Components​

```js
import React from 'react'
import { useSelector, useDispatch } from 'react-redux'
import { decrement, increment } from './counterSlice'

export function Counter() {
  const count = useSelector((state) => state.counter.value)
  const dispatch = useDispatch()

  return (
    <div>
      <div>
        <button onClick={() => dispatch(increment())}>Increment</button>
        <span>{count}</span>
        <button onClick={() => dispatch(decrement())}>Decrement</button>
      </div>
    </div>
  )
}
```

---

# Zustand: Installation
<br/>

Install with NPM

```shell
npm install zustand
```

Install with Yarn

```shell
yarn add zustand
```


---

# Zustand: Create store

```js
import create from "zustand";

let store = (set, get) => ({
  users: [],
  getUsers: async () => {
    const response = await fetch("https://jsonplaceholder.typicode.com/users");
    set({
      users: await response.json()
    });
  },
  removeUser: (index) => set((state) => state.users.splice(index, 1))
});

export default create(store);

```

---

# Zustand: Use the store in React component

```js
import { shallow } from "zustand/shallow";
import userStore from "./userStore";

export function Users() {
  const { users, removeUser } = userStore((state) => ({
      users: state.users,
      removeUser: state.removeUser
    }), shallow);

	return (
		// render component
	)
}
```
---

# Zustand: Using middleware

```js{2,9-13}
import create from "zustand";
import { persist, devtools, subscribeWithSelector } from "zustand/middleware";

let store = (set, get) => ({
  users: [],
  getUsers: () => {}
});

store = devtools(store);

store = persist(store, { name: "user_data" });

store = subscribeWithSelector(store);

export default create(store);

```

---

# Zustand: Listening to state change
<br/>

Listening to state change in any store outside of component

```js{4-12}
import { userStore } from "./app/store";

export default function App() {
  const subscriber1 = userStore.subscribe(
    (state) => state.users.length,
    (cur, prev) => console.log('subscriber1: ', cur, prev)
  );

  const subscriber2 = userStore.subscribe(
    (state) => state.users[0],
    (cur, prev) => console.log('subscriber2: ', cur, prev)
  );

	return (
		// render component
	)
}
```

---

# Demo App
<br/>

Try in Codesandbox

[https://codesandbox.io/s/react-zustand-vdi14](https://codesandbox.io/s/react-zustand-vdi14)