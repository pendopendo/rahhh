## To‑Do App Example

This is a simple To‑Do List application built using our custom framework. It demonstrates core features such as state management, DOM manipulation, event handling, and HTTP requests.

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) installed on your system

### Installation

1. Navigate to the example folder and install dependencies (if using Express):

   ```bash
   cd example
   npm install
   ```

#### Running the Express Server

1. Start the server by running:

   ```bash
   node server.js
   ```

2. Open your browser and navigate to `http://localhost:3000`.

## Overview
- - **Active and Completed Tabs:**
  - **Active Tab:** Displays only tasks that are not yet marked as completed.
  - **Completed Tab:** Displays tasks that have been completed.
- - **Task Management:**
  - **Add Task:** Enter a new task via an input field.
  - **Complete Task:** Click the **Complete** button on an active task to mark it as completed (this moves the task to the Completed tab).
  - **Delete Task:** Remove a task completely from the application.

## Code Breakdown

### State Management
The application imports the framework's `store` to keep track of tasks and the current tab filter. Updates to the state automatically trigger re-rendering of the UI.

<details>
<summary>Example</summary>

```js
// From index.js

// State initialization
if (!store.getState().tasks) {
  store.update({ tasks: [] });
}
if (!store.getState().filter) {
  store.update({ filter: FILTERS.ACTIVE });
}

// Reactive rendering with store subscriptions
store.subscribe(renderApp);

// State updates in response to user actions
function completeTask(id) {
  put(`http://localhost:3000/tasks/${id}`, { completed: true })
    .then(updatedTask => {
      fetchTasks();
    });
}
```
</details>

### TaskList Component
The `TaskList` demonstrates the component composition.

<details>
<summary>Example</summary>

```js
// From TaskList.js
class TaskList extends Component {
  render() {
    const { tasks, filter, onComplete, onDelete } = this.props;
    
    // Business logic within components
    const filteredTasks = filter === 'active'
      ? tasks.filter(task => !task.completed)
      : tasks.filter(task => task.completed);
      
    // Component composition - creating child components
    filteredTasks.forEach(task => {
      const taskItem = new TaskItem({ task, onComplete, onDelete });
      container.appendChild(taskItem.render());
    });
    
    return container;
  }
}
```
</details>

### TaskItem Component
The `TaskItem` demonstrates component creation and props usage.

<details>
<summary>Example</summary>

```js
// From TaskItem.js
class TaskItem extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    // Destructure the task from props.
    const { task } = this.props;

    // Create the container element and store the task id as a data attribute.
    const container = createElement('div', { 
      attributes: { 
        class: 'task-item',
        'data-task-id': task.id 
      } 
    });

    // Create an element for the task text.
    const taskText = createElement('span', { attributes: { class: 'task-text' } }, task.text);

    // Use toggleStyle to conditionally apply styles when the task is completed.
    // If task.completed is true, then text will be struck through and colored gray.
    toggleStyle(taskText, 'textDecoration', 'line-through', task.completed);
    toggleStyle(taskText, 'color', '#aaa', task.completed);

    container.appendChild(taskText);

    // If the task is active, add a "Complete" button with a data-action attribute.
    if (!task.completed) {
      const completeButton = createElement('button', {
        attributes: { 
          class: 'complete',
          'data-action': 'complete'
        }
      }, 'Complete');
      container.appendChild(completeButton);
    }

    // Add a "Delete" button (always present) with a data-action attribute.
    const deleteButton = createElement('button', {
      attributes: { 
        class: 'delete',
        'data-action': 'delete'
      }
    }, 'Delete');
    container.appendChild(deleteButton);

    return container;
  }
}
```
</details>



## Review Points

### Application State Management
#### It stores and updates application state between sessions.

<details>
<summary>Example from the project</summary>
  
```js
// Initialize state if not already present in localStorage
if (!store.getState().tasks) {
  store.update({ tasks: [] });
}
if (!store.getState().filter) {
  store.update({ filter: FILTERS.ACTIVE });
}

// When tasks are modified, the state is automatically saved to localStorage
function createTask(task) {
  post('http://localhost:3000/tasks', task)
    .then(newTask => {
      const currentTasks = store.getState().tasks || [];
      store.update({ tasks: [...currentTasks, newTask] }); // This saves to localStorage
    });
}
```
</details>

#### Application state can be shared between elements.

<details>
<summary>Example from the project</summary>
  
```js
// 1. The filter buttons use the shared filter state
const activeButton = createElement('button', {
  events: { click: () => router.navigate('/active') }
}, 'Active');

// Button's appearance changes based on the shared filter state
if (store.getState().filter === FILTERS.ACTIVE) {
  activeButton.classList.add('active');
}

// 2. The task list uses the same shared state
const tasksToRender = store.getState().filter === FILTERS.ACTIVE
  ? store.getState().tasks.filter(task => !task.completed)
  : store.getState().tasks.filter(task => task.completed);

// 3. The add task input is only shown based on the shared filter state
if (store.getState().filter === FILTERS.ACTIVE) {
  const addTaskDiv = createElement('div', { attributes: { class: 'add-task' } });
  // ... input and button creation
  appContainer.appendChild(addTaskDiv);
}
```
</details>

#### Application state can be shared between pages.

<details>
<summary>Example from the project</summary>
  
```js
// 1. Routes update the filter state without losing task data
router.registerRoute('/active', () => {
  store.update({ filter: FILTERS.ACTIVE });  // Only changes filter, preserves tasks
});

router.registerRoute('/completed', () => {
  store.update({ filter: FILTERS.COMPLETED });  // Only changes filter, preserves tasks
});

// 2. Navigation between "pages" happens via these buttons
const activeButton = createElement('button', {
  events: { click: () => router.navigate('/active') }  // Changes page URL
}, 'Active');

const completedButton = createElement('button', {
  events: { click: () => router.navigate('/completed') }  // Changes page URL
}, 'Completed');
```
</details>

#### The application state changes based on the URL.

<details>
<summary>Example from the project</summary>
  
```js
// Register routes that update application state when URL changes
router.registerRoute('/active', () => {
  // When URL is '/active', update the filter state
  store.update({ filter: FILTERS.ACTIVE });
});

router.registerRoute('/completed', () => {
  // When URL is '/completed', update the filter state
  store.update({ filter: FILTERS.COMPLETED });
});

// Navigation buttons that update URL
const activeButton = createElement('button', {
  events: { click: () => router.navigate('/active') }
}, 'Active');

const completedButton = createElement('button', {
  events: { click: () => router.navigate('/completed') }
}, 'Completed');
```
</details>

### URL and Navigation
#### It can control the URL.

<details>
<summary>Example from the project</summary>
  
```js
// Buttons that update the URL when clicked
const activeButton = createElement('button', {
  events: { 
    click: () => router.navigate('/active') // Changes URL to '/active'
  }
}, 'Active');

const completedButton = createElement('button', {
  events: { 
    click: () => router.navigate('/completed') // Changes URL to '/completed'
  }
}, 'Completed');
```
</details>

### Element and Component Management
#### Elements can be created & Elements can be nested in other elements.

<details>
<summary>Example from the project</summary>
  
```js
class TaskItem extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    // Destructure properties from props.
    const { task, onComplete, onDelete } = this.props;

    // Create the container element for the task item.
    const container = createElement('div', { attributes: { class: 'task-item' } });

    // Create an element for the task text.
    const taskText = createElement('span', { attributes: { class: 'task-text' } }, task.text);
    container.appendChild(taskText);

    // If the task is active, add a "Complete" button.
    if (!task.completed) {
      const completeButton = createElement('button', {
        events: { click: () => onComplete(task.id) }
      }, 'Complete');
      completeButton.classList.add('complete');
      container.appendChild(completeButton);
    }

    // Add a "Delete" button (always present).
    const deleteButton = createElement('button', {
      events: { click: () => onDelete(task.id) }
    }, 'Delete');
    deleteButton.classList.add('delete');
    container.appendChild(deleteButton);

    return container;
  }
}
```
</details>

#### It has a system for adding and manipulating styles and attributes.

<details>
<summary>Example from the project</summary>
  
```js
// Manipulate attributes
const addTaskDiv = createElement('div', { attributes: { class: 'add-task' } });
// Manipulate styles
applyStyles(addTaskDiv, { backgroundColor: '#f9f9f9', padding: '10px' });
```
</details>

#### It has reusable component architecture.

<details>
<summary>Example from the project</summary>
  
```js
// Import the base Component class from framework
import Component from '../../../framework/src/components/Component.js';

// Create a reusable TaskItem component by extending Component
class TaskItem extends Component {
  constructor(props) {
    super(props);
  }

export default TaskList; // Export for reuse elsewhere
```
</details>

### Event Handling
#### It handles user input, and form submissions.

<details>
<summary>Example from the project</summary>
  
```js
// Create the task input form
if (store.getState().filter === FILTERS.ACTIVE) {
  const addTaskDiv = createElement('div', { attributes: { class: 'add-task' } });
  
  // Input element for user text entry
  const input = createElement('input', {
    attributes: { type: 'text', placeholder: 'New task...' }
  });
  
  // Button that processes the input value
  const addButton = createElement('button', {
    events: { 
      click: () => {
        // Get and validate user input
        const text = input.value.trim();
        if (text) {
          // Process the input value
          createTask({ text, completed: false });
          // Reset the input field
          input.value = '';
        }
      } 
    }
  }, 'Add Task');
  
  addTaskDiv.appendChild(input);
  addTaskDiv.appendChild(addButton);
  appContainer.appendChild(addTaskDiv);
}
```
</details>

#### Event listeners can be registered when elements are rendered.

<details>
<summary>Example from the project</summary>
  
```js
// Event listener registered on the Add Task button during creation
const addButton = createElement('button', {
  events: { 
    click: () => {
      const text = input.value.trim();
      if (text) {
        createTask({ text, completed: false });
        input.value = '';
      }
    } 
  }
}, 'Add Task');

// Event listeners registered on navigation buttons during rendering
const activeButton = createElement('button', {
  events: { click: () => router.navigate('/active') }
}, 'Active');

const completedButton = createElement('button', {
  events: { click: () => router.navigate('/completed') }
}, 'Completed');
```
</details>

#### Event handling can be delegated to parent elements.

<details>
<summary>Example from the project</summary>
  
```js
  // Attach delegated event listeners.
  delegateEvent(taskListDiv, 'button.complete', 'click', function(event) {
    const taskId = parseInt(this.parentElement.getAttribute('data-task-id'));
    completeTask(taskId);
  });
  delegateEvent(taskListDiv, 'button.delete', 'click', function(event) {
    const taskId = parseInt(this.parentElement.getAttribute('data-task-id'));
    deleteTask(taskId);
  });
```
</details>

#### It prevents default browser behavior and event bubbling.

<details>
<summary>Example from the framework</summary>
  
```js
export function delegateEvent(parent, selector, eventType, handler) {
  parent.addEventListener(eventType, function(event) {
    let target = event.target;
    while (target && target !== parent) {
      if (target.matches(selector)) {
        event.preventDefault(); // preventDefault
        event.stopPropagation(); // stopPropagation
        event.delegateTarget = target;
        handler.call(target, event);
        break;
      }
      target = target.parentElement;
    }
  });
}
```
</details>

<details>
<summary>Example from the project</summary>
  
```js
  // When a click occurs on a button, delegateEvent function intercepts that event:
  // * It calls event.preventDefault() to stop any default behavior
  // * It calls event.stopPropagation() to prevent the event from bubbling up
  delegateEvent(taskListDiv, 'button.complete', 'click', function(event) {
    const taskId = parseInt(this.parentElement.getAttribute('data-task-id'));
    completeTask(taskId);
  });
  delegateEvent(taskListDiv, 'button.delete', 'click', function(event) {
    const taskId = parseInt(this.parentElement.getAttribute('data-task-id'));
    deleteTask(taskId);
  });
```
</details>


