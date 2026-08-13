# Ex03 To-Do List using JavaScript
## Date: 12.08.2026

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM
### HTML
```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>To-Do List</title>
<link rel="stylesheet" href="style.css">
</head>
<body>

  <div class="app">
    <h1>Vasanth's To-Do List</h1>

    <form id="todo-form">
      <input type="text" id="todo-input" placeholder="Add a new task..." autocomplete="off" required>
      <button type="submit">Add</button>
    </form>

    <div class="filters">
      <button class="filter-btn active" data-filter="all">All</button>
      <button class="filter-btn" data-filter="active">Active</button>
      <button class="filter-btn" data-filter="completed">Completed</button>
    </div>

    <ul id="todo-list"></ul>

    <div class="footer">
      <span id="items-left">0 items left</span>
      <button id="clear-completed">Clear Completed</button>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>
```
### CSS
```
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', Arial, sans-serif;
}

body {
  min-height: 100vh;
  background: linear-gradient(135deg, #6366f1, #8b5cf6);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.app {
  background: #fff;
  width: 100%;
  max-width: 420px;
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

#todo-form {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}

#todo-input {
  flex: 1;
  padding: 10px 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 14px;
  outline: none;
}

#todo-input:focus {
  border-color: #6366f1;
}

#todo-form button {
  padding: 10px 16px;
  border: none;
  background: #6366f1;
  color: #fff;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
}

#todo-form button:hover {
  background: #4f46e5;
}

.filters {
  display: flex;
  gap: 8px;
  margin-bottom: 12px;
}

.filter-btn {
  flex: 1;
  padding: 6px;
  border: 1px solid #ddd;
  background: #fff;
  border-radius: 6px;
  cursor: pointer;
  font-size: 13px;
  color: #555;
}

.filter-btn.active {
  background: #6366f1;
  color: #fff;
  border-color: #6366f1;
}

#todo-list {
  list-style: none;
  max-height: 300px;
  overflow-y: auto;
  margin-bottom: 12px;
}

#todo-list li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 6px;
  border-bottom: 1px solid #eee;
}

#todo-list li span {
  flex: 1;
  font-size: 14px;
  color: #333;
  word-break: break-word;
}

#todo-list li.completed span {
  text-decoration: line-through;
  color: #aaa;
}

#todo-list li .delete-btn {
  border: none;
  background: none;
  color: #e11d48;
  cursor: pointer;
  font-size: 16px;
}

.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 13px;
  color: #666;
}

#clear-completed {
  border: none;
  background: none;
  color: #6366f1;
  cursor: pointer;
  font-size: 13px;
}

#clear-completed:hover {
  text-decoration: underline;
}
```
### JavaScript
```
const form = document.getElementById('todo-form');
const input = document.getElementById('todo-input');
const list = document.getElementById('todo-list');
const itemsLeft = document.getElementById('items-left');
const clearCompletedBtn = document.getElementById('clear-completed');
const filterBtns = document.querySelectorAll('.filter-btn');

let todos = JSON.parse(localStorage.getItem('todos')) || [];
let currentFilter = 'all';

function saveTodos() {
  localStorage.setItem('todos', JSON.stringify(todos));
}

function render() {
  list.innerHTML = '';

  const filtered = todos.filter(todo => {
    if (currentFilter === 'active') return !todo.completed;
    if (currentFilter === 'completed') return todo.completed;
    return true;
  });

  filtered.forEach(todo => {
    const li = document.createElement('li');
    li.className = todo.completed ? 'completed' : '';

    const checkbox = document.createElement('input');
    checkbox.type = 'checkbox';
    checkbox.checked = todo.completed;
    checkbox.addEventListener('change', () => {
      todo.completed = checkbox.checked;
      saveTodos();
      render();
    });

    const span = document.createElement('span');
    span.textContent = todo.text;

    const deleteBtn = document.createElement('button');
    deleteBtn.className = 'delete-btn';
    deleteBtn.textContent = '✕';
    deleteBtn.addEventListener('click', () => {
      todos = todos.filter(t => t.id !== todo.id);
      saveTodos();
      render();
    });

    li.appendChild(checkbox);
    li.appendChild(span);
    li.appendChild(deleteBtn);
    list.appendChild(li);
  });

  const activeCount = todos.filter(t => !t.completed).length;
  itemsLeft.textContent = `${activeCount} item${activeCount !== 1 ? 's' : ''} left`;
}

form.addEventListener('submit', (e) => {
  e.preventDefault();
  const text = input.value.trim();
  if (!text) return;

  todos.push({ id: Date.now(), text, completed: false });
  saveTodos();
  input.value = '';
  render();
});

clearCompletedBtn.addEventListener('click', () => {
  todos = todos.filter(t => !t.completed);
  saveTodos();
  render();
});

filterBtns.forEach(btn => {
  btn.addEventListener('click', () => {
    filterBtns.forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    currentFilter = btn.dataset.filter;
    render();
  });
});

render();
```

## OUTPUT

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b6c7fe80-163d-4382-a6e7-c255fa673015" />


## RESULT
The program for creating To-do list using JavaScript is executed successfully.
