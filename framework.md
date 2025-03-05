## WORK IN PROGRESS

## Directory Structure

```
/my-project-root
├── example/
│   ├── index.html
│   ├── src/
│   │   └── index.js
│   │   └── TaskItem.js
│   │   └── TaskList.js
│   └── server.js           
└── framework/
    ├── core/
    │   ├── store.js
    │   └── router.js
    ├── dom/
    │   └── domHelper.js
    ├── http/
    │   └── http.js
    └── components/
        └── Components.js
        └── formComponent.js
```


## How It Works

- **State Management:** The application imports the framework's store to keep track of tasks and the current tab filter. Updates to the state automatically trigger re-rendering of the UI.
- **DOM Manipulation:** The framework's DOM helper simplifies element creation and event binding, reducing boilerplate code.
- **Event Handling:** Tasks are manipulated (added, completed, deleted) by handling click events on corresponding buttons.

The framework is structured as follows:
- `store.js` manages the task list state.
- `router.js` enables tab navigation.
- `eventDelegator.js` handles user interactions.
- `domHelper.js` dynamically updates the UI.
- `http.js` handles API requests.
- `Component.js` handles API requests.
- `formComponent.js` handles API requests.

## Table of Contents
- [Overview](#overview)
- [Setup Instructions](#setup-instructions)
- [How It Works](#how-it-works)
- [Code Breakdown](#code-breakdown)
  - [State Management](#state-management)
  - [Event Handling](#event-handling)
  - [DOM Manipulation](#dom-manipulation)
  - [Routing](#routing)
  - [HTTP Requests](#http-requests)
- [Further Development](#further-development)


## Code Breakdown

### Component.js
Base class for reusable UI components with lifecycle methods for rendering, mounting, updating, and unmounting.

<details>
<summary>Example</summary>

```js
// Reusable component solution:
// build things once and reuse them many times, customizing them as needed for each specific use case

// Component class handles tasks like:
// adding elements to the page, removing elements from the page, updating elements when data changes

class Component {
    constructor(props = {}) {
      this.props = props; // values passed to a component when it's created, allowing the same component to display different data
      this.state = {}; // enables the component to store and update its own data
      this.element = null; // holds a reference to the actual DOM element, value is initially null until a reference to the DOM element is assigned
    }
```
</details>

### formComponent.js
Simplifies form creation and handling by creating ready-to-use forms.

<details>
<summary>Example</summary>

```js
import createElement from './domHelper.js';
import store from './store.js';

function createForm() {
  const input = createElement('input', {
    attributes: { type: 'text', placeholder: 'Enter value' },
    events: { input: (e) => store.update({ userInput: e.target.value }) }
  });
  
  const button = createElement('button', {
    attributes: { type: 'submit' },
    events: { click: (e) => { /* click handler */ } }
  }, 'Submit');

  return createElement('form', { events: { submit: (e) => e.preventDefault() } }, input, button);
}
```
</details>

### eventDelegator.js
Implements event delegation, allowing a single event listener on a parent element to handle events from multiple child elements, prevents default browser behavior and event bubbling.

<details>
<summary>Example</summary>

```js
export function delegateEvent(parent, selector, eventType, handler) {
  parent.addEventListener(eventType, function(event) {
    // Event delegation implementation
  });
}
```
</details>

### router.js
Handles URL-based navigation without page reloads, registers route handlers, and updates the browser history.

<details>
<summary>Example</summary>

```js
class Router {
  constructor() {
    this.routes = {};
    window.addEventListener('popstate', () => { this.handleURLChange(location.pathname); });
  }
  // Methods for routing
}
```
</details>

### store.js
Provides centralized state management with localStorage persistence, subscription system for updates, and methods to get/update state.

<details>
<summary>Example</summary>

```js
class Store {
  constructor(initialState = {}) {
    this.state = this.loadState() || initialState;
    this.subscribers = [];
  }
  // Methods for state management
}
```
</details>

### domHelper.js
Simplifies DOM creation and manipulation, enabling setting attributes, styles, and event listeners in one function call.

<details>
<summary>Example</summary>

```js
export default function createElement(tag, options = {}, ...children) {
  const element = document.createElement(tag);
  // Element configuration code
  return element;
}
```
</details>

### styleManager.js
Tool for manipulating element styles.

<details>
<summary>Example</summary>

```js
export function applyStyles(element, styles) {
  Object.entries(styles).forEach(([property, value]) => {
    element.style[property] = value;
  });
}

export function toggleStyle(element, property, value, condition) {
  element.style[property] = condition ? value : '';
}
```
</details>

### http.js
Provides a straightforward and simplified API for HTTP requests

<details>
<summary>Example</summary>

```js
export function get(url, options = {}) { /* implementation */ }
export function post(url, data, options = {}) { /* implementation */ }
export function put(url, data, options = {}) { /* implementation */ }
export function del(url, options = {}) { /* implementation */ }
```
</details>
