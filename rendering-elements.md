# Rendering Elements
- An element describes what you want to see on the screen:
```js
const element = <h1>Hello, world!</h1>;
```
- Unlike browser DOM elements, React elements are plain objects, and cheap to create.

## Rendering an Element into DOM
- Applications built with just React usually have a single root DOM node:
```html
<div id="root"></div>
```
- To render a React element into a root DOM node, pass both to [`ReactDOM.render()`](https://reactjs.org/docs/react-dom.html#render):
```js
const element = <h1>Hello, world!</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

## Updating the Rendered Element
- React elements are immutable.
- With our knowledge so far, the only way to update the UI is to create a new element, and pass it to [`ReactDOM.render()`](https://reactjs.org/docs/react-dom.html#render). Consider this ticking clock example, which calls [`ReactDOM.render()`](https://reactjs.org/docs/react-dom.html#render) every second from a [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) callback:
```js
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```
> **Note:**
> 
> In practice, most React apps only call [`ReactDOM.render()`](https://reactjs.org/docs/react-dom.html#render) once. In the next sections we will learn how such code gets encapsulated into [stateful components](https://reactjs.org/docs/state-and-lifecycle.html).

## React Only Updates What's Necessary
- React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.
