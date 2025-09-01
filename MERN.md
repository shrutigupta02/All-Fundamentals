## React:
React is a **JavaScript library**

What is JSX?
-> javascript XML
-> html + js containing code is saved under JSX, it is an extension to js 
-> react avoids code complexity by not using JS DOM instead it uses Virtual DOM

Do browsers understand JSX
-> To enable browsers to run code written with JSX, a process called transpilation is required. Transpilation involves converting JSX code into plain JavaScript that browsers can understand.

What is virtual DOM?
-> React maintains a lightweight, in-memory representation of the actual DOM, which is called the Virtual DOM. This is essentially a JavaScript object tree that mirrors the structure of the real DOM.
-> manipulating real DOM is much slower

What is an Event and how to create it?
-> an action that can be triggered by the User or the System like mouse clicking, key press etc

What is Component?
-> single, buildable units in react that make up an application. called the **building blocks** of react
-> reusable pieces

What is a State?
-> Object that stores values of **properties belonging to a component** 
-> every time the state changes, react re-renders the component
-> state can be accessed by this.state

useState(): holds a piece of data that can change over time and trigger re-renders of the component when updated.

Higher Order Component - takes another component as a parameter called **props** and returns a component
Pure Component - variation of react.component class that compares props and state

How to implement React Routing?
-> react programs are SPA ie. single page applications which are basically single page HTML files which are initially loaded and "page changes" are handled by re-rendering of components
-> libraries like react-router-dom are used to map specific URLs to react components using the code:
```jsx
<BrowserRouter>
    <Routes>
      <Route exact path='/' element={<ComponentName/>}/>
    </Routes>
</BrowserRouter>
```


## Node.js:
What is Node.js
-> open-source, cross platform, javascript **RUNTIME ENVIRONMENT** for running web apps outside client's browser
-> uses asynchronous event-driven model which is great for data intensive apps and scale rapidly
-> flexible
-> very fast
-> has many packages/modules
-> easy sync between frontend and backend because of same code base of JS

Explain the diff between frontend and backend:
frontend is client side, backend is server side
frontend focuses on appearance and user experience whereas backend focuses on robustness, security, data integrity.

What is NPM:
node packet manager, responsible for providing all the online modules/repositories/packages like express.js

What are modules in node: just like libraries in JS, python that provide a functionality without having to explicitly code for it
include a library in node:
const express = require('express');

What is express.js
-> node.js web app **framework** that provides a wide set of features to build both web and mobile apps

what is cors:
cross origin requests, ensures frontend connection running on different port with backend running on different port

What is mongoose
-> also node.js web app framework that makes connecting a backend server to a db much easier, express and mongoose can be used together

## JS:
Hoisting: Hoisting in JavaScript is a behavior where variable and function declarations are conceptually moved to the top of their containing scope during the compilation phase

In JavaScript, `5 == '5'` would be `true` (because of type coercion), but `5 === '5'` would be `false` because the types are different (number vs. string).

What are arrow functions and how are they different from regular func:
Arrow functions offer a concise syntax (using `=>`) for writing JavaScript functions. Regular functions have dynamic `this` binding, a dedicated `arguments` object, can be used as constructors

Scope: range of accessibility and visibility of variables. three types: global scope, func scope, block scope

