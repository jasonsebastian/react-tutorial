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
