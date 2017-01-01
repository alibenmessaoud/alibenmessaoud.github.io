---
layout: post
title: Get Started With React Hooks
tags: reactjs react patterns hooks
---

In this tutorial, we'll take a look at React Hooks and how you use them?

## What Are React Hooks?

Hooks are functions like any other JS function, but they are special. Hooks help you to "hook into" React inside a component. In the previous versions, the side effects were implemented using lifecycle methods. But today, in the new way of creating components, hooks took a big part of React and became necessary in functional components.

This means you no longer need to create React components from classes. This shift in architecture helped break up components into small reusable functions instead of a class full of methods (Hated `this` keyword and props drilling). 

In simple words, hooks simplify code, and make it easy to read, maintain, test, and re-use.

Hooks came to life in October 2018, at React conference, where they officially made available under version 16.8. Some of the existing React features were kept, but many features were put on the list of deprecation, so the good news is that you can start using them right now without affecting your projects.

Let's create a new React app and get our hands dirty with hooks.

With Node installed, we do:

```bash
npx create-react-app my-app
```

Then, we change the folder and start the app:

```bash
cd my-app
npm start
```

The app runs the `App` component, which is located at `src/App.js`.

Now let's look at some code. 

But before this, let us clarify something important. There are two types of components in React: class and functional components.

To understand the difference, with and without a hook, let's take a look at this component:

```javascript
import React from "react";

export default class UsernameInput 
    extends React.Component {
        
  constructor(props) {
    super(props);
    this.state = {
      username: "John"
    };
    this.updateUsername = this
        .updateUsername
        .bind(this);
  }

  updateUsername(e) {
    this.setState({
      name: e.target.value
    });
  }
        
  // componentDidMount() {
    /* ... */
  // }      

  render() {
    return (
      <form autoComplete="off">
        <input type="text"
           value={this.state.username}
           onChange={this.updateUsername}
        />
       </form>
       <p>Hello {this.state.username}</p>
    );
  }
}
```

It is an ES6 class that extends from `React.Component` and has `state` and lifecycle methods.

In the constructor, we've declared under our `state` an attribute called `username` and a binding `updateUsername` function to its declaration in the class. Then in the render, we return a form written in JSX, and we used `this.state.username` in the input value and `updateUsername` to ensure the update after any user change. The value held in the form is also output in the message of a greeting.

Now, we will write this version of the component in a functional style and hooks. Functional components are functions that accept arguments as the properties of the component and return valid JSX.

Look at the next code, with some missing parts of code:

```javascript
import React from "react";

export default function UsernameInput(props) { 
  function updateUsername(e) {
    ...
  }

  render() {
    return (
      <form autoComplete="off">
        <input type="text"
           value={..}
           onChange={updateUsername}
        />
       </form>
       <p>Hello {...}</p>
    );
  }
}
```

No constructor, no state object, no `this` keyword, and binding. The ... are the missing code to be filled by the use of hooks.

So, the first hook to meet is `useState`. 

The `useState` is a function that take an initial state object and return an array of 2 objects. It allows us to declare only one state variable (of any type) at a time. In return, as we said before, we get an array, where the first element is the state variable and, the second element is a function to update the value of the variable:

```javascript
const messageState = useState('');
const message = messageState[0]; // Contains ''
const setMessage = messageState[1]; // It’s a function
```

Usually, we’ll use array `destructuring` to simplify the code shown above:

```javascript
const [state, setState] = useState(initialState);
```

This way, we can use the state variable in the functional component like any other variable:

```javascript
import React from "react";

export default function UsernameInput(props) {
    
  const [username, setUsername] = useState('john');
    
  function updateUsername(e) {
    setUpdateUsername(e.target.value);
  }

  render() {
    return (
      <form autoComplete="off">
        <input type="text"
           value={username}
           onChange={updateUsername}
        />
       </form>
       <p>Hello {username}</p>
    );
  }
}
```

It's already much more compact and easier to understand than the class version, yet they both do the same thing. 

I hope you're impressed by now! 

But what if we want to declare more than one property in the state? No problem. Just use multiple calls to `useState`.

```javascript
import React from "react";

export default function UsernameInput(props) {
    
  const [username, setUsername] = useState('john');
  const [location, setLocation] = useState("valladolid");
  ...
  
}
```

Hooks are a new concept that can shake the entire JavaScript ecosystem. The `useState` is a Hook that allows you to have state variables in functional components. The list of hooks to talk about is too long, and we will try in the next articles to cover them.