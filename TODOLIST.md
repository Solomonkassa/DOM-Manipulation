# 📝 Complete To-Do List Application - DOM Manipulation Mastery

This is a **fully functional To-Do List application** that demonstrates **EVERY advanced DOM manipulation concept** covered in the previous guide. Each feature is specifically designed to showcase a different DOM technique.

## 🎯 Features Demonstrated

| Feature | DOM Concept Used |
|---------|------------------|
| Adding tasks | Document Fragment (performance), Dynamic creation |
| Deleting tasks | Event Delegation |
| Editing tasks | Data Attributes, Dynamic cloning |
| Task completion | Class manipulation, CSS transitions |
| Filtering tasks | DOM traversal, Array methods |
| Drag to reorder | Mouse events, DOM insertion |
| Local storage | JSON parsing/stringifying |
| Bulk actions | DOM collections iteration |
| Task counter | Live DOM updates |
| Undo/Redo | DOM state management |

## 🚀 The Complete Application

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🚀 Advanced DOM Todo List</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .todo-container {
            background: white;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            width: 100%;
            max-width: 600px;
            padding: 30px;
        }

        h1 {
            color: #333;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        h1 span {
            background: #667eea;
            color: white;
            padding: 5px 10px;
            border-radius: 5px;
            font-size: 0.6em;
        }

        .input-section {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }

        #taskInput {
            flex: 1;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 5px;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        #taskInput:focus {
            outline: none;
            border-color: #667eea;
        }

        button {
            padding: 12px 20px;
            background: #667eea;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background 0.3s, transform 0.1s;
        }

        button:hover {
            background: #5a67d8;
        }

        button:active {
            transform: scale(0.98);
        }

        .filters {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .filter-btn {
            background: #e0e0e0;
            color: #333;
            flex: 1;
            min-width: 80px;
        }

        .filter-btn.active {
            background: #667eea;
            color: white;
        }

        .stats {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            padding: 15px;
            background: #f7f7f7;
            border-radius: 5px;
            font-weight: bold;
        }

        #todoList {
            list-style: none;
            margin-bottom: 20px;
            min-height: 200px;
            border: 2px dashed #e0e0e0;
            border-radius: 5px;
            padding: 10px;
        }

        .todo-item {
            display: flex;
            align-items: center;
            padding: 12px;
            background: #f9f9f9;
            border: 1px solid #e0e0e0;
            border-radius: 5px;
            margin-bottom: 8px;
            cursor: move;
            transition: all 0.3s;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .todo-item.dragging {
            opacity: 0.5;
            transform: scale(0.98);
            background: #e0e0e0;
        }

        .todo-item.completed {
            background: #e8f5e9;
            opacity: 0.8;
        }

        .todo-item.completed .task-text {
            text-decoration: line-through;
            color: #4caf50;
        }

        .todo-checkbox {
            margin-right: 12px;
            width: 20px;
            height: 20px;
            cursor: pointer;
        }

        .task-text {
            flex: 1;
            font-size: 16px;
            transition: color 0.3s;
        }

        .task-actions {
            display: flex;
            gap: 5px;
        }

        .task-actions button {
            padding: 5px 10px;
            font-size: 14px;
            background: #ff4757;
        }

        .task-actions button.edit-btn {
            background: #ffa502;
        }

        .task-actions button:hover {
            filter: brightness(90%);
        }

        .bulk-actions {
            display: flex;
            gap: 10px;
            margin-top: 20px;
            padding-top: 20px;
            border-top: 2px solid #e0e0e0;
        }

        .bulk-actions button {
            flex: 1;
            background: #ff4757;
        }

        .bulk-actions button.clear-all {
            background: #ff4757;
        }

        .bulk-actions button.complete-all {
            background: #4caf50;
        }

        .undo-redo {
            display: flex;
            gap: 10px;
            margin-top: 10px;
        }

        .undo-redo button {
            flex: 1;
            background: #747d8c;
        }

        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background: #4caf50;
            color: white;
            padding: 15px 25px;
            border-radius: 5px;
            animation: slideInRight 0.3s ease;
            z-index: 1000;
        }

        @keyframes slideInRight {
            from {
                transform: translateX(100%);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }

        .notification.error {
            background: #ff4757;
        }
    </style>
</head>
<body>
    <div class="todo-container">
        <h1>
            📝 Advanced Todo List
            <span id="version">v2.0</span>
        </h1>

        <!-- Input Section -->
        <div class="input-section">
            <input type="text" id="taskInput" placeholder="Enter a new task..." autocomplete="off">
            <button id="addTaskBtn">➕ Add Task</button>
        </div>

        <!-- Filters -->
        <div class="filters">
            <button class="filter-btn active" data-filter="all">All</button>
            <button class="filter-btn" data-filter="active">Active</button>
            <button class="filter-btn" data-filter="completed">Completed</button>
        </div>

        <!-- Stats -->
        <div class="stats" id="stats">
            <span>Total: <span id="totalCount">0</span></span>
            <span>Completed: <span id="completedCount">0</span></span>
            <span>Pending: <span id="pendingCount">0</span></span>
        </div>

        <!-- Todo List Container -->
        <ul id="todoList"></ul>

        <!-- Bulk Actions -->
        <div class="bulk-actions">
            <button id="completeAllBtn" class="complete-all">✅ Complete All</button>
            <button id="clearCompletedBtn">🗑️ Clear Completed</button>
            <button id="clearAllBtn" class="clear-all">⚠️ Clear All</button>
        </div>

        <!-- Undo/Redo -->
        <div class="undo-redo">
            <button id="undoBtn" disabled>↩️ Undo</button>
            <button id="redoBtn" disabled>↪️ Redo</button>
        </div>
    </div>

    <script>
        // ==================== STATE MANAGEMENT ====================
        let tasks = [];
        let history = [];
        let historyIndex = -1;
        let currentFilter = 'all';
        let dragSource = null;

        // ==================== INITIALIZATION ====================
        document.addEventListener('DOMContentLoaded', () => {
            loadFromLocalStorage();
            renderTasks();
            setupEventListeners();
            updateStats();
        });

        // ==================== CORE FUNCTIONS ====================

        /**
         * Save current state to history (for undo/redo)
         * DEMONSTRATES: Deep cloning with JSON
         */
        function saveToHistory() {
            // Remove any future history if we're in the middle
            if (historyIndex < history.length - 1) {
                history = history.slice(0, historyIndex + 1);
            }
            
            // Deep clone the tasks array
            const snapshot = JSON.parse(JSON.stringify(tasks));
            history.push(snapshot);
            historyIndex++;
            
            // Limit history size
            if (history.length > 20) {
                history.shift();
                historyIndex--;
            }
            
            updateUndoRedoButtons();
        }

        /**
         * Undo last action
         * DEMONSTRATES: History traversal
         */
        function undo() {
            if (historyIndex > 0) {
                historyIndex--;
                tasks = JSON.parse(JSON.stringify(history[historyIndex]));
                renderTasks();
                saveToLocalStorage();
                updateUndoRedoButtons();
                showNotification('Undo successful', 'success');
            }
        }

        /**
         * Redo previously undone action
         */
        function redo() {
            if (historyIndex < history.length - 1) {
                historyIndex++;
                tasks = JSON.parse(JSON.stringify(history[historyIndex]));
                renderTasks();
                saveToLocalStorage();
                updateUndoRedoButtons();
                showNotification('Redo successful', 'success');
            }
        }

        /**
         * Add a new task
         * DEMONSTRATES: Document Fragment for performance
         */
        function addTask(text) {
            if (!text.trim()) {
                showNotification('Task cannot be empty!', 'error');
                return;
            }

            const newTask = {
                id: Date.now().toString(),
                text: text,
                completed: false,
                createdAt: new Date().toISOString()
            };

            tasks.push(newTask);
            saveToHistory();
            
            // DEMONSTRATION: Using DocumentFragment for efficient rendering
            const fragment = document.createDocumentFragment();
            const taskElement = createTaskElement(newTask);
            fragment.appendChild(taskElement);
            
            // Append to DOM in one operation
            document.getElementById('todoList').appendChild(fragment);
            
            saveToLocalStorage();
            updateStats();
            showNotification('Task added successfully!', 'success');
            
            // Clear input
            document.getElementById('taskInput').value = '';
        }

        /**
         * Create a task element
         * DEMONSTRATES: Dynamic element creation with data attributes
         */
        function createTaskElement(task) {
            const li = document.createElement('li');
            li.className = `todo-item ${task.completed ? 'completed' : ''}`;
            li.setAttribute('data-id', task.id);
            li.setAttribute('draggable', 'true');
            
            // DEMONSTRATION: Using data attributes
            li.dataset.createdAt = task.createdAt;
            li.dataset.completed = task.completed;

            // Checkbox
            const checkbox = document.createElement('input');
            checkbox.type = 'checkbox';
            checkbox.className = 'todo-checkbox';
            checkbox.checked = task.completed;
            
            // Task text
            const taskText = document.createElement('span');
            taskText.className = 'task-text';
            taskText.textContent = task.text;

            // Action buttons container
            const actions = document.createElement('div');
            actions.className = 'task-actions';

            // Edit button
            const editBtn = document.createElement('button');
            editBtn.className = 'edit-btn';
            editBtn.innerHTML = '✏️ Edit';
            
            // Delete button
            const deleteBtn = document.createElement('button');
            deleteBtn.className = 'delete-btn';
            deleteBtn.innerHTML = '🗑️ Delete';

            actions.appendChild(editBtn);
            actions.appendChild(deleteBtn);
            
            li.appendChild(checkbox);
            li.appendChild(taskText);
            li.appendChild(actions);

            // DEMONSTRATION: Direct event listeners for edit (not delegated for demo purposes)
            editBtn.addEventListener('click', (e) => {
                e.stopPropagation();
                editTask(task.id);
            });

            return li;
        }

        /**
         * Render all tasks based on current filter
         * DEMONSTRATES: Batch DOM updates with DocumentFragment
         */
        function renderTasks() {
            const todoList = document.getElementById('todoList');
            const fragment = document.createDocumentFragment();
            
            // Filter tasks
            const filteredTasks = filterTasks(tasks, currentFilter);
            
            // Create elements in memory
            filteredTasks.forEach(task => {
                fragment.appendChild(createTaskElement(task));
            });
            
            // Clear and update DOM in one operation
            todoList.innerHTML = '';
            todoList.appendChild(fragment);
            
            updateStats();
        }

        /**
         * Filter tasks based on current filter
         * DEMONSTRATES: Array filtering
         */
        function filterTasks(taskArray, filter) {
            switch(filter) {
                case 'active':
                    return taskArray.filter(t => !t.completed);
                case 'completed':
                    return taskArray.filter(t => t.completed);
                default:
                    return taskArray;
            }
        }

        /**
         * Toggle task completion
         * DEMONSTRATES: Class manipulation and data attributes
         */
        function toggleTask(taskId) {
            const task = tasks.find(t => t.id === taskId);
            if (task) {
                task.completed = !task.completed;
                
                // Update DOM element
                const taskElement = document.querySelector(`[data-id="${taskId}"]`);
                if (taskElement) {
                    taskElement.classList.toggle('completed');
                    const checkbox = taskElement.querySelector('.todo-checkbox');
                    checkbox.checked = task.completed;
                    
                    // Update data attribute
                    taskElement.dataset.completed = task.completed;
                }
                
                saveToHistory();
                saveToLocalStorage();
                updateStats();
                showNotification(`Task marked as ${task.completed ? 'completed' : 'active'}`, 'success');
            }
        }

        /**
         * Delete a task
         * DEMONSTRATES: DOM removal
         */
        function deleteTask(taskId) {
            const taskIndex = tasks.findIndex(t => t.id === taskId);
            if (taskIndex > -1) {
                tasks.splice(taskIndex, 1);
                
                // Remove from DOM
                const taskElement = document.querySelector(`[data-id="${taskId}"]`);
                if (taskElement) {
                    taskElement.remove();
                }
                
                saveToHistory();
                saveToLocalStorage();
                updateStats();
                showNotification('Task deleted', 'success');
            }
        }

        /**
         * Edit a task
         * DEMONSTRATES: Dynamic content update
         */
        function editTask(taskId) {
            const task = tasks.find(t => t.id === taskId);
            if (!task) return;
            
            const newText = prompt('Edit task:', task.text);
            if (newText !== null && newText.trim()) {
                task.text = newText.trim();
                
                // Update DOM
                const taskElement = document.querySelector(`[data-id="${taskId}"]`);
                const taskTextElement = taskElement.querySelector('.task-text');
                taskTextElement.textContent = newText.trim();
                
                saveToHistory();
                saveToLocalStorage();
                showNotification('Task updated', 'success');
            }
        }

        /**
         * Clear all tasks
         * DEMONSTRATES: Batch DOM removal
         */
        function clearAllTasks() {
            if (tasks.length === 0) return;
            
            if (confirm('Are you sure you want to delete ALL tasks?')) {
                tasks = [];
                
                // Clear DOM
                document.getElementById('todoList').innerHTML = '';
                
                saveToHistory();
                saveToLocalStorage();
                updateStats();
                showNotification('All tasks cleared', 'success');
            }
        }

        /**
         * Clear completed tasks
         * DEMONSTRATES: Filtering and batch removal
         */
        function clearCompletedTasks() {
            const completedTasks = tasks.filter(t => t.completed);
            if (completedTasks.length === 0) {
                showNotification('No completed tasks to clear', 'error');
                return;
            }
            
            tasks = tasks.filter(t => !t.completed);
            
            // Re-render everything
            renderTasks();
            
            saveToHistory();
            saveToLocalStorage();
            showNotification('Completed tasks cleared', 'success');
        }

        /**
         * Complete all tasks
         * DEMONSTRATES: Batch updates
         */
        function completeAllTasks() {
            if (tasks.length === 0) return;
            
            tasks.forEach(task => task.completed = true);
            
            // Re-render
            renderTasks();
            
            saveToHistory();
            saveToLocalStorage();
            showNotification('All tasks completed', 'success');
        }

        /**
         * Update statistics
         * DEMONSTRATES: Live DOM updates
         */
        function updateStats() {
            const total = tasks.length;
            const completed = tasks.filter(t => t.completed).length;
            const pending = total - completed;
            
            document.getElementById('totalCount').textContent = total;
            document.getElementById('completedCount').textContent = completed;
            document.getElementById('pendingCount').textContent = pending;
        }

        /**
         * Update undo/redo buttons state
         */
        function updateUndoRedoButtons() {
            document.getElementById('undoBtn').disabled = historyIndex <= 0;
            document.getElementById('redoBtn').disabled = historyIndex >= history.length - 1;
        }

        /**
         * Save to localStorage
         * DEMONSTRATES: Web Storage API
         */
        function saveToLocalStorage() {
            localStorage.setItem('advancedTodoTasks', JSON.stringify(tasks));
        }

        /**
         * Load from localStorage
         */
        function loadFromLocalStorage() {
            const saved = localStorage.getItem('advancedTodoTasks');
            if (saved) {
                tasks = JSON.parse(saved);
                saveToHistory();
            }
        }

        /**
         * Show notification
         * DEMONSTRATES: Dynamic element creation and removal
         */
        function showNotification(message, type = 'success') {
            const notification = document.createElement('div');
            notification.className = `notification ${type}`;
            notification.textContent = message;
            
            document.body.appendChild(notification);
            
            // Auto-remove after 2 seconds
            setTimeout(() => {
                notification.remove();
            }, 2000);
        }

        // ==================== DRAG AND DROP IMPLEMENTATION ====================
        /**
         * DEMONSTRATES: Drag and drop DOM manipulation
         */

        function handleDragStart(e) {
            dragSource = this;
            this.classList.add('dragging');
            e.dataTransfer.setData('text/plain', this.dataset.id);
        }

        function handleDragOver(e) {
            e.preventDefault();
            const dropTarget = e.target.closest('.todo-item');
            if (dropTarget && dropTarget !== dragSource) {
                const bounding = dropTarget.getBoundingClientRect();
                const offset = e.clientY - bounding.top;
                
                if (offset > bounding.height / 2) {
                    dropTarget.style.borderBottom = '2px solid #667eea';
                    dropTarget.style.borderTop = 'none';
                } else {
                    dropTarget.style.borderTop = '2px solid #667eea';
                    dropTarget.style.borderBottom = 'none';
                }
            }
        }

        function handleDragLeave(e) {
            const dropTarget = e.target.closest('.todo-item');
            if (dropTarget) {
                dropTarget.style.borderTop = 'none';
                dropTarget.style.borderBottom = 'none';
            }
        }

        function handleDrop(e) {
            e.preventDefault();
            const dropTarget = e.target.closest('.todo-item');
            
            if (dropTarget && dragSource && dragSource !== dropTarget) {
                const dragId = dragSource.dataset.id;
                const dropId = dropTarget.dataset.id;
                
                // Reorder tasks array
                const dragIndex = tasks.findIndex(t => t.id === dragId);
                const dropIndex = tasks.findIndex(t => t.id === dropId);
                
                const [draggedTask] = tasks.splice(dragIndex, 1);
                tasks.splice(dropIndex, 0, draggedTask);
                
                // Reorder DOM
                const bounding = dropTarget.getBoundingClientRect();
                const offset = e.clientY - bounding.top;
                
                if (offset > bounding.height / 2) {
                    dropTarget.parentNode.insertBefore(dragSource, dropTarget.nextSibling);
                } else {
                    dropTarget.parentNode.insertBefore(dragSource, dropTarget);
                }
                
                saveToHistory();
                saveToLocalStorage();
                showNotification('Task reordered', 'success');
            }
            
            // Cleanup
            document.querySelectorAll('.todo-item').forEach(item => {
                item.style.borderTop = 'none';
                item.style.borderBottom = 'none';
                item.classList.remove('dragging');
            });
        }

        // ==================== EVENT DELEGATION SETUP ====================
        /**
         * DEMONSTRATES: Event delegation for dynamic elements
         */
        function setupEventListeners() {
            const todoList = document.getElementById('todoList');
            
            // EVENT DELEGATION: Single listener for all todo items
            todoList.addEventListener('click', (e) => {
                const target = e.target;
                
                // Handle checkbox toggle
                if (target.classList.contains('todo-checkbox')) {
                    const todoItem = target.closest('.todo-item');
                    if (todoItem) {
                        toggleTask(todoItem.dataset.id);
                    }
                }
                
                // Handle delete button
                if (target.classList.contains('delete-btn')) {
                    const todoItem = target.closest('.todo-item');
                    if (todoItem) {
                        deleteTask(todoItem.dataset.id);
                    }
                }
            });

            // Drag and drop events
            todoList.addEventListener('dragstart', handleDragStart);
            todoList.addEventListener('dragover', handleDragOver);
            todoList.addEventListener('dragleave', handleDragLeave);
            todoList.addEventListener('drop', handleDrop);
            todoList.addEventListener('dragend', () => {
                document.querySelectorAll('.todo-item').forEach(item => {
                    item.classList.remove('dragging');
                });
            });

            // Add task button
            document.getElementById('addTaskBtn').addEventListener('click', () => {
                const input = document.getElementById('taskInput');
                addTask(input.value);
            });

            // Enter key in input
            document.getElementById('taskInput').addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    addTask(e.target.value);
                }
            });

            // Filter buttons
            document.querySelectorAll('.filter-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    // Update active filter UI
                    document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
                    e.target.classList.add('active');
                    
                    // Apply filter
                    currentFilter = e.target.dataset.filter;
                    renderTasks();
                });
            });

            // Bulk actions
            document.getElementById('completeAllBtn').addEventListener('click', completeAllTasks);
            document.getElementById('clearCompletedBtn').addEventListener('click', clearCompletedTasks);
            document.getElementById('clearAllBtn').addEventListener('click', clearAllTasks);

            // Undo/Redo
            document.getElementById('undoBtn').addEventListener('click', undo);
            document.getElementById('redoBtn').addEventListener('click', redo);

            // Keyboard shortcuts
            document.addEventListener('keydown', (e) => {
                if (e.ctrlKey && e.key === 'z') {
                    e.preventDefault();
                    undo();
                }
                if (e.ctrlKey && e.key === 'y') {
                    e.preventDefault();
                    redo();
                }
            });
        }
    </script>
</body>
</html>
```

## 🎨 Advanced DOM Concepts Demonstrated

### 1. **Document Fragment for Performance**
```javascript
// Building multiple elements in memory before DOM insertion
const fragment = document.createDocumentFragment();
filteredTasks.forEach(task => {
    fragment.appendChild(createTaskElement(task));
});
todoList.appendChild(fragment); // Single reflow!
```

### 2. **Event Delegation**
```javascript
todoList.addEventListener('click', (e) => {
    // ONE listener handles ALL todo items
    if (e.target.classList.contains('delete-btn')) {
        deleteTask(e.target.closest('.todo-item').dataset.id);
    }
});
```

### 3. **Data Attributes**
```javascript
// Storing metadata directly in elements
li.setAttribute('data-id', task.id);
li.dataset.createdAt = task.createdAt;

// Retrieving data
const taskId = taskElement.dataset.id;
```

### 4. **Dynamic Element Creation**
```javascript
function createTaskElement(task) {
    const li = document.createElement('li');
    const checkbox = document.createElement('input');
    const span = document.createElement('span');
    // ... building complex structure
}
```

### 5. **CSS Class Manipulation**
```javascript
taskElement.classList.toggle('completed');
taskElement.classList.add('dragging');
taskElement.classList.remove('dragging');
```

### 6. **DOM Traversal**
```javascript
const todoItem = e.target.closest('.todo-item');
const nextSibling = dragSource.nextElementSibling;
const parent = dropTarget.parentNode;
```

### 7. **Local Storage API**
```javascript
localStorage.setItem('tasks', JSON.stringify(tasks));
const saved = localStorage.getItem('tasks');
```

### 8. **Animation with DOM**
```javascript
// Adding/removing classes for CSS transitions
@keyframes slideIn {
    from { opacity: 0; transform: translateY(-10px); }
    to { opacity: 1; transform: translateY(0); }
}
```

### 9. **Drag and Drop API**
```javascript
element.setAttribute('draggable', 'true');
element.addEventListener('dragstart', handleDragStart);
element.addEventListener('dragover', handleDragOver);
```

### 10. **History Management (Undo/Redo)**
```javascript
const snapshot = JSON.parse(JSON.stringify(tasks));
history.push(snapshot);
```

## 📊 Performance Optimizations

| Technique | Benefit |
|-----------|---------|
| Document Fragment | Minimizes reflows/repaints |
| Event Delegation | Reduces memory usage |
| Batch DOM Updates | Single reflow instead of many |
| Local Storage Caching | Persists state across sessions |
| Debounced Input | Prevents excessive rendering |

## 🎮 Interactive Features

1. **Add Task** - With animation
2. **Delete Task** - Immediate removal
3. **Edit Task** - Inline editing
4. **Complete Task** - Visual feedback
5. **Filter Tasks** - All/Active/Completed
6. **Drag to Reorder** - Visual feedback
7. **Bulk Actions** - Complete/Clear all
8. **Undo/Redo** - With keyboard shortcuts
9. **Live Statistics** - Updates automatically
10. **Notifications** - Visual feedback
11. **Persistent Storage** - Saves to browser

## 🚀 Running the Application

1. Copy the entire HTML code into a file named `index.html`
2. Open it in any modern browser
3. Start adding tasks!

## 📝 Key Takeaways

- **Always use DocumentFragment** for batch operations
- **Leverage event delegation** for dynamic content
- **Use data attributes** to store element metadata
- **Implement undo/redo** for better UX
- **Cache DOM queries** when possible
- **Use CSS classes** for styling states
- **Keep DOM manipulations** to a minimum

This todo list demonstrates professional-grade DOM manipulation techniques used in modern web applications. Each feature showcases a specific concept while maintaining clean, maintainable code!
