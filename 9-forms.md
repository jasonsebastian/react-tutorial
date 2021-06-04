# Forms
The default HTML form behavior is to browse to a new page when the user submits the form.
If you want this behavior in React, it just works. But in most cases, it’s convenient to
have a JavaScript function that handles the submission of the form and has access to the
data that the user entered into the form. The standard way to achieve this is with a
technique called "controlled components".

## Controlled Components
In HTML, form elements such as `<input>`, `<textarea>`, and `<select>` typically maintain
their own state and update it based on user input. In React, mutable state is typically
kept in the state property of components, and only updated with `setState()`.

We can combine the two by making the React state be the "single source of truth". Then the
React component that renders a form also controls what happens in that form on subsequent
user input. An input form element whose value is controlled by React in this way is called
a "controlled component".

For example, we have this form in plain HTML which accepts a single name:
```js
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

If we want to make the previous example log the name when it is submitted, we can write
the form as a controlled component:
```js
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

With a controlled component, the input's value is always driven by the React state.
While this means you have to type a bit more code, you can now pass the value to other
UI elements too, or reset it from other event handlers.

## The textarea Tag
In HTML, a `<textarea>` element defines its text by its children:
```html
<textarea>
  Hello there, this is some text in a text area
</textarea>
```
In React, a `<textarea>` uses a value attribute instead. This way, a form using a
`<textarea>` can be written very similarly to a form that uses a single-line input:
```js
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Please write an essay about your favorite DOM element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Essay:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

## The select Tag
In HTML, `<select>` creates a drop-down list. For example, this HTML creates a drop-down list of flavors:
```html
<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Coconut</option>
  <option value="mango">Mango</option>
</select>
```
In React:
```js
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

Overall, this makes it so that `<input type="text">`, `<textarea>`, and `<select>` all
work very similarly - they all accept a value attribute that you can use to implement
a controlled component.

## The file input Tag
In HTML, an `<input type="file">` lets the user choose one or more files from their
device storage to be uploaded to a server or manipulated by JavaScript via the File API.

```html
<input type="file" />
```

Because its value is read-only, it is an uncontrolled component in React.

## Handling Multiple Inputs
When you need to handle multiple controlled input elements, you can add a `name`
attribute to each element and let the handler function choose what to do based on the
value of `event.target.name`.

```js
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```
Note how we used the ES6 [computed property name](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names)
syntax to update the state key corresponding to the given input name.
