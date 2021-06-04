# Handling Events
Reference: https://reactjs.org/docs/handling-events.html

- Handling events with React elements is very similar to handling events on DOM elements. There are some syntax differences:
  - React events are named using camelCase, rather than lowercase.
  - With JSX you pass a function as the event handler, rather than a string.
  - You cannot return `false` to prevent default behavior in React. You must call `preventDefault` explicitly.
- When using React, you generally don't need to call `addEventListener` to add listeners to a DOM element after it is created. Instead, just provide a listener when the element is initially rendered.
- When you define a component using an ES6 class, a common pattern is for an event handler to be a method on the class.
  - Binding is necessary to make `this` work in the callback.

## Passing Arguments to Event Handlers
- Inside a loop, it is common to want to pass an extra parameter to an event handler.
