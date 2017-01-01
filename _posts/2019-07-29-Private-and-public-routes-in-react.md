---
layout: post
title: Private and Public Routes in React
tags: reactjs react patterns
---

In this tutorial, we will learn how to manage different types of routes private and public in an application.

## Routes in React

React Router conditionally render components to display depending on the route being used in the URL (/ for the home page, /about for the about page, etc.):

```jsx
<Switch>
  <Route path="/login">
    <Login />
  </Route>
  <Route path="/profile">
    <Profile />
  </Route>
</Switch>
```

For private and public routes, most developers at first used to write utility functions to manage the access to some features of the app but they do it at the level page or in wrapper component before declaring the component in the router. So we can do better!

## Private Routes

Private routes are used to manage access or no to some parts of the application. For example, if the user is connected, the component will be shown else it will show the login:

```javascript
import React from "react";
import { Route, Redirect } from "react-router-dom";

const PrivateRoute = ({
  targetedComponent: TargetedComponent,
  loginComponent: LoginComponent,
  isLoggedIn: isLoggedIn,
  ...rest
}) => {
  return (
    <Route
      {...rest}
      render={(props) =>
        isLoggedIn() 
            ? <TargetedComponent {...props} /> 
            : <LoginComponent />
      }
    />
  );
};

export default PrivateRoute;
```

### Usage

Using the created `PrivateRoute` component, we need to specify the required attributes in the `Switch` tag:

```jsx
...
<BrowserRouter>
  <Switch>
    <PrivateRoute
      targetedComponent={Home}
      loginComponent={Login}
      isLoggedIn={isLoggedIn}
      path="/profile"
      exact
    />
  </Switch>
</BrowserRouter>
...
```

## Public Route

On the other hand, public route component is the model for all public routes. We can add some behaviors to restrict access based on criteria or a configuration like the time, the date, or privilege. 

```javascript
import React from "react";
import { Route, Redirect } from "react-router-dom";

const PublicRoute = ({ targetComponent: Component, 
                      isRestricted, ...rest }) => {
  return (
    <Route
      {...rest}
      render={(props) =>
        isRestricted() 
            ? <Redirect to="/" /> 
            : <TargetedComponent />
      }
    />
  );
};

export default PublicRoute;
```

### Usage

```jsx
...
<BrowserRouter>
  <Switch>
    <PublicRoute
      targetComponent={About}
      isRestricted={false}
      path="/about"
      exact
    />
  </Switch>
</BrowserRouter>
...
```

## Conclusion

In this tutorial, we saw how to manage different types of routes and how to use them.