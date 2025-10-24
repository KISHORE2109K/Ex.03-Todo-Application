# Ex03 To-Do List using JavaScript
# Name: Kishore K
# Register Number:212223040101
## Date: 24-10-2025

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
### html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern To-Do List</title>
    <link rel="stylesheet" href="style.css"> 
</head>
<body>

    <h1>✅ Modern To-Do List</h1>
    
    <div class="container">
        <div class="input-container">
            <input type="text" id="task-input" placeholder="What needs to be done today?">
            <button id="add-button">Add Task</button>
        </div>
        
        <ul id="task-list">
            </ul>
    </div>

    <script src="script.js"></script> 
</body>
</html>

```
### css
```css
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

:root {
    --primary-color: #4CAF50;
    --danger-color: #F44336;
    --background-light: #e0f7fa;
    --container-bg: #ffffff;
    --text-dark: #333;
    --text-light: #fff;
    --completed-text: #9e9e9e;
}

body {
    font-family: 'Poppins', sans-serif;
    background-color: var(--background-light);
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 50px 0;
    min-height: 100vh;
}

h1 {
    color: var(--text-dark);
    font-weight: 600;
    margin-bottom: 30px;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.05);
}

.container {
    width: 90%;
    max-width: 500px;
    background: var(--container-bg);
    border-radius: 12px;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
    padding: 30px;
}

.input-container {
    display: flex;
    gap: 10px;
    margin-bottom: 25px;
}

#task-input {
    flex: 1;
    padding: 12px;
    border: 2px solid #ddd;
    border-radius: 8px;
    font-size: 16px;
    transition: border-color 0.3s;
}

#task-input:focus {
    border-color: var(--primary-color);
    outline: none;
    box-shadow: 0 0 5px rgba(76, 175, 80, 0.5);
}

#add-button {
    padding: 12px 20px;
    background: var(--primary-color);
    color: var(--text-light);
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-size: 16px;
    font-weight: 600;
    transition: background 0.3s, transform 0.2s;
}

#add-button:hover {
    background: #43A047;
    transform: translateY(-1px);
}

#task-list {
    list-style-type: none;
    padding: 0;
    margin: 0;
}

.task-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 15px;
    border-bottom: 1px solid #eee;
    background-color: #f9f9f9;
    margin-bottom: 8px;
    border-radius: 6px;
    transition: background-color 0.3s;
}
    
.task-item:hover {
    background-color: #fffde7;
}

.task-item:last-child {
    border-bottom: none;
    margin-bottom: 0;
}

.task-item .task-text {
    flex: 1;
    cursor: pointer;
    font-size: 17px;
    color: var(--text-dark);
    margin-right: 15px;
    line-height: 1.4;
    user-select: none;
}

.task-item .task-text.completed {
    text-decoration: line-through;
    color: var(--completed-text);
    font-style: italic;
}

.task-item .delete-btn {
    background: var(--danger-color);
    color: var(--text-light);
    border: none;
    border-radius: 4px;
    padding: 6px 12px;
    cursor: pointer;
    font-size: 14px;
    transition: background 0.3s;
}

.task-item .delete-btn:hover {
    background: #d32f2f;
}

```
### js
```js
document.addEventListener('DOMContentLoaded', () => {
    const taskInput = document.getElementById('task-input');
    const addButton = document.getElementById('add-button');
    const taskList = document.getElementById('task-list');

    addButton.addEventListener('click', () => addTask());
    taskInput.addEventListener('keypress', (event) => {
        if (event.key === 'Enter') {
            addTask();
        }
    });

    function addTask(taskData = { text: taskInput.value.trim(), completed: false }) {
        const { text, completed } = taskData;
        if (text === '') {
            alert('Please enter a task.');
            return;
        }

        const li = document.createElement('li');
        li.className = 'task-item';

        const textSpan = document.createElement('span');
        textSpan.className = 'task-text';
        textSpan.textContent = text;

        if (completed) {
            textSpan.classList.add('completed');
        }

        textSpan.addEventListener('click', () => {
            textSpan.classList.toggle('completed');
            saveTasks();
        });

        const deleteButton = document.createElement('button');
        deleteButton.className = 'delete-btn';
        deleteButton.textContent = 'Delete';
        deleteButton.addEventListener('click', () => {
            taskList.removeChild(li);
            saveTasks();
        });

        li.appendChild(textSpan);
        li.appendChild(deleteButton);
        taskList.appendChild(li);

        if (!taskData.hasOwnProperty('text') || taskInput.value.trim() === text) {
            taskInput.value = '';
        }
        saveTasks();
    }

    function saveTasks() {
        const tasks = [];
        taskList.querySelectorAll('.task-item').forEach(li => {
            const textSpan = li.querySelector('.task-text');
            tasks.push({
                text: textSpan.textContent,
                completed: textSpan.classList.contains('completed')
            });
        });
        localStorage.setItem('todoTasks', JSON.stringify(tasks));
    }

    function loadTasks() {
        const storedTasks = localStorage.getItem('todoTasks');
        if (storedTasks) {
            const tasks = JSON.parse(storedTasks);
            tasks.forEach(task => {
                addTask(task);
            });
        }
    }
    
    loadTasks();
});

```
## OUTPUT
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/f2157b77-8fb8-4ba3-a416-0cc07f8da0d8" />

## RESULT
The program for creating To-do list using JavaScript is executed successfully.
