# Components and Props
Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called "props") and return React elements describing what should appear on the screen.

## Function and Class Components
The simplest way to define a component is to write a JavaScript function:
```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
This function accepts a single "props" object argument with data and returns a React element. We call such components "function components" because they are literally JavaScript functions.

You can also use an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) to define a component:
```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

## Rendering a Component
Previously, we only encountered React elements that represent DOM tags:
```js
const element = <div />;
```
However, elements can also represent user-defined components:
```js
const element = <Welcome name="Sara" />;
```
When React sees an element representing a user-defined component, it passes JSX attributes and children to this component as a single object. We call this object "props". For example, this code render "Hello, Sara" on the page:
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
Recap:
1. We call `ReactDOM.render()` with the `<Welcome name="Sara" />` element.
2. React calls the `Welcome` component with `{name: 'Sara'}` as the props.
3. Our `Welcome` component returns a `<h1>Hello, Sara</h1>` element as the result.
4. React DOM efficiently updates the DOM to match `<h1>Hello, Sara</h1>`.

> **Note:** Always start component names with a capital letter.
> 
> React treats components starting with lowercase letters as DOM tags. For example, `<div />` represents an HTML div tag, but `<Welcome />` represents a component and requires `Welcome` to be in scope.

## Composing Components
Components can refer to other components in their output. This lets us use the same component abstraction for any level of detail.
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
Typically, new React apps have a single `App` component at the very top. However, if you integrate React into an existing app, you might start bottom-up with a small component like `Button` and gradually work your way to the top of the view hierarchy.

## Extracting Components
Don't be afraid to split components into smaller components. Consider this `Comment` component:
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
This component can be tricky to change because of all the nesting, and it is also hard to reuse individual parts of it. Let’s extract a few components from it. First, we will extract `Avatar`:
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
We recommend naming props from the component's own point of view rather than the context in which it is being used. In this case, we have given its prop a more generic name: `user` rather than `author`.

Now Comment becomes:
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
Next, we will extract a `UserInfo` component that renders an `Avatar` next to the user's name:
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
This lets us simplify `Comment` even further:
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
Having a palette of reusable components pays off in larger apps despite of the seemingly grunt work at first. A good rule of thumb is that if a part of your UI is used several times (`Button`, `Panel`, `Avatar`), or is complex enough on its own (`App`, `FeedStory`, `Comment`), it is a good candidate to be extracted to a separate component.

## Props are Read-Only
All React components must act like [pure functions](https://en.wikipedia.org/wiki/Pure_function) with respect to their props. Pure functions do not attempt to change their inputs, and always return the same result for the same inputs.

Of course, application UIs are dynamic and change over time. State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.
