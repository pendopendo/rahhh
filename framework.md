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

The app is structured as follows:
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

### Event Handling
Event delegation ensures efficient event management.

<details>
<summary>Example</summary>

```js
const delegator = new EventDelegator(document.body);
delegator.on('click', '.complete-task', (event) => {
    const taskId = event.target.dataset.id;
    toggleTaskCompletion(taskId);
});
```
</details>

### DOM Helper
The `domHelper` module dynamically updates the UI.

<details>
<summary>Example</summary>

```js
// todo
```
</details>

### HTTP Communication
The `http` module handles API requests.

<details>
<summary>Example</summary>

```js
// todo
```
</details>
