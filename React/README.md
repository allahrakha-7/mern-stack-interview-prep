# React Interview Questions and Answers

---

## ❓ What is React?

**React** is a JavaScript **library** (not a framework) developed by Meta (Facebook) for building user interfaces. It follows a **component-based architecture**, allowing developers to build complex UIs from small, isolated, and reusable pieces of code.

### Core Concepts:

#### 1. Component-Based Architecture
Everything in React is a component—a self-contained module that renders specific output. This allows for code **reusability** (write once, use everywhere) and easier maintenance.

#### 2. Virtual DOM (The Speed Secret)
React keeps a lightweight copy of the actual DOM in memory called the **Virtual DOM**.
* **Diffing:** When data changes, React compares the new Virtual DOM with the previous version.
* **Reconciliation:** It calculates the most efficient way to update the Real DOM and only changes the specific elements that need updating, rather than reloading the whole page.

#### 3. Declarative UI
Instead of manually manipulating the DOM (imperative), you describe *what* the UI should look like based on the current state, and React handles the *how*.

#### 4. Unidirectional Data Flow
Data in React flows in one direction: **downwards** (Parent to Child) via **props**. This makes the application data predictable and easier to debug.

**React** is a JavaScript **library** not a framework. It focuses on the View layer. Frameworks like Angular provide routing and HTTP requests out of the box; React needs external libraries (or meta-frameworks like Next.js) for those features.

---

## 📜 History of React

React was not built overnight. It started as an internal tool at Facebook to solve a specific problem: **updates on the Facebook News Feed were too slow.**

### The Origin Story
* **Creator:** Jordan Walke, a software engineer at Facebook.
* **Inspiration:** He was influenced by **XHP** (an HTML component framework for PHP).
* **The Problem:** Traditional DOM manipulation was slow and difficult to maintain for massive applications like Facebook.
* **The Solution:** Walke created a prototype called **"FaxJS"** (which later became React) that allowed the UI to be updated efficiently using JavaScript.

### 📅 Key Milestones Timeline

| Year | Milestone | Description |
| :--- | :--- | :--- |
| **2011** | **Internal Use** | React was first deployed on Facebook's **News Feed**. |
| **2012** | **Instagram** | Instagram was acquired by Facebook and adopted React, proving it could be decoupled from Facebook's core. |
| **2013** | **Open Source** | React was open-sourced at **JSConf US** in May. (Many developers were initially skeptical of JSX!). |
| **2015** | **React Native** | Facebook released **React Native**, allowing developers to build mobile apps (iOS/Android) using React. |
| **2016** | **React 15** | Major release focusing on stability and the switch to "Semantic Versioning." |
| **2017** | **React 16 (Fiber)** | Complete rewrite of the core engine (React Fiber) to support asynchronous rendering and better error handling. |
| **2019** | **React 16.8 (Hooks)** | **Game Changer:** Introduced **Hooks** (`useState`, `useEffect`), enabling functional components to hold state. |
| **2022** | **React 18** | Introduced **Concurrent Mode**, Automatic Batching, and Transitions for smoother user experiences. |
| **Now** | **React Server Components** | The shift toward server-side rendering (RSC) with frameworks like **Next.js**. |

> **💡 Fun Fact:** When React was first presented in 2013, the audience "booed" and criticized it because mixing HTML (JSX) and JavaScript was considered a bad practice ("Separation of Concerns") at the time. Today, it is the industry standard.

---

## 📝 JSX (JavaScript XML)

JSX is a syntax extension for JavaScript that looks like HTML. It is not valid JavaScript, and browsers cannot understand it directly.

### How it works 🤔
Tools like **Babel** compile JSX down to `React.createElement()` calls.

```jsx
// 1. What you write (JSX)
const element = <h1 className="title">Hello</h1>;

// 2. What the browser actually runs
const element = React.createElement(
  'h1',
  { className: 'title' },
  'Hello'
);
```
#### ⚠️ 3 Mandatory Rules of JSX
<p>If you break these, your app will crash.</p>
<p><b>1. Return a Single Parent:</b> You cannot return multiple elements side-by-side. They must be wrapped in a parent <div> or a Fragment (<>...</>).</p>
<p><b>2. CamelCase Attributes: </b> standard HTML class becomes className, onclick becomes onClick, tabindex becomes tabIndex.</p>
<p><b>3. Close All Tags:</b> Self-closing tags must end with a slash (e.g., <img />, <br />, <input />).</p>

## 🧩 Components
<p>Components are the building blocks of React. They split the UI into independent, reusable pieces. Conceptually, components are like JavaScript functions: they accept arbitrary inputs (called "Props") and return React elements describing what should appear on the screen.</p>

#### 1. Functional Components (The Modern Standard)
A simple JavaScript function that accepts props and returns JSX. Since React 16.8 (Hooks), these are the primary way to write React code.
```jsx
  const Welcome = (props) => {
  return <h1>Hello, {props.name}</h1>;
}
```
#### 2. Class Components (Legacy)
ES6 classes that extend React.Component. You will see these in older codebases. They require a render() method.
```jsx
  class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

#### 🆚 Functional vs. Class Components

| Feature | Functional Components | Class Components |
| :--- | :--- | :--- |
| **Syntax** | Simple JS Functions. | ES6 Class syntax (more boilerplate). |
| **State Management** | Uses **Hooks** (`useState`). | Uses `this.state` and `this.setState`. |
| **Lifecycle** | Uses `useEffect`. | Uses `componentDidMount`, `componentDidUpdate`, etc. |
| **`this` keyword** | Not needed. | **Confusing.** Requires binding `this` for event handlers. |
| **Performance** | Slightly faster and lighter code. | Slightly heavier due to class overhead. |
| **Best Practice** | **Preferred in 2025.** | Legacy only. Avoid for new projects. |

#### 🌳 The Component Tree
React apps are structured as a tree.
<li>Root Component: Usually <App />.</li>
<li>Parent/Child: Components render other components.</li>
  
```text
App
 ├── Navbar
 ├── Feed
 │    ├── Post
 │    └── Post
 └── Footer
```
---
## 🌳 Virtual DOM (VDOM) & How it Works

### ❓ What is the Virtual DOM?
Virtual DOM is an in-memory representation of the real DOM. React compares diffs and updates only changed parts for performance.
The **Virtual DOM (VDOM)** is a programming concept where a **virtual representation of the UI** is kept in memory and synced with the "real" DOM by a library such as ReactDOM.

Think of it as a **blueprint** or a **lightweight copy** of the actual DOM. It allows React to do all the heavy calculations in JavaScript (which is fast) before touching the browser's DOM (which is slow).

### 🐢 The Problem: Why not use the Real DOM?
The Real DOM is not "slow" to update technically—it's just a JavaScript object. However, **painting the screen is slow.**

Every time you change the Real DOM:
1.  The browser must parse the HTML.
2.  It removes the element and its children.
3.  It recalculates the CSS and Layout (Reflow).
4.  It Repaints the pixels on the screen.

If you update a list of 10 items, the browser might do this process 10 times. This is computationally expensive and causes "layout thrashing."

### 🚀 The Solution: How the Virtual DOM Works



React uses a process called **Reconciliation**. Here is the step-by-step lifecycle of an update:

#### 1. The Snapshot (Render)
When a component's state or props change, React creates a **new** Virtual DOM tree representing the updated UI.

#### 2. The Diffing (Comparison)
React compares this **New Virtual DOM** with the **Previous Virtual DOM**. This process is called **Diffing**.
* React identifies exactly *what* changed (e.g., "The text inside this `<span>` changed from 'Hello' to 'Hi'").
* It ignores everything that stayed the same.

#### 3. The Update (Commit)
React calculates the most efficient way to update the Real DOM. It then applies **only those specific changes** to the Real DOM in a single batch.

**Result:** instead of reloading the whole page, React just modifies one text node.

### 🧠 The Diffing Algorithm (The "Secret Sauce")
Comparing two huge trees is normally an O(n³) algorithm (very slow). React uses a **heuristic O(n) algorithm** based on two main assumptions to make it lightning fast:

#### Rule 1: Two elements of different types produce different trees.
If you change a `<div>` to a `<span>`, React doesn't try to reuse the old content. It destroys the old `<div>` tree completely and builds a new `<span>` tree from scratch.

```jsx
// Old
<div> <Counter /> </div>

// New
<span> <Counter /> </span>

// Result: The old Counter is unmounted and destroyed. A new Counter is mounted.
```
<p>"The Virtual DOM is a strategy React uses to optimize performance. Instead of updating the browser's DOM directly for every small change (which causes slow Reflows and Repaints), React updates a lightweight memory copy first. It compares the new copy to the old one (Diffing), calculates the minimum changes needed, and then efficiently updates the Real DOM in a batch (Reconciliation)."</p>

---

## 🔄 Props vs. State

Props are read-only data passed from parent to child; state is mutable data within a component that triggers re-render when updated.

### 🎁 1. Props (Properties)
**Props** are inputs passed to a component. They allow data to flow from a **Parent** component down to a **Child** component.

* **Immutable:** Props are **read-only**. A child component cannot change the props it receives.
* **External:** They come from "outside" the component.
* **Analogy:** Think of props like function arguments. `function add(a, b)` — `a` and `b` are passed in and the function uses them.

**Example:**
```jsx
// Parent Component
function App() {
  return <Welcome name="Sara" />; // Passing "Sara" as a prop
}

// Child Component
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>; // Using the prop
}
```
### 🧠 2. State
<b>State</b> is data that is managed inside the component. It represents the "memory" of the component.
<li><b>Mutable:</b> State can be changed (updated) using the useState hook.</li>
<li><b>Internal:</b> It is declared and managed within the component itself.</li>
<li><b>Re-rendering:</b> When state changes, React automatically re-renders the component to update the UI.</li>

**Example:**
```jsx
import { useState } from 'react';

function Counter() {
  // declaring state
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      {/* updating state */}
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

| Feature                  | Props                                      | State                                          |
|--------------------------|--------------------------------------------|------------------------------------------------|
| **Source**               | Passed from Parent                         | Defined inside the Component                   |
| **Mutability**           | Immutable (Read-only)                      | Mutable (Changeable)                           |
| **Role**                 | Configuration & Data passing               | Managing dynamic data & User interaction       |
| **Can Parent Change it?**| Yes (by passing a new value)               | No (it's private to the child)                 |
| **Component Type**       | Functional & Class Components              | Functional (via Hooks) & Class Components      |

<p>"Props get passed down the tree. State lives inside a component. If you need to share state between components, you usually 'lift the state up' to the nearest common parent and pass it down as props."</p>

---

# Controlled vs Uncontrolled Components in React
Controlled components use React state to manage input values.
Uncontrolled components use refs to access DOM values directly.

In React, form elements (like `<input>`, `<textarea>`, `<select>`) naturally maintain their own internal state. However, React provides two ways to manage this data:

| Aspect                  | Controlled Components                          | Uncontrolled Components                       |
|-------------------------|------------------------------------------------|-----------------------------------------------|
| **State Management**    | React state is the **single source of truth**  | DOM itself is the source of truth             |
| **Value Source**        | Value comes from React state (`value` prop)    | Value comes from DOM (`defaultValue` prop)    |
| **Updates**             | Handled via `onChange` → update state          | Accessed only when needed (via `ref`)         |
| **Recommended by React**| Yes (preferred in most cases)                  | Fallback for simple cases or integration      |

## Controlled Components

The value of the input is fully controlled by React state.

```jsx
import { useState } from 'react';

function ControlledInput() {
  const [value, setValue] = useState('');

  const handleChange = (e) => {
    setValue(e.target.value);
  };

  return (
    <input
      type="text"
      value={value}          // ← Controlled by React state
      onChange={handleChange}
    />
  );
}
```

#### Advantages
<li>Predictable and testable behavior</li>
<li>Easy to validate, format, or conditionally transform input</li>
<li>Enables instant reaction (e.g., character counters, formatting)</li>
<li>Works seamlessly with React state management (Redux, Context, etc.)</li>

#### Common Use Cases
<li>Forms with validation</li>
<li>Dynamic forms (show/hide fields based on input)</li>
<li>Real-time formatting (uppercase, masking, etc.)</li>

## Uncontrolled Components
<b>Example</b>

```jsx
import { useRef } from 'react';

function UncontrolledInput() {
  const inputRef = useRef(null);

  const handleSubmit = () => {
    alert(`Input value: ${inputRef.current.value}`);
  };

  return (
    <>
      <input type="text" defaultValue="Hello" ref={inputRef} />
      <button onClick={handleSubmit}>Submit</button>
    </>
  );
}
```

#### Advantages
<li>Less boilerplate for simple forms</li>
<li>Slightly better performance (no re-render on every keystroke)</li>
<li>Useful when integrating with non-React code or libraries</li>

#### Disadvantages
<li>Harder to implement validation or real-time feedback</li>
<li>Not ideal for complex forms</li>

---

# What are keys and what is the purpose of keys in lists?
