## WORK IN PROGRESS

## Features

- **Active and Completed Tabs:**
  - **Active Tab:** Displays only tasks that are not yet marked as completed.
  - **Completed Tab:** Displays tasks that have been completed.
- **Task Management:**
  - **Add Task:** Enter a new task via an input field.
  - **Complete Task:** Click the **Complete** button on an active task to mark it as completed (this moves the task to the Completed tab).
  - **Delete Task:** Remove a task completely from the application.
- **State Management:** Uses the framework's global store to maintain tasks and the current filter.
- **DOM Helper:** Leverages the framework’s DOM helper to create and manipulate elements with minimal boilerplate.

## Directory Structure

```
/my-project-root
├── example/
│   ├── index.html          # Main HTML file that loads the app
│   ├── src/
│   │   └── index.js        # Main JavaScript file for the example project
│   └── server.js           # Express server to serve the example project (optional)
└── framework/              # Custom framework code (used by the example)
    ├── core/
    │   ├── store.js        # Application state management
    │   └── router.js       # URL routing (not used in this example)
    ├── dom/
    │   └── domHelper.js    # DOM element creation and manipulation helper
    ├── http/
    │   └── http.js         # HTTP utilities (not used in this example)
    └── components/         # Reusable components (if needed)
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
