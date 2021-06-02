# Introducing JSX
```js
const element = <h1>Hello, world!</h1>;
```
- This funny syntax is neither a string nor HTML. It is called JSX.
- JSX is a syntax extension to JavaScript.
- JSX is used to describe what the UI should look like.
- JSX produces React "elements".

## Why JSX?
- React embraces the fact that rendering logic is inherently coupled with other UI logic:
  - how events are handled,
  - how the state changes over time, and
  - how the data is prepared for display.

## Embedding Expressions in JSX
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

## JSX Is An Expression Too
- You can use JSX inside of `if` statements and `for` loops, assign it to variables, accept it as arguments, and return it from functions:
```js
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}</h1>;
  }
  return <h1>Hello, Stranger</h1>;
}
```

## Specifying Attributes with JSX
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

## Specifying Children with JSX
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

## JSX Prevents Injection Attacks
- By default, React DOM escapes any values embedded in JSX before rendering them. Hence it is safe to embed user input in JSX:
```js
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

## JSX Represents Objects
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
