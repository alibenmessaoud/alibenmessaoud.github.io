---
layout: post
title: Top 5 React Hooks
tags: react hooks 
---

Facebook introduced React Hooks at the beginning of 2019 and quickly gained popularity among React developers. Through my experience, I used existing hooks, wrote my custom hooks, and developed some best practices to get hooks and their code right from the start. `useState` and `useEffect` are among the popular hooks and basic hooks to build a feature or custom hooks. In this post, I'll share some valuable hooks to use in your next project.

### UseHttp

- [UseHttp](https://www.npmjs.com/package/use-http) is an awesome package to fetch data from APIs.

- Alternatives: [[react-query](https://github.com/tannerlinsley/react-query)].

```javascript
import useFetch from 'use-http'

function Todos() {
  const [todos, setTodos] = useState([])
  const { get, response, loading, error } = useFetch('https://example.com')

  useEffect(() => { initializeTodos() }, []) 
  
  async function initializeTodos() {
    const initialTodos = await get('/todos')
    if (response.ok) setTodos(initialTodos)
  }

  return (
    <>
      {error && 'Error!'}
      {loading && 'Loading...'}
      {todos.map(todo => (
        <div key={todo.id}>{todo.title}</div>
      )}
    </>
  )
}
```

### UseLocalStorage

- [UseLocalStorage](https://usehooks.com/useLocalStorage/) helps to sync state to local storage so that it persists through a page refresh.

```javascript
import React from "react";
import { useLocalStorage } from '@rehooks/local-storage';

export default function App() {
  // first arg is key to the value in local storage
  const [value, setValue, remove] = useLocalStorage("theme", "");

  return (
    <div>
      <div>Value: {value}</div>
      <button onClick={() => setValue("blue")}>Blue theme</button>
      <button onClick={() => setValue("green")}>Green theme</button>
      <button onClick={() => remove()}>Default theme</button>
    </div>
  );
}
```

### HookRouter

- [HookRouter](https://www.npmjs.com/package/hookrouter) is an alternative to react-router but in the hooks style!

```javascript
import {useRoutes} from 'hookrouter';

const routes = {
    '/': () => <div>Home</div>,
    '/about': () => <div>About</div>,
    '/products': () => <div>Products</div>,
    '/products/:id': ({id}) => <div>{id}</div>
};

const MyApp = () => {
    const routeResult = useRoutes(routes);
    return routeResult || <div>Not found</div>;
}
```

### ReactHookForm

- [react-hook-form](https://www.react-hook-form.com/) is performant, flexible and extensible forms with easy-to-use validation.

```javascript
import { useForm } from 'react-hook-form';

function App() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();
  const onSubmit = (data) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register('name', { required: true })} />
      {errors.name && <p>Name is required.</p>}
      <input type="submit" />
    </form>
  );
}
```

### UsePortal

[react-useportal](https://github.com/alex-cory/react-useportal) helps to create dropdowns, lightboxes/modals/dialogs, global message notifications, or tooltips.

```javascript
import usePortal from 'react-useportal'

const App = () => {
  var { openPortal, closePortal, isOpen, Portal } = usePortal()

  // want to use array destructuring? You can do that too
  var [openPortal, closePortal, isOpen, Portal] = usePortal()

  return (
    <>
      <button onClick={openPortal}>
        Open Portal
      </button>
      {isOpen && (
        <Portal>
          <p>
            This Portal handles its own state.{' '}
            <button onClick={closePortal}>Close me!</button>, hit ESC or
            click outside of me.
          </p>
        </Portal>
      )}
    </>
  )
}
```

Hooks can change the way you write code in React apps. If you look at the [official documentation](https://reactjs.org/docs/hooks-intro.html), you will see many interesting functionalities with React Hooks. Moreover, React community has provided us in the last two years a lot of custom and useful hooks ready to use. Here I listed some of them, but you can find more and alternatives also.