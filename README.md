<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Study Planner</title>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&family=Roboto:wght@300;400&display=swap" rel="stylesheet">
    <!-- Optional: Colorful favicon -->
    <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><rect width='100' height='100' fill='%23667eea'/><text y='.9em' font-size='90'>📚</text></svg>">
    <style>
        /* Vibrant Body Background */
        body {
            font-family: 'Roboto', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
            background-attachment: fixed;
            margin: 0;
            padding: 20px;
            font-weight: 300;
            color: #333;
            min-height: 100vh;
        }

        /* Container with Colorful Border */
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 8px 32px rgba(102, 126, 234, 0.3);
            border: 2px solid transparent;
            background-clip: padding-box;
            position: relative;
        }

        .container::before {
            content: '';
            position: absolute;
            top: -2px; left: -2px; bottom: -2px; right: -2px;
            background: linear-gradient(135deg, #667eea, #764ba2, #f093fb, #f5576c);
            border-radius: 17px;
            z-index: -1;
        }

        /* Section-Specific Colors */
        .header-section {
            background: linear-gradient(135deg, rgba(102, 126, 234, 0.1), rgba(118, 75, 162, 0.1));
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        .add-task-section {
            background: linear-gradient(135deg, rgba(76, 175, 80, 0.05), rgba(255, 152, 0, 0.05));
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        .progress-section {
            background: linear-gradient(135deg, rgba(76, 175, 80, 0.1), rgba(0, 188, 212, 0.1));
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
            text-align: center;
        }

        .task-list-section {
            background: linear-gradient(135deg, rgba(33, 150, 243, 0.05), rgba(255, 87, 34, 0.05));
            padding: 20px;
            border-radius: 10px;
        }

        /* Colored Title Boxes */
        .title-box {
            font-family: 'Poppins', sans-serif;
            font-weight: 600;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
            color: white;
            padding: 15px 25px;
            border-radius: 12px;
            margin: 0 0 15px 0;
            text-align: center;
            box-shadow: 0 6px 12px rgba(102, 126, 234, 0.3);
            position: relative;
            overflow: hidden;
        }

        .title-box::after {
            content: '';
            position: absolute;
            top: 0; left: -100%; width: 100%; height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transition: left 0.5s;
        }

        .title-box:hover::after {
            left: 100%;
        }

        .progress-title {
            background: linear-gradient(135deg, #4CAF50 0%, #00BCD4 100%);
        }

        .task-title {
            background: linear-gradient(135deg, #2196F3 0%, #FF5722 100%);
            display: inline-block;
            margin: 0;
        }

        header p {
            font-family: 'Roboto', sans-serif;
            color: #555;
            text-align: center;
            margin-top: 10px;
            font-size: 16px;
        }

        /* Add Task Section */
        .add-task {
            display: flex;
            gap: 12px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .add-task input, .add-task select {
            padding: 14px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-family: 'Roboto', sans-serif;
            font-size: 14px;
            background: rgba(255, 255, 255, 0.8);
            transition: all 0.3s ease;
            flex: 1;
            min-width: 150px;
        }

        .add-task input:focus, .add-task select:focus {
            border-color: #667eea;
            box-shadow: 0 0 10px rgba(102, 126, 234, 0.3);
            outline: none;
            background: white;
        }

        /* Buttons */
        button {
            padding: 12px 18px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-family: 'Poppins', sans-serif;
            font-weight: 500;
            font-size: 14px;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            position: relative;
            overflow: hidden;
        }

        button::before {
            content: '';
            position: absolute;
            top: 0; left: -100%; width: 100%; height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
            transition: left 0.5s;
        }

        button:hover::before {
            left: 100%;
        }

        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 16px rgba(0,0,0,0.2);
            filter: brightness(1.1);
        }

        .add-task button {
            background: linear-gradient(135deg, #4CAF50 0%, #8BC34A 100%);
            color: white;
            flex-shrink: 0;
        }

        .task-controls button {
            background: linear-gradient(135deg, #FF9800 0%, #FF5722 100%);
            color: white;
            padding: 10px 16px;
            font-size: 13px;
            margin-left: 10px;
        }

        .complete-btn {
            background: linear-gradient(135deg, #4CAF50 0%, #8BC34A 100%);
            color: white;
        }

        .edit-btn {
            background: linear-gradient(135deg, #2196F3 0%, #03A9F4 100%);
            color: white;
        }

        .delete-btn {
            background: linear-gradient(135deg, #f44336 0%, #E57373 100%);
            color: white;
        }

        /* Progress Section */
        .progress-section p {
            font-family: 'Roboto', sans-serif;
            font-size: 18px;
            margin-top: 15px;
            color: #333;
            font-weight: 400;
        }

        .progress-bar {
            width: 100%;
            height: 30px;
            background: linear-gradient(135deg, #e0e0e0, #f5f5f5);
            border-radius: 15px;
            overflow: hidden;
            margin: 15px 0;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(135deg, #4CAF50 0%, #FFEB3B 50%, #FF9800 100%);
            width: 0%;
            transition: width 0.5s ease;
            box-shadow: 0 0 10px rgba(76, 175, 80, 0.5);
        }

        /* Task List */
        .task-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .task-list ul {
            list-style: none;
            padding: 0;
        }

        .task-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 18px;
            border: 2px solid #eee;
            margin-bottom: 12px;
            border-radius: 12px;
            background: linear-gradient(135deg, rgba(255,255,255,0.9), rgba(248,248,255,0.9));
            font-family: 'Roboto', sans-serif;
            transition: all 0.3s ease;
            position: relative;
        }

        .task-item:hover {
            border-color: #667eea;
            box-shadow: 0 4px 12px rgba(102, 126, 234, 0.2);
            transform: scale(1.02);
        }

        .task-item.completed {
            background: linear-gradient(135deg, rgba(232, 245, 233, 0.8), rgba(200, 230, 201, 0.8));
            opacity: 0.9;
            border-color: #4CAF50;
        }

        .task-title-span {
            flex-grow: 1;
            padding: 10px 15px;
            border-radius: 8px;
            color: white;
            font-weight: 600;
            margin-right: 15px;
            font-size: 16px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .task-title-span.high {
            background: linear-gradient(135deg, #f44336, #EF5350);
        }

        .task-title-span.medium {
            background: linear-gradient(135deg, #FF9800, #FFB74D);
        }

        .task-title-span.low {
            background: linear-gradient(135deg, #4CAF50, #81C784);
        }

        .task-detail {
            color: #FF5722 !important;
            font-size: 13px;
            margin-right: 10px;
        }

        .task-item button {
            padding: 10px 14px;
            margin-left: 8px;
            border-radius: 8px;
            font-size: 13px;
            font-family: 'Poppins', sans-serif;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        /* Responsive */
        @media (max-width: 600px) {
            body { padding: 10px; }
            .container { padding: 15px; }
            .add-task, .task-controls { flex-direction: column; }
            .task-item { flex-direction: column; align-items: flex-start; gap: 12px; }
            .task-title-span { margin-right: 0; width: 100%; }
            .title-box { font-size: 18px; padding: 12px 15px; }
        }
    </style>
</head>
<body>
    <div class="container">
        <header class="header-section">
            <h1 class="title-box">🎯 Smart Study Planner</h1>
            <p>Manage your tasks and track progress to boost productivity!</p>
        </header>

        <section class="add-task-section add-task">
            <input type="text" id="taskTitle" placeholder="Enter Task Title..." required>
            <input type="date" id="dueDate">
            <select id="priority">
                <option value="low">Low 🌟</option>
                <option value="medium">Medium ⚡</option>
                <option value="high">High 🚨</option>
            </select>
            <button onclick="addTask()" title="Add a new task">➕ Add Task</button>
        </section>

        <section class="progress-section">
            <h2 class="title-box progress-title">📊 Progress Overview</h2>
            <div class="progress-bar">
                <div id="progressFill" class="progress-fill"></div>
            </div>
            <p id="progressText">0% Complete (0/0 tasks)</p>
        </section>

        <section class="task-list-section task-list">
            <div class="task-controls">
                <h2 class="title-box task-title">📝 Your Tasks</h2>
                <div>
                    <button onclick="clearCompleted()" title="Remove all completed tasks">🗑️ Clear Completed</button>
                    <button onclick="toggleSort()" id="sortBtn" title="Sort tasks by priority">🔄 Sort by Priority</button>
                </div>
            </div>
            <ul id="taskList"></ul>
        </section>
    </div>

    <script>
        let tasks = JSON.parse(localStorage.getItem('tasks')) || [];
        let isSorted = false;

        document.addEventListener('DOMContentLoaded', () => {
            renderTasks();
            updateProgress();
        });

        function addTask() {
            const title = document.getElementById('taskTitle').value.trim();
            const dueDate = document.getElementById('dueDate').value;
            const priority = document.getElementById('priority').value;

            if (!title) {
                alert('Please enter a task title!');
                return;
            }

            const task = {
                id: Date.now(),
                title,
                dueDate,
                priority,
                completed: false
            };

            tasks.push(task);
            saveTasks();
            renderTasks();
            updateProgress();

            document.getElementById('taskTitle').value = '';
            document.getElementById('dueDate').value = '';
        }

        function renderTasks() {
            const taskList = document.getElementById('taskList');
            taskList.innerHTML = '';

            let displayTasks = [...tasks];
            if (isSorted) {
                const priorityOrder = { high: 3, medium: 2, low: 1 };
                displayTasks.sort((a, b) => priorityOrder[b.priority] - priorityOrder[a.priority]);
            }

            displayTasks.forEach(task => {
                const li = document.createElement('li');
                li.className = `task-item ${task.completed ? 'completed' : ''}`;
                li.innerHTML = `
                    <span class="task-title-span ${task.priority}">${task.title}</span>
                    <span class="task-detail">(Due: ${task.dueDate || 'No date'}) - Priority: ${task.priority}</span>
                    <div>
                        <button class="complete-btn" onclick="toggleComplete(${task.id})">${task.completed ? '↩️ Undo' : '✅ Complete'}</button>
                        <button class="edit-btn" onclick="editTask(${task.id})">✏️ Edit</button>
                        <button class="delete-btn" onclick="deleteTask(${task.id})">🗑️ Delete</button>
                    </div>
                `;
                taskList.appendChild(li);
            });

            document.getElementById('sortBtn').textContent = isSorted ? '↩️ Unsorted' : '🔄 Sort by Priority';
        }

        function toggleComplete(id) {
            const task = tasks.find(t => t.id === id);
            if (task) {
                task.completed = !task.completed;
                saveTasks();
                renderTasks();
                updateProgress();
            }
        }

        function editTask(id) {
            const task = tasks.find(t => t.id === id);
            if (task) {
                const newTitle = prompt('Edit title:', task.title);
                if (newTitle !== null && newTitle.trim()) {
                    task.title = newTitle.trim();
                    saveTasks();
                    renderTasks();
                }
            }
        }

        function deleteTask(id) {
            if (confirm('Delete this task?')) {
                tasks = tasks.filter(t => t.id !== id);
                saveTasks();
                renderTasks();
                updateProgress();
            }
        }

        function clearCompleted() {
            if (confirm('Clear all completed tasks?')) {
                tasks = tasks.filter(t => !t.completed);
                saveTasks();
                renderTasks();
                updateProgress();
            }
        }

        function toggleSort() {
            isSorted = !isSorted;
            renderTasks();
        }

        function updateProgress() {
            const total = tasks.length;
            const completed = tasks.filter(t => t.completed).length;
            const percentage = total
