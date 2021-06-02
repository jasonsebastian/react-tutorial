# ReactJS Documentation

This document is a bullet-point summary of the documentation for ReactJS.

## Introducing JSX
```js
const element = <h1>Hello, world!</h1>;
```
- This funny syntax is neither a string nor HTML. It is called JSX.
- JSX is a syntax extension to JavaScript.
- JSX is used to describe what the UI should look like.
- JSX produces React "elements".

### Why JSX?
- React embraces the fact that rendering logic is inherently coupled with other UI logic:
  - how events are handled,
  - how the state changes over time, and
  - how the data is prepared for display.

### Embedding Expressions in JSX
- Use a variable called `name` inside JSX by wrapping it in curly braces:
```js
const name = 'Jason';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```
- You can put any valid JavaScript expression inside the curly braces in JSX (e.g., `2 + 2`, `user.firstName`, or `formatName(user)`).
- Split JSX over multiple lines for readability to avoid the pitfalls of [automatic semicolon insertion](https://stackoverflow.com/q/2846283):
```js
const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);
```

### JSX Is An Expression Too
- You can use JSX inside of `if` statements and `for` loops, assign it to variables, accept it as arguments, and return it from functions:
```js
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}</h1>;
  }
  return <h1>Hello, Stranger</h1>;
}
```

### Specifying Attributes with JSX
- You may use quotes to specify string literals as attributes:
```js
const element = <div tabIndex="0"></div>;
```
- You may also use curly braces to embed a JavaScript expression in an attribute:
```js
const element = <img src={user.avatarUrl}></img>;
```
- Don't put quotes around curly braces when embedding a JavaScript expression in an attribute. Use either quotes (for string values) or curly braces (for expressions), but not both in the same attribute.

> **Warning:**
> 
> Since JSX is closer to JavaScript than to HTML, React DOM uses `camelCase` property naming convention instead of HTML attribute names.
> 
> For example, `class` becomes [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) in JSX, and `tabindex` becomes [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex).

### Specifying Children with JSX
- If a tag is empty, you may close it immediately with `/>`:
```js
const element = <img src={user.avatarUrl} />;
```
- JSX tags may contain children:
```
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

### JSX Prevents Injection Attacks
- By default, React DOM escapes any values embedded in JSX before rendering them. Hence it is safe to embed user input in JSX:
```js
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

### JSX Represents Objects
- Babel compiles JSX down to `React.createElement()` calls. These two examples are identical:
```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```
```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```
- `React.createElement()` performs a few checks to help you write bug-free code but essentially it creates an object like this:
```js
// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```
- These objects are called "React elements". React reads these objects and uses them to construct the DOM and keep it up to date.

## Rendering Elements
- An element describes what you want to see on the screen:
```js
const element = <h1>Hello, world!</h1>;
```
- Unlike browser DOM elements, React elements are plain objects, and cheap to create.

### Rendering an Element into DOM
- Applications built with just React usually have a single root DOM node:
```html
<div id="root"></div>
```
- To render a React element into a root DOM node, pass both to [`ReactDOM.render()`](https://reactjs.org/docs/react-dom.html#render):
```js
const element = <h1>Hello, world!</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

### Updating the Rendered Element
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

### React Only Updates What's Necessary
- React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

## Components and Props
- Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called "props") and return React elements describing what should appear on the screen.

### Function and Class Components
- The simplest way to define a component is to write a JavaScript function:
```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
- This function accepts a single "props" object argument with data and returns a React element. We call such components "function components" because they are literally JavaScript functions.
- You can also use an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) to define a component:
```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### Rendering a Component
- Previously, we only encountered React elements that represent DOM tags:
```js
const element = <div />;
```
- However, elements can also represent user-defined components:
```js
const element = <Welcome name="Sara" />;
```
- When React sees an element representing a user-defined component, it passes JSX attributes and children to this component as a single object. We call this object "props". For example, this code render "Hello, Sara" on the page:
```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
- Recap:
  1. We call `ReactDOM.render()` with the `<Welcome name="Sara" />` element.
  2. React calls the `Welcome` component with `{name: 'Sara'}` as the props.
  3. Our `Welcome` component returns a `<h1>Hello, Sara</h1>` element as the result.
  4. React DOM efficiently updates the DOM to match `<h1>Hello, Sara</h1>`.

> **Note:** Always start component names with a capital letter.
> 
> React treats components starting with lowercase letters as DOM tags. For example, `<div />` represents an HTML div tag, but `<Welcome />` represents a component and requires `Welcome` to be in scope.

### Composing Components
- Components can refer to other components in their output. This lets us use the same component abstraction for any level of detail.
```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
- Typically, new React apps have a single `App` component at the very top. However, if you integrate React into an existing app, you might start bottom-up with a small component like `Button` and gradually work your way to the top of the view hierarchy.

### Extracting Components
- Don't be afraid to split components into smaller components. Consider this `Comment` component:
```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
- This component can be tricky to change because of all the nesting, and it is also hard to reuse individual parts of it. Let’s extract a few components from it. First, we will extract `Avatar`:
```js
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```
- We recommend naming props from the component's own point of view rather than the context in which it is being used. In this case, we have given its prop a more generic name: `user` rather than `author`.
- Now Comment becomes:
```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
- Next, we will extract a `UserInfo` component that renders an `Avatar` next to the user's name:
```js
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```
- This lets us simplify Comment even further:
```js
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
- Having a paletter of reusable components pays off in larger apps despite of the seemingly grunt work at first. A good rule of thumb is that if a part of your UI is used several times (`Button`, `Panel`, `Avatar`), or is complex enough on its own (`App`, `FeedStory`, `Comment`), it is a good candidate to be extracted to a separate component.

### Props are Read-Only
- All React components must act like [pure functions](https://en.wikipedia.org/wiki/Pure_function) with respect to their props. Pure functions do not attempt to change their inputs, and always return the same result for the same inputs.
- Of course, application UIs are dynamic and change over time. State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.
