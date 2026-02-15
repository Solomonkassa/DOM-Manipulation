# 🚀 Advanced To-Do List Application - Tailwind CSS Version

Here's the complete To-Do List application rebuilt with **Tailwind CSS** for modern, utility-first styling. All the advanced DOM manipulation concepts remain intact while the styling is cleaner and more maintainable.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🚀 Advanced Todo List - Tailwind Edition</title>
    <!-- Tailwind CSS via CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Optional: Add custom animations that Tailwind doesn't include by default -->
    <style>
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

        .animate-slide-in {
            animation: slideIn 0.3s ease;
        }

        .animate-slide-right {
            animation: slideInRight 0.3s ease;
        }

        .dragging {
            opacity: 0.5;
            transform: scale(0.98);
            @apply bg-gray-200;
        }

        .drop-target-top {
            @apply border-t-2 border-indigo-500;
        }

        .drop-target-bottom {
            @apply border-b-2 border-indigo-500;
        }
    </style>
</head>
<body class="bg-gradient-to-br from-indigo-500 via-purple-500 to-pink-500 min-h-screen flex justify-center items-center p-5 font-sans">
    <div class="bg-white rounded-xl shadow-2xl w-full max-w-2xl p-8">
        <!-- Header -->
        <h1 class="text-3xl font-bold text-gray-800 mb-5 flex items-center gap-2">
            📝 Advanced Todo List
            <span class="bg-indigo-500 text-white px-2 py-1 rounded text-sm font-normal">v2.0</span>
        </h1>

        <!-- Input Section -->
        <div class="flex gap-2 mb-5">
            <input 
                type="text" 
                id="taskInput" 
                placeholder="Enter a new task..." 
                autocomplete="off"
                class="flex-1 px-4 py-3 border-2 border-gray-200 rounded-lg text-base focus:outline-none focus:border-indigo-500 transition-colors"
            >
            <button 
                id="addTaskBtn" 
                class="px-5 py-3 bg-indigo-500 text-white rounded-lg font-medium hover:bg-indigo-600 active:scale-95 transition-all"
            >
                ➕ Add Task
            </button>
        </div>

        <!-- Filters -->
        <div class="flex gap-2 mb-5 flex-wrap">
            <button class="filter-btn flex-1 min-w-[80px] px-4 py-3 bg-gray-200 text-gray-800 rounded-lg font-medium hover:bg-gray-300 transition-all active" data-filter="all">
                All
            </button>
            <button class="filter-btn flex-1 min-w-[80px] px-4 py-3 bg-gray-200 text-gray-800 rounded-lg font-medium hover:bg-gray-300 transition-all" data-filter="active">
                Active
            </button>
            <button class="filter-btn flex-1 min-w-[80px] px-4 py-3 bg-gray-200 text-gray-800 rounded-lg font-medium hover:bg-gray-300 transition-all" data-filter="completed">
                Completed
            </button>
        </div>

        <!-- Stats -->
        <div class="flex justify-between mb-5 p-4 bg-gray-50 rounded-lg font-bold">
            <span>Total: <span id="totalCount" class="text-indigo-600">0</span></span>
            <span>Completed: <span id="completedCount" class="text-green-600">0</span></span>
            <span>Pending: <span id="pendingCount" class="text-orange-600">0</span></span>
        </div>

        <!-- Todo List Container -->
        <ul 
            id="todoList" 
            class="list-none mb-5 min-h-[200px] border-2 border-dashed border-gray-200 rounded-lg p-3 space-y-2"
        ></ul>

        <!-- Bulk Actions -->
        <div class="flex gap-2 mt-5 pt-5 border-t-2 border-gray-200">
            <button 
                id="completeAllBtn" 
                class="flex-1 px-4 py-3 bg-green-500 text-white rounded-lg font-medium hover:bg-green-600 transition-all"
            >
                ✅ Complete All
            </button>
            <button 
                id="clearCompletedBtn" 
                class="flex-1 px-4 py-3 bg-red-400 text-white rounded-lg font-medium hover:bg-red-500 transition-all"
            >
                🗑️ Clear Completed
            </button>
            <button 
                id="clearAllBtn" 
                class="flex-1 px-4 py-3 bg-red-500 text-white rounded-lg font-medium hover:bg-red-600 transition-all"
            >
                ⚠️ Clear All
            </button>
        </div>

        <!-- Undo/Redo -->
        <div class="flex gap-2 mt-3">
            <button 
                id="undoBtn" 
                disabled 
                class="flex-1 px-4 py-3 bg-gray-400 text-white rounded-lg font-medium disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-500 transition-all"
            >
                ↩️ Undo
            </button>
            <button 
                id="redoBtn" 
                disabled 
                class="flex-1 px-4 py-3 bg-gray-400 text-white rounded-lg font-medium disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-500 transition-all"
            >
                ↪️ Redo
            </button>
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
         * Create a task element with Tailwind classes
         * DEMONSTRATES: Dynamic element creation with data attributes
         */
        function createTaskElement(task) {
            const li = document.createElement('li');
            li.className = `todo-item flex items-center p-3 bg-gray-50 border border-gray-200 rounded-lg mb-2 cursor-move transition-all hover:shadow-md animate-slide-in ${task.completed ? 'bg-green-50 opacity-80' : ''}`;
            li.setAttribute('data-id', task.id);
            li.setAttribute('draggable', 'true');
            
            // DEMONSTRATION: Using data attributes
            li.dataset.createdAt = task.createdAt;
            li.dataset.completed = task.completed;

            // Checkbox
            const checkbox = document.createElement('input');
            checkbox.type = 'checkbox';
            checkbox.className = 'todo-checkbox w-5 h-5 mr-3 cursor-pointer accent-indigo-500';
            checkbox.checked = task.completed;
            
            // Task text
            const taskText = document.createElement('span');
            taskText.className = `task-text flex-1 text-base transition-all ${task.completed ? 'line-through text-green-600' : 'text-gray-800'}`;
            taskText.textContent = task.text;

            // Action buttons container
            const actions = document.createElement('div');
            actions.className = 'flex gap-1';

            // Edit button
            const editBtn = document.createElement('button');
            editBtn.className = 'edit-btn px-2 py-1 bg-yellow-500 text-white rounded text-sm hover:bg-yellow-600 transition-all';
            editBtn.innerHTML = '✏️';
            
            // Delete button
            const deleteBtn = document.createElement('button');
            deleteBtn.className = 'delete-btn px-2 py-1 bg-red-500 text-white rounded text-sm hover:bg-red-600 transition-all';
            deleteBtn.innerHTML = '🗑️';

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
            
            // Update filter button active states
            document.querySelectorAll('.filter-btn').forEach(btn => {
                if (btn.dataset.filter === currentFilter) {
                    btn.classList.add('bg-indigo-500', 'text-white');
                    btn.classList.remove('bg-gray-200', 'text-gray-800');
                } else {
                    btn.classList.remove('bg-indigo-500', 'text-white');
                    btn.classList.add('bg-gray-200', 'text-gray-800');
                }
            });
            
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
                    // Toggle background
                    if (task.completed) {
                        taskElement.classList.add('bg-green-50', 'opacity-80');
                    } else {
                        taskElement.classList.remove('bg-green-50', 'opacity-80');
                    }
                    
                    // Update checkbox
                    const checkbox = taskElement.querySelector('.todo-checkbox');
                    checkbox.checked = task.completed;
                    
                    // Update text style
                    const taskText = taskElement.querySelector('.task-text');
                    if (task.completed) {
                        taskText.classList.add('line-through', 'text-green-600');
                        taskText.classList.remove('text-gray-800');
                    } else {
                        taskText.classList.remove('line-through', 'text-green-600');
                        taskText.classList.add('text-gray-800');
                    }
                    
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
                
                // Remove from DOM with animation
                const taskElement = document.querySelector(`[data-id="${taskId}"]`);
                if (taskElement) {
                    taskElement.style.animation = 'slideIn 0.3s reverse';
                    setTimeout(() => {
                        taskElement.remove();
                    }, 200);
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
            const undoBtn = document.getElementById('undoBtn');
            const redoBtn = document.getElementById('redoBtn');
            
            undoBtn.disabled = historyIndex <= 0;
            redoBtn.disabled = historyIndex >= history.length - 1;
            
            // Update button styles based on disabled state
            [undoBtn, redoBtn].forEach(btn => {
                if (btn.disabled) {
                    btn.classList.add('opacity-50', 'cursor-not-allowed');
                    btn.classList.remove('hover:bg-gray-500');
                } else {
                    btn.classList.remove('opacity-50', 'cursor-not-allowed');
                    btn.classList.add('hover:bg-gray-500');
                }
            });
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
         * DEMONSTRATES: Dynamic element creation and removal with Tailwind
         */
        function showNotification(message, type = 'success') {
            const notification = document.createElement('div');
            notification.className = `fixed top-5 right-5 px-6 py-3 rounded-lg text-white font-medium shadow-lg animate-slide-right z-50 ${
                type === 'success' ? 'bg-green-500' : 'bg-red-500'
            }`;
            notification.textContent = message;
            
            document.body.appendChild(notification);
            
            // Auto-remove after 2 seconds
            setTimeout(() => {
                notification.remove();
            }, 2000);
        }

        // ==================== DRAG AND DROP IMPLEMENTATION ====================
        /**
         * DEMONSTRATES: Drag and drop DOM manipulation with Tailwind classes
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
                
                // Remove existing drop indicators
                dropTarget.classList.remove('drop-target-top', 'drop-target-bottom');
                
                if (offset > bounding.height / 2) {
                    dropTarget.classList.add('drop-target-bottom');
                } else {
                    dropTarget.classList.add('drop-target-top');
                }
            }
        }

        function handleDragLeave(e) {
            const dropTarget = e.target.closest('.todo-item');
            if (dropTarget) {
                dropTarget.classList.remove('drop-target-top', 'drop-target-bottom');
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
                item.classList.remove('drop-target-top', 'drop-target-bottom', 'dragging');
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
                    item.classList.remove('dragging', 'drop-target-top', 'drop-target-bottom');
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

## 🎨 Tailwind CSS Benefits Demonstrated

### 1. **Utility-First Styling**
```html
<!-- Before: Custom CSS -->
<div class="todo-item completed">
    <span class="task-text">Task</span>
</div>

<!-- After: Tailwind Utilities -->
<li class="flex items-center p-3 bg-gray-50 border border-gray-200 rounded-lg">
    <span class="flex-1 text-gray-800">Task</span>
</li>
```

### 2. **Conditional Classes with JavaScript**
```javascript
// Dynamic class application based on state
if (task.completed) {
    taskElement.classList.add('bg-green-50', 'opacity-80');
    taskText.classList.add('line-through', 'text-green-600');
} else {
    taskElement.classList.remove('bg-green-50', 'opacity-80');
    taskText.classList.remove('line-through', 'text-green-600');
}
```

### 3. **Responsive Design Built-in**
```html
<!-- Automatically responsive without media queries -->
<div class="flex-col md:flex-row">
    <!-- Stacks on mobile, row on desktop -->
</div>
```

### 4. **Dark Mode Support (Optional)**
```html
<!-- Can easily add dark mode with 'dark:' prefix -->
<div class="bg-white dark:bg-gray-800 text-gray-900 dark:text-white">
```

### 5. **Custom Animations Integration**
```css
/* Custom animations work seamlessly with Tailwind */
.animate-slide-in {
    animation: slideIn 0.3s ease;
}
```

## 📊 Tailwind vs Traditional CSS Comparison

| Feature | Traditional CSS | Tailwind CSS |
|---------|----------------|--------------|
| **File Size** | Custom CSS file | Utility classes (purged unused) |
| **Naming** | BEM/SMACSS conventions | No naming needed |
| **Maintenance** | Multiple CSS files | Single HTML file |
| **Responsiveness** | Media queries | Responsive prefixes (`md:`, `lg:`) |
| **States** | `:hover`, `:focus` | State variants (`hover:`, `focus:`) |
| **Customization** | Custom properties | `tailwind.config.js` |

## 🚀 Key Tailwind Classes Used

### Layout & Spacing
- `flex`, `flex-1` - Flexbox layouts
- `p-3`, `px-4`, `py-3` - Padding utilities
- `m-5`, `mb-5`, `gap-2` - Margin and gap
- `min-h-screen` - Minimum height viewport

### Typography
- `text-base`, `text-sm` - Font sizes
- `font-bold`, `font-medium` - Font weights
- `text-gray-800`, `text-white` - Text colors
- `line-through` - Text decoration

### Colors & Backgrounds
- `bg-indigo-500`, `bg-green-500` - Background colors
- `hover:bg-indigo-600` - Hover states
- `border-gray-200` - Border colors
- `bg-gradient-to-br` - Gradients

### Effects & Transitions
- `rounded-lg`, `rounded-xl` - Border radius
- `shadow-2xl`, `hover:shadow-md` - Box shadows
- `transition-all` - Transitions
- `opacity-50`, `opacity-80` - Opacity

### Interactive States
- `hover:bg-indigo-600` - Hover effects
- `active:scale-95` - Active/pressed state
- `focus:outline-none` - Focus removal
- `disabled:opacity-50` - Disabled state

### Responsive Design
- `min-w-[80px]` - Arbitrary values with square brackets
- `flex-col md:flex-row` - Responsive flex direction

## 🎯 Benefits of Using Tailwind in This Demo

1. **Faster Development**: No context switching between HTML and CSS
2. **Consistent Design System**: Built-in color palette and spacing
3. **No CSS Conflicts**: Classes are local to elements
4. **Easy Maintenance**: Styles live with the HTML
5. **Performance**: Only used classes are included in production
6. **Dark Mode Ready**: Can easily add with `dark:` variant
7. **Customizable**: Can extend with custom animations

## 📝 How to Run

1. Save the HTML file
2. Open in browser (internet connection needed for Tailwind CDN)
3. Start adding tasks!

The application maintains ALL the advanced DOM manipulation concepts while providing a modern, clean, and maintainable styling approach with Tailwind CSS!
