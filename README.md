<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>To-Do List</title>
<style>
  body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-image: url('sea_background.jpg'); /* Specify the path to your background image */
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    background-color: rgba(0, 128, 128, 0.5); /* Semi-transparent sea blue color */
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .container {
    background-color: rgba(255, 255, 255, 0.9);
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    text-align: center; /* Center the button */
    transition: background-color 0.3s ease;
  }
  h2 {
    text-align: center;
    margin-bottom: 20px;
    color: #333;
    transition: color 0.3s ease;
  }
  ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
  }
  li {
    margin-bottom: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 8px 12px;
    border-radius: 5px;
    background-color: #f9f9f9;
    animation: fadeIn 0.5s ease forwards;
    transition: background-color 0.3s ease;
  }
  li.completed {
    background-color: #e0e0e0;
  }
  input[type="text"],
  input[type="datetime-local"],
  input[type="submit"],
  select {
    width: calc(50% - 5px);
    padding: 10px;
    margin-bottom: 10px;
    box-sizing: border-box;
    border: none;
    border-radius: 5px;
    transition: all 0.3s ease;
  }
  input[type="text"]:focus,
  input[type="datetime-local"]:focus,
  select:focus {
    outline: none;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
  }
  input[type="submit"] {
    width: 100%;
    background-color: #4CAF50;
    color: white;
    cursor: pointer;
  }
  button {
    background-color: #f44336;
    color: white;
    padding: 5px 10px;
    border: none;
    cursor: pointer;
    margin-left: 5px;
    border-radius: 5px;
    transition: all 0.3s ease;
  }
  button.clear-all {
    background-color: #2196F3;
  }
  button.add-more {
    background-color: #FFD700;
  }

  /* Animation */
  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(-20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  @keyframes fadeOut {
    from {
      opacity: 1;
    }
    to {
      opacity: 0;
    }
  }
</style>
</head>
<body>

<div class="container">
  <h2>To-Do List</h2>
  <form id="todo-form">
    <input type="text" id="todo-input" placeholder="Enter your task...">
    <input type="datetime-local" id="due-date" placeholder="Select deadline...">
    <select id="priority">
      <option value="low">Low Priority</option>
      <option value="medium">Medium Priority</option>
      <option value="high">High Priority</option>
    </select>
    <input type="text" id="labels" placeholder="Enter labels (comma separated)...">
    <input type="submit" value="Add Task">
  </form>
  <ul id="todo-list"></ul>
  <button class="clear-all" onclick="clearAll()">Clear All</button>
  <button class="add-more" onclick="addMore()">Add More Tasks</button>
</div>

<script>
  document.getElementById('todo-form').addEventListener('submit', addTask);

  function addTask(event) {
    event.preventDefault();
    var todoInput = document.getElementById('todo-input');
    var dueDateInput = document.getElementById('due-date');
    var priorityInput = document.getElementById('priority');
    var labelsInput = document.getElementById('labels');
    var todoList = document.getElementById('todo-list');
    if (todoInput.value.trim() !== '') {
      var listItem = document.createElement('li');
      var checkbox = document.createElement('input');
      checkbox.type = 'checkbox';
      checkbox.addEventListener('change', toggleTask);
      var taskText = document.createElement('span');
      taskText.textContent = todoInput.value.trim();
      var dueDate = document.createElement('span');
      dueDate.textContent = formatDueDate(new Date(dueDateInput.value));
      var priority = document.createElement('span');
      priority.textContent = priorityInput.value;
      var labels = document.createElement('span');
      labels.textContent = labelsInput.value;
      var deleteButton = document.createElement('button');
      deleteButton.textContent = 'Delete';
      deleteButton.addEventListener('click', deleteTask);

      listItem.appendChild(checkbox);
      listItem.appendChild(taskText);
      listItem.appendChild(dueDate);
      listItem.appendChild(priority);
      listItem.appendChild(labels);
      listItem.appendChild(deleteButton);
      todoList.appendChild(listItem);

      todoInput.value = '';
      dueDateInput.value = '';
      priorityInput.value = 'low';
      labelsInput.value = '';
    } else {
      alert('Please enter a task!');
    }
  }

  function toggleTask(event) {
    var listItem = event.target.parentElement;
    if (event.target.checked) {
      listItem.classList.add('completed');
    } else {
      listItem.classList.remove('completed');
    }
  }

  function deleteTask(event) {
    var listItem = event.target.parentElement;
    listItem.classList.add('deleted');
    setTimeout(function() {
      listItem.remove();
    }, 500); // Wait for fadeOut animation duration
  }

  function clearAll() {
    var todoList = document.getElementById('todo-list');
    todoList.innerHTML = '';
  }

  function addMore() {
    var todoForm = document.getElementById('todo-form');
    var newTaskInput = document.createElement('input');
    newTaskInput.type = 'text';
    newTaskInput.placeholder = 'Enter your task...';
    newTaskInput.style.width = 'calc(50% - 5px)';
    newTaskInput.style.padding = '10px';
    newTaskInput.style.marginBottom = '10px';
    newTaskInput.style.boxSizing = 'border-box';
    newTaskInput.style.border = 'none';
    newTaskInput.style.borderRadius = '5px';
    todoForm.insertBefore(newTaskInput, todoForm.childNodes[0]);
  }

  function formatDueDate(date) {
    var options = { year: 'numeric', month: 'short', day: 'numeric', hour: 'numeric', minute: 'numeric' };
    return date.toLocaleDateString('en-US', options);
  }
</script>

</body>
</html>
