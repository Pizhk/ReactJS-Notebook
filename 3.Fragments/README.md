# React Fragments📌

## What is a React fragment?

Fragments are syntax that allow us to add multiple elements to a React component without wrapping them in an extra DOM node.
Let’s take a look at the following code:

```javascript
const App = () => {
  return (
    <h1>This is heading1 text</h1>
  );
}

export default App
```

This is a simple React component. When we return only one JSX in a component, we can avoid wrapping the JSX in another wrapper element as seen above. However, when we add more than one JSX element like so:

```javascript
const App = () => {
  return (
    <h1>This is heading1 text</h1>
    <p>This is paragraph text</p>
  );
}

export default App
```

We will encounter a SyntaxError. And thus, crashing our application in development.

<br>

In React, when a component returns multiple elements, we must wrap them in a container element like div for the code to work:
```javascript
const App = () => {
  return (
    <div>
      <h1>This is heading1 text</h1>
      <p>This is paragraph text</p>
    </div>
  );
};
export default App;
```
While this is fine, it may however cause unintended issues in our components.

## React fragment vs <b>div</b>

There is no problem with <b>div</b> containers if they serve a purpose like adding styles to the JSX. However, they are not always needed to wrap our <b>JSX</b>. In this case, when we do, they become extra nodes that clutter the DOM tree.


Sometimes when we work with nested components, these wrappers can cause anomaly in the code. For instance, the div can cause the layout to break when working with the CSS Flexbox and Grid. We may also experience invalid HTML for elements that must follow a specific structure like <b>ul > li</b> and <b>table>tr>td.</b>


Having said that, we will take a look at some of these issues in practice and see how the React Fragment solves them. Starting with the CSS layout with Flexbox.

## Using div wrapper in a <b>CSS</b> layout

Consider the following example to create a simple layout of rows and columns using Flexbox:

```javascript
import "./styles.css";

const Row = ({ children }) => <div className="row">{children}</div>;

const Column = ({ children }) => <div className="col">{children}</div>;

const Box = ({ color }) => (
  <div className="box" style={{ backgroundColor: color }}></div>
);

export default function App() {
  return (
    <Row>
      <Column>
        <Box color="#007bff" />
      </Column>
      <Column>
        <Box color="#fc3" />
      </Column>
      <Column>
        <Box color="#ff3333" />
      </Column>
    </Row>
  );
}
```

Each Row renders a div, enclosing content aligned in a single row, and a Column renders enclosing content in a vertical fashion. Inside every Column, there is a Box component that renders a simple div with a fixed-width container and a background color passed as props to it:

```css
/* styles.css */
.row {
  display: flex;
}
.col {
  flex: 1;
}
.box {
  min-width: 100px;
  min-height: 100px;
}
```
The above code renders three columns in a single row, as shown below:

<img width="700" alt="Screen Shot 2023-01-15 at 5 54 08 PM" src="https://user-images.githubusercontent.com/96326525/212539105-7b7be163-9d79-40cf-861e-20f631b69211.png">


Let’s refactor the above code to separate the first two columns into a different component called ChildComponent. Imagine this as a reusable component that you might want to decouple:

```javascript
export default function App() {
  return (
    <Row>
      <ChildComponent />
      <Column>
        <Box color="#ff3333" />
      </Column>
    </Row>
  );
}

const ChildComponent = () => (
  <div>
    <Column>
      <Box color="#007bff" />
    </Column>
    <Column>
      <Box color="#fc3" />
    </Column>
  </div>
);
```

The expected result should be the same as before, but it isn’t. Decoupling the first two columns in a separate component, ChildComponent, breaks the layout:

<img width="700" alt="Screen Shot 2023-01-15 at 5 57 51 PM" src="https://user-images.githubusercontent.com/96326525/212539219-b8cf8345-fc0e-4820-afef-843e1283e884.png">
