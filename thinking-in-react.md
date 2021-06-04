# Thinking in React

In this document, we’ll walk you through the thought process of building a searchable
product data table using React.

## Start With A Mock

Imagine that we already have a JSON API and a mock from our designer. The mock looks
like this:

![](img/thinking-in-react-mock.png)

Our JSON API returns some data that looks like this:
```
[
  {category: "Sporting Goods", price: "$49.99", stocked: true, name: "Football"},
  {category: "Sporting Goods", price: "$9.99", stocked: true, name: "Baseball"},
  {category: "Sporting Goods", price: "$29.99", stocked: false, name: "Basketball"},
  {category: "Electronics", price: "$99.99", stocked: true, name: "iPod Touch"},
  {category: "Electronics", price: "$399.99", stocked: false, name: "iPhone 5"},
  {category: "Electronics", price: "$199.99", stocked: true, name: "Nexus 7"}
];
```

## Step 1: Break The UI Into A Component Hierarchy
The first thing you’ll want to do is to draw boxes around every component (and
subcomponent) in the mock and give them all names. If you’re working with a designer, they
may have already done this, so go talk to them! Their Photoshop layer names may end up
being the names of your React components!

But how do you know what should be its own component? Use the same techniques for
deciding if you should create a new function or object. One such technique is the
[single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle),
that is, a component should ideally only do one thing. If it ends up growing, it should
be decomposed into smaller subcomponents.

Since you’re often displaying a JSON data model to a user, you’ll find that if your
model was built correctly, your UI (and therefore your component structure) will map
nicely. That’s because UI and data models tend to adhere to the same *information
architecture*. Separate your UI into components, where each component matches one piece
of your data model.

![](img/thinking-in-react-components.png)

You’ll see here that we have five components in our app. We’ve italicized the data each component represents.
1. **`FilterableProductTable` (orange)**: contains the entirety of the example
2. **`SearchBar` (blue)**: receives all *user input*
3. **`ProductTable` (green)**: displays and filters the *data collection* based on *user input*
4. **`ProductCategoryRow` (turquoise)**: displays a heading for each *category*
5. **`ProductRow` (red)**: displays a row for each *product*

If you look at `ProductTable`, you’ll see that the table header (containing the "Name"
and "Price" labels) isn’t its own component. This is a matter of preference, and there's
an argument to be made either way. For this example, we left it as part of
`ProductTable` because it is part of rendering the data collection which is
`ProductTable`'s responsibility. However, if this header grows to be complex (e.g., if
we were to add affordances for sorting), it would certainly make sense to make this its
own `ProductTableHeader` component.

Now that we’ve identified the components in our mock, let’s arrange them into a
hierarchy. Components that appear within another component in the mock should appear as
a child in the hierarchy:

- `FilterableProductTable`
  - `SearchBar`
  - `ProductTable`
    - `ProductCategoryRow`
    - `ProductRow`

## Step 2: Build A Static Version in React
See the Pen [Thinking In React: Step 2](https://codepen.io/gaearon/pen/BwWzwm) on
[CodePen](https://codepen.io/).

