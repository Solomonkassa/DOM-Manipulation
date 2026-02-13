# Complete DOM Manipulation Course in JavaScript

## Table of Contents
1. [Introduction to DOM](#chapter-1-introduction-to-dom)
2. [Selecting Elements](#chapter-2-selecting-elements)
3. [Manipulating Elements](#chapter-3-manipulating-elements)
4. [Working with Attributes](#chapter-4-working-with-attributes)
5. [Creating and Removing Elements](#chapter-5-creating-and-removing-elements)
6. [Event Handling](#chapter-6-event-handling)
7. [Traversing the DOM](#chapter-7-traversing-the-dom)
8. [Styling Elements](#chapter-8-styling-elements)
9. [Advanced DOM Techniques](#chapter-9-advanced-dom-techniques)
10. [Practice Projects](#chapter-10-practice-projects)

---

## Chapter 1: Introduction to DOM

### What is DOM?
The Document Object Model (DOM) is a programming interface for HTML documents. It represents the page as a tree-like structure where each element, attribute, and text becomes an object that JavaScript can manipulate.

```html
<!DOCTYPE html>
<html>
<head>
    <title>DOM Introduction</title>
</head>
<body>
    <h1 id="main-title">Hello DOM!</h1>
    <p class="text">This is a paragraph.</p>
    <div class="container">
        <p>Nested paragraph</p>
    </div>

    <script>
        // DOM is ready!
        console.log(document); // The entire document
        console.log(document.body); // Body element
        console.log(document.head); // Head element
        console.log(document.title); // Title
        console.log(document.documentElement); // HTML element
    </script>
</body>
</html>
```

### Key Concepts
- **Document**: The root node of the DOM tree
- **Elements**: HTML tags represented as nodes
- **Attributes**: Properties of elements (class, id, etc.)
- **Text**: Text content within elements
- **Tree Structure**: Parent-child relationships between nodes

---

## Chapter 2: Selecting Elements

### 2.1 Basic Selectors

```html
<!DOCTYPE html>
<html>
<head>
    <title>DOM Selectors</title>
    <style>
        .highlight { background-color: yellow; }
        .selected { border: 2px solid blue; }
    </style>
</head>
<body>
    <h1 id="header">DOM Selectors Demo</h1>
    
    <div class="container">
        <p class="text">Paragraph 1</p>
        <p class="text">Paragraph 2</p>
        <p class="text special">Paragraph 3 (special)</p>
    </div>
    
    <ul id="list">
        <li>Item 1</li>
        <li class="important">Item 2</li>
        <li>Item 3</li>
    </ul>
    
    <div data-info="example">Custom Data</div>

    <script>
        // getElementById - Returns a single element
        const header = document.getElementById('header');
        console.log('Header:', header);
        
        // getElementsByClassName - Returns HTMLCollection (live)
        const paragraphs = document.getElementsByClassName('text');
        console.log('Paragraphs:', paragraphs);
        
        // getElementsByTagName - Returns HTMLCollection (live)
        const listItems = document.getElementsByTagName('li');
        console.log('List items:', listItems);
        
        // querySelector - Returns first matching element
        const firstPara = document.querySelector('.text');
        console.log('First paragraph:', firstPara);
        
        // querySelectorAll - Returns NodeList (static)
        const allParas = document.querySelectorAll('.text');
        console.log('All paragraphs:', allParas);
        
        // Working with collections
        allParas.forEach(para => {
            console.log('Paragraph text:', para.textContent);
        });
        
        // Select by attribute
        const dataElement = document.querySelector('[data-info]');
        console.log('Data element:', dataElement);
    </script>
</body>
</html>
```

### 2.2 Selector Methods Comparison

| Method | Returns | Live? | Browser Support |
|--------|---------|-------|-----------------|
| `getElementById()` | Single element | N/A | All |
| `getElementsByClassName()` | HTMLCollection | Yes | All |
| `getElementsByTagName()` | HTMLCollection | Yes | All |
| `querySelector()` | Single element | No | Modern |
| `querySelectorAll()` | NodeList | No | Modern |

---

## Chapter 3: Manipulating Elements

### 3.1 Content Manipulation

```html
<!DOCTYPE html>
<html>
<head>
    <title>Content Manipulation</title>
</head>
<body>
    <div id="demo">
        <p id="text-content">Original text</p>
        <div id="html-content">
            <span>Original HTML</span>
        </div>
        <input type="text" id="input-field" value="Input value">
    </div>
    
    <div id="output"></div>
    
    <button onclick="demonstrateText()">Show textContent</button>
    <button onclick="demonstrateInnerHTML()">Show innerHTML</button>
    <button onclick="demonstrateInnerText()">Show innerText</button>
    <button onclick="modifyContent()">Modify Content</button>

    <script>
        function demonstrateText() {
            const element = document.getElementById('text-content');
            alert('textContent: ' + element.textContent);
        }
        
        function demonstrateInnerHTML() {
            const element = document.getElementById('html-content');
            alert('innerHTML: ' + element.innerHTML);
        }
        
        function demonstrateInnerText() {
            const element = document.getElementById('text-content');
            alert('innerText: ' + element.innerText);
        }
        
        function modifyContent() {
            // textContent - sets plain text
            document.getElementById('text-content').textContent = 'Modified with textContent';
            
            // innerHTML - sets HTML content
            document.getElementById('html-content').innerHTML = '<strong>Modified with innerHTML</strong>';
            
            // value - for input elements
            document.getElementById('input-field').value = 'New input value';
            
            // Combining text and HTML
            const output = document.getElementById('output');
            output.innerHTML = `
                <h3>Modified Content:</h3>
                <p>Text content updated: ${document.getElementById('text-content').textContent}</p>
                <p>HTML content updated: ${document.getElementById('html-content').innerHTML}</p>
            `;
        }
        
        // Additional examples
        console.log('Example of differences:');
        const demo = document.getElementById('demo');
        console.log('innerHTML:', demo.innerHTML);
        console.log('textContent:', demo.textContent);
        console.log('innerText:', demo.innerText);
    </script>
</body>
</html>
```

### 3.2 Content Properties Comparison

| Property | Reads | Writes | Security |
|----------|-------|--------|----------|
| `innerHTML` | HTML content | HTML content | XSS risk |
| `textContent` | Raw text | Plain text | Safe |
| `innerText` | Rendered text | Plain text | Safe |
| `value` | Form field value | Form field value | Safe |

---

## Chapter 4: Working with Attributes

### 4.1 Attribute Manipulation

```html
<!DOCTYPE html>
<html>
<head>
    <title>Attribute Manipulation</title>
    <style>
        .dynamic-class {
            color: blue;
            font-weight: bold;
        }
        .highlight {
            background-color: yellow;
        }
    </style>
</head>
<body>
    <h2>Attribute Manipulation Demo</h2>
    
    <div id="target-element" 
         class="demo" 
         data-custom="initial value"
         style="padding: 10px; border: 1px solid black;">
        This element will have its attributes modified
    </div>
    
    <br>
    
    <button onclick="getAttributes()">Get Attributes</button>
    <button onclick="setAttributes()">Set Attributes</button>
    <button onclick="removeAttribute()">Remove Attribute</button>
    <button onclick="toggleClass()">Toggle Class</button>
    <button onclick="dataAttributes()">Work with Data Attributes</button>
    
    <div id="output" style="margin-top: 20px;"></div>

    <script>
        const target = document.getElementById('target-element');
        const output = document.getElementById('output');
        
        function getAttributes() {
            output.innerHTML = '<h3>Current Attributes:</h3>';
            
            // Get specific attribute
            const className = target.getAttribute('class');
            const style = target.getAttribute('style');
            const dataCustom = target.getAttribute('data-custom');
            
            output.innerHTML += `
                <p>class: ${className}</p>
                <p>style: ${style}</p>
                <p>data-custom: ${dataCustom}</p>
                <p>has class 'demo': ${target.hasAttribute('class')}</p>
                <p>has id: ${target.hasAttribute('id')}</p>
            `;
            
            // Get all attributes
            output.innerHTML += '<h4>All attributes:</h4>';
            Array.from(target.attributes).forEach(attr => {
                output.innerHTML += `<p>${attr.name} = ${attr.value}</p>`;
            });
        }
        
        function setAttributes() {
            // Set single attribute
            target.setAttribute('title', 'This is a tooltip');
            
            // Set multiple attributes
            target.setAttribute('data-modified', Date.now());
            target.setAttribute('aria-label', 'Modified element');
            
            output.innerHTML = '<p>Attributes set successfully!</p>';
        }
        
        function removeAttribute() {
            target.removeAttribute('data-custom');
            output.innerHTML = '<p>data-custom attribute removed</p>';
        }
        
        function toggleClass() {
            // Working with classList
            target.classList.toggle('dynamic-class');
            target.classList.toggle('highlight');
            
            output.innerHTML = `
                <p>Current classes: ${target.className}</p>
                <p>Contains 'dynamic-class': ${target.classList.contains('dynamic-class')}</p>
            `;
        }
        
        function dataAttributes() {
            // Working with dataset (modern way for data-* attributes)
            target.dataset.custom = 'new value';
            target.dataset.timestamp = Date.now();
            target.dataset.user = 'john_doe';
            
            output.innerHTML = '<h3>Dataset values:</h3>';
            for (let key in target.dataset) {
                output.innerHTML += `<p>data-${key}: ${target.dataset[key]}</p>`;
            }
        }
    </script>
</body>
</html>
```

### 4.2 Attribute Methods

| Method | Purpose |
|--------|---------|
| `getAttribute(name)` | Get attribute value |
| `setAttribute(name, value)` | Set attribute value |
| `removeAttribute(name)` | Remove attribute |
| `hasAttribute(name)` | Check if attribute exists |
| `classList.add()` | Add class |
| `classList.remove()` | Remove class |
| `classList.toggle()` | Toggle class |
| `classList.contains()` | Check if class exists |

---

## Chapter 5: Creating and Removing Elements

### 5.1 Dynamic Element Creation

```html
<!DOCTYPE html>
<html>
<head>
    <title>Creating and Removing Elements</title>
    <style>
        .card {
            border: 1px solid #ddd;
            padding: 15px;
            margin: 10px;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        .card-header {
            font-weight: bold;
            margin-bottom: 10px;
        }
        .card-body {
            color: #666;
        }
    </style>
</head>
<body>
    <h2>Dynamic Element Creation</h2>
    
    <div id="container"></div>
    
    <button onclick="createElement()">Create Element</button>
    <button onclick="createFragment()">Create Fragment</button>
    <button onclick="cloneElement()">Clone Element</button>
    <button onclick="removeElement()">Remove Last Element</button>
    <button onclick="clearContainer()">Clear Container</button>

    <script>
        const container = document.getElementById('container');
        let elementCount = 0;
        
        function createElement() {
            elementCount++;
            
            // Create new elements
            const card = document.createElement('div');
            const header = document.createElement('div');
            const body = document.createElement('div');
            const button = document.createElement('button');
            
            // Set attributes and content
            card.className = 'card';
            card.id = `card-${elementCount}`;
            
            header.className = 'card-header';
            header.textContent = `Card #${elementCount}`;
            
            body.className = 'card-body';
            body.textContent = `This card was created at ${new Date().toLocaleTimeString()}`;
            
            button.textContent = 'Click Me';
            button.onclick = function() {
                alert(`Card ${elementCount} clicked!`);
            };
            
            // Append elements
            card.appendChild(header);
            card.appendChild(body);
            card.appendChild(button);
            
            // Insert into container
            container.appendChild(card);
        }
        
        function createFragment() {
            // DocumentFragment - more efficient for multiple insertions
            const fragment = document.createDocumentFragment();
            
            for (let i = 0; i < 5; i++) {
                elementCount++;
                const card = document.createElement('div');
                card.className = 'card';
                card.innerHTML = `
                    <div class="card-header">Batch Card ${elementCount}</div>
                    <div class="card-body">Created with DocumentFragment</div>
                `;
                fragment.appendChild(card);
            }
            
            container.appendChild(fragment);
        }
        
        function cloneElement() {
            if (container.lastChild) {
                // Clone an existing element
                const original = container.lastChild;
                const clone = original.cloneNode(true); // true = deep clone
                
                // Modify clone to differentiate
                const header = clone.querySelector('.card-header');
                if (header) {
                    header.textContent += ' (Clone)';
                }
                
                container.appendChild(clone);
            }
        }
        
        function removeElement() {
            if (container.lastChild) {
                container.removeChild(container.lastChild);
                elementCount--;
            }
        }
        
        function clearContainer() {
            // Remove all children
            while (container.firstChild) {
                container.removeChild(container.firstChild);
            }
            elementCount = 0;
        }
    </script>
</body>
</html>
```

### 5.2 Creation Methods

| Method | Description |
|--------|-------------|
| `createElement(tagName)` | Create new element |
| `createTextNode(text)` | Create text node |
| `createDocumentFragment()` | Create fragment |
| `cloneNode(deep)` | Clone element |
| `appendChild(node)` | Add child at end |
| `insertBefore(new, reference)` | Insert before reference |
| `removeChild(node)` | Remove child |
| `replaceChild(new, old)` | Replace child |

---

## Chapter 6: Event Handling

### 6.1 Comprehensive Event Handling

```html
<!DOCTYPE html>
<html>
<head>
    <title>Event Handling</title>
    <style>
        .event-demo {
            padding: 20px;
            margin: 10px;
            border: 1px solid #ccc;
            cursor: pointer;
        }
        .hover-effect {
            background-color: #e0e0e0;
        }
        #event-log {
            height: 200px;
            overflow-y: auto;
            border: 1px solid #ccc;
            padding: 10px;
            margin-top: 20px;
            font-family: monospace;
        }
    </style>
</head>
<body>
    <h2>Event Handling Demo</h2>
    
    <div id="event-area">
        <button id="click-btn">Click Me (Single)</button>
        <button id="dblclick-btn">Double Click Me</button>
        <button id="context-btn">Right Click Me</button>
        
        <div id="mouse-area" class="event-demo">
            Mouse Event Area
            <p id="mouse-coords">Coordinates will appear here</p>
        </div>
        
        <input type="text" id="input-demo" placeholder="Type something...">
        <select id="select-demo">
            <option value="option1">Option 1</option>
            <option value="option2">Option 2</option>
            <option value="option3">Option 3</option>
        </select>
        
        <form id="form-demo">
            <input type="text" name="username" placeholder="Username">
            <input type="email" name="email" placeholder="Email">
            <button type="submit">Submit</button>
        </form>
    </div>
    
    <div id="event-log"></div>
    
    <button onclick="clearLog()">Clear Log</button>

    <script>
        const log = document.getElementById('event-log');
        
        function logEvent(event, message) {
            const timestamp = new Date().toLocaleTimeString();
            const logEntry = document.createElement('div');
            logEntry.textContent = `[${timestamp}] ${message} - Type: ${event.type}`;
            
            if (event.target.id) {
                logEntry.textContent += ` on #${event.target.id}`;
            }
            
            log.appendChild(logEntry);
            log.scrollTop = log.scrollHeight;
        }
        
        // Mouse Events
        document.getElementById('click-btn').addEventListener('click', (e) => {
            logEvent(e, 'Click event');
            e.target.style.backgroundColor = 
                e.target.style.backgroundColor === 'lightblue' ? '' : 'lightblue';
        });
        
        document.getElementById('dblclick-btn').addEventListener('dblclick', (e) => {
            logEvent(e, 'Double click event');
            alert('Double click detected!');
        });
        
        document.getElementById('context-btn').addEventListener('contextmenu', (e) => {
            e.preventDefault(); // Prevent context menu
            logEvent(e, 'Context menu event');
        });
        
        // Mouse movement
        const mouseArea = document.getElementById('mouse-area');
        const coords = document.getElementById('mouse-coords');
        
        mouseArea.addEventListener('mousemove', (e) => {
            coords.textContent = `X: ${e.offsetX}, Y: ${e.offsetY}`;
        });
        
        mouseArea.addEventListener('mouseenter', (e) => {
            e.target.classList.add('hover-effect');
            logEvent(e, 'Mouse entered');
        });
        
        mouseArea.addEventListener('mouseleave', (e) => {
            e.target.classList.remove('hover-effect');
            logEvent(e, 'Mouse left');
        });
        
        // Keyboard Events
        document.getElementById('input-demo').addEventListener('keydown', (e) => {
            logEvent(e, `Key down: ${e.key}`);
        });
        
        // Input Events
        document.getElementById('input-demo').addEventListener('input', (e) => {
            logEvent(e, `Input value: ${e.target.value}`);
        });
        
        // Form Events
        document.getElementById('form-demo').addEventListener('submit', (e) => {
            e.preventDefault(); // Prevent form submission
            logEvent(e, 'Form submitted');
        });
        
        function clearLog() {
            log.innerHTML = '';
        }
    </script>
</body>
</html>
```

### 6.2 Event Types

| Category | Events |
|----------|--------|
| Mouse | `click`, `dblclick`, `mousedown`, `mouseup`, `mousemove`, `mouseover`, `mouseout` |
| Keyboard | `keydown`, `keyup`, `keypress` |
| Form | `submit`, `change`, `input`, `focus`, `blur` |
| Document | `DOMContentLoaded`, `load`, `resize`, `scroll` |
| Touch | `touchstart`, `touchmove`, `touchend` |

---

## Chapter 7: Traversing the DOM

### 7.1 DOM Traversal

```html
<!DOCTYPE html>
<html>
<head>
    <title>DOM Traversal</title>
    <style>
        .tree {
            font-family: monospace;
            margin: 20px;
        }
        .node {
            margin-left: 20px;
            padding: 5px;
            border-left: 2px solid #ccc;
        }
        .highlight {
            background-color: yellow;
        }
        .current {
            background-color: lightblue;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h2>DOM Traversal Demo</h2>
    
    <div id="traversal-demo" class="tree">
        <div id="root">
            <p>Paragraph 1</p>
            <div>
                <p>Paragraph 2</p>
                <ul>
                    <li>Item 1</li>
                    <li>Item 2</li>
                    <li>Item 3</li>
                </ul>
            </div>
            <p>Paragraph 3</p>
        </div>
    </div>
    
    <div id="traversal-controls">
        <button onclick="traverseChildren()">Show Children</button>
        <button onclick="traverseParent()">Show Parent</button>
        <button onclick="traverseSiblings()">Show Siblings</button>
        <button onclick="findClosest()">Find Closest</button>
        <button onclick="traverseAll()">Traverse All</button>
    </div>
    
    <div id="traversal-output"></div>

    <script>
        const root = document.getElementById('root');
        const output = document.getElementById('traversal-output');
        let currentNode = root;
        
        function highlightNode(node) {
            document.querySelectorAll('.highlight, .current').forEach(el => {
                el.classList.remove('highlight', 'current');
            });
            if (node) {
                node.classList.add('current');
            }
        }
        
        function traverseChildren() {
            if (currentNode) {
                output.innerHTML = '<h3>Children:</h3>';
                const children = currentNode.children;
                
                if (children.length === 0) {
                    output.innerHTML += '<p>No children</p>';
                } else {
                    Array.from(children).forEach((child, index) => {
                        output.innerHTML += `<p>${index}: ${child.tagName} - ${child.textContent.substring(0, 20)}</p>`;
                    });
                }
                
                if (children.length > 0) {
                    currentNode = children[0];
                    highlightNode(currentNode);
                }
            }
        }
        
        function traverseParent() {
            if (currentNode && currentNode.parentElement) {
                currentNode = currentNode.parentElement;
                highlightNode(currentNode);
                output.innerHTML = `<p>Moved to parent: ${currentNode.tagName}</p>`;
            } else {
                output.innerHTML = '<p>No parent element</p>';
            }
        }
        
        function traverseSiblings() {
            if (currentNode) {
                output.innerHTML = '<h3>Siblings:</h3>';
                
                const parent = currentNode.parentElement;
                if (parent) {
                    const siblings = Array.from(parent.children);
                    const currentIndex = siblings.indexOf(currentNode);
                    
                    siblings.forEach((sibling, index) => {
                        const relation = index === currentIndex ? '(current)' :
                                        index < currentIndex ? '(previous)' : '(next)';
                        output.innerHTML += `<p>${index}: ${sibling.tagName} ${relation}</p>`;
                    });
                    
                    // Navigate to next sibling if exists
                    if (currentIndex < siblings.length - 1) {
                        currentNode = siblings[currentIndex + 1];
                        highlightNode(currentNode);
                    }
                }
            }
        }
        
        function findClosest() {
            const clicked = document.querySelector('li');
            if (clicked) {
                const closest = clicked.closest('div');
                output.innerHTML = `<p>Closest div to first li: ${closest ? closest.tagName : 'none'}</p>`;
                if (closest) highlightNode(closest);
            }
        }
        
        function traverseAll() {
            output.innerHTML = '<h3>Full Tree Traversal:</h3>';
            
            function traverse(node, depth = 0) {
                if (!node) return;
                
                const indent = '  '.repeat(depth);
                output.innerHTML += `<p>${indent}${node.tagName || '#text'}</p>`;
                
                // Traverse child nodes
                const children = node.children;
                Array.from(children).forEach(child => traverse(child, depth + 1));
            }
            
            traverse(document.getElementById('traversal-demo'));
        }
    </script>
</body>
</html>
```

### 7.2 Traversal Properties

| Property | Description |
|----------|-------------|
| `parentNode` | Parent node |
| `parentElement` | Parent element |
| `childNodes` | All child nodes |
| `children` | Child elements |
| `firstChild` | First child node |
| `lastChild` | Last child node |
| `firstElementChild` | First child element |
| `lastElementChild` | Last child element |
| `previousSibling` | Previous sibling node |
| `nextSibling` | Next sibling node |
| `previousElementSibling` | Previous sibling element |
| `nextElementSibling` | Next sibling element |

---

## Chapter 8: Styling Elements

### 8.1 Dynamic Styling

```html
<!DOCTYPE html>
<html>
<head>
    <title>Dynamic Styling</title>
    <style>
        .style-box {
            width: 200px;
            height: 200px;
            margin: 20px;
            border: 1px solid black;
            transition: all 0.3s ease;
        }
        .custom-class {
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
            transform: scale(1.05);
        }
    </style>
</head>
<body>
    <h2>Dynamic Styling Demo</h2>
    
    <div id="style-demo">
        <div id="style-box" class="style-box"></div>
    </div>
    
    <div id="style-controls">
        <h3>Style Controls:</h3>
        
        <button onclick="changeColor()">Change Color</button>
        <button onclick="toggleClass()">Toggle Class</button>
        <button onclick="setMultipleStyles()">Multiple Styles</button>
        <button onclick="getComputedStyles()">Get Computed Styles</button>
        <button onclick="resetStyles()">Reset</button>
        
        <br><br>
        
        <label>Background Color: 
            <input type="color" id="color-picker" onchange="setColor(this.value)">
        </label>
        
        <br><br>
        
        <label>Width: 
            <input type="range" id="width-slider" min="100" max="300" value="200" onchange="setWidth(this.value)">
        </label>
    </div>
    
    <div id="style-output"></div>

    <script>
        const box = document.getElementById('style-box');
        const output = document.getElementById('style-output');
        
        function changeColor() {
            const colors = ['red', 'blue', 'green', 'yellow', 'purple', 'orange'];
            const randomColor = colors[Math.floor(Math.random() * colors.length)];
            box.style.backgroundColor = randomColor;
            output.innerHTML = `<p>Background color set to: ${randomColor}</p>`;
        }
        
        function setColor(color) {
            box.style.backgroundColor = color;
            output.innerHTML = `<p>Background color set to: ${color}</p>`;
        }
        
        function setWidth(width) {
            box.style.width = width + 'px';
            output.innerHTML = `<p>Width set to: ${width}px</p>`;
        }
        
        function toggleClass() {
            box.classList.toggle('custom-class');
            const hasClass = box.classList.contains('custom-class');
            output.innerHTML = `<p>Custom class ${hasClass ? 'added' : 'removed'}</p>`;
        }
        
        function setMultipleStyles() {
            Object.assign(box.style, {
                backgroundColor: 'lightblue',
                border: '3px solid navy',
                borderRadius: '15px',
                transform: 'rotate(5deg)',
                boxShadow: '0 8px 16px rgba(0,0,0,0.3)'
            });
            output.innerHTML = '<p>Multiple styles applied</p>';
        }
        
        function getComputedStyles() {
            const styles = window.getComputedStyle(box);
            output.innerHTML = '<h3>Computed Styles:</h3>';
            const properties = [
                'backgroundColor', 'width', 'height', 'border', 'borderRadius', 'opacity'
            ];
            
            properties.forEach(prop => {
                output.innerHTML += `<p>${prop}: ${styles[prop]}</p>`;
            });
        }
        
        function resetStyles() {
            box.style = {};
            box.classList.remove('custom-class');
            document.getElementById('width-slider').value = 200;
            output.innerHTML = '<p>Styles reset to default</p>';
        }
    </script>
</body>
</html>
```

### 8.2 Styling Methods

| Method/Property | Description |
|-----------------|-------------|
| `style.property` | Set inline style |
| `classList.add()` | Add CSS class |
| `classList.remove()` | Remove CSS class |
| `classList.toggle()` | Toggle CSS class |
| `className` | Get/set all classes |
| `window.getComputedStyle()` | Get computed styles |

---

## Chapter 9: Advanced DOM Techniques

### 9.1 Event Delegation and Performance

```html
<!DOCTYPE html>
<html>
<head>
    <title>Advanced DOM Techniques</title>
    <style>
        .list-item {
            padding: 10px;
            margin: 5px;
            background-color: #f0f0f0;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .list-item:hover {
            background-color: #e0e0e0;
        }
        .list-item.selected {
            background-color: #007bff;
            color: white;
        }
        .performance-metrics {
            font-family: monospace;
            padding: 10px;
            background-color: #f5f5f5;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <h2>Advanced DOM Techniques</h2>
    
    <h3>Event Delegation Demo</h3>
    <div id="list-container">
        <button onclick="addItems()">Add 1000 Items</button>
        <button onclick="clearItems()">Clear Items</button>
        <button onclick="measurePerformance()">Measure Performance</button>
        <div id="item-list"></div>
    </div>
    
    <div id="output" class="performance-metrics"></div>

    <script>
        const container = document.getElementById('item-list');
        const output = document.getElementById('output');
        
        // Event Delegation - Single event listener for all items
        container.addEventListener('click', (e) => {
            const target = e.target;
            
            // Check if clicked element is a list item
            if (target.classList.contains('list-item')) {
                // Remove selected class from all items
                document.querySelectorAll('.list-item').forEach(item => {
                    item.classList.remove('selected');
                });
                
                // Add selected class to clicked item
                target.classList.add('selected');
                
                // Get data attribute
                const index = target.dataset.index;
                output.innerHTML = `<p>Selected item #${index}: ${target.textContent}</p>`;
            }
        });
        
        function addItems() {
            const fragment = document.createDocumentFragment();
            const startTime = performance.now();
            
            for (let i = 0; i < 1000; i++) {
                const item = document.createElement('div');
                item.className = 'list-item';
                item.dataset.index = i;
                item.textContent = `Item ${i + 1}`;
                fragment.appendChild(item);
            }
            
            container.appendChild(fragment);
            
            const endTime = performance.now();
            output.innerHTML = `<p>Added 1000 items in ${(endTime - startTime).toFixed(2)}ms</p>`;
        }
        
        function clearItems() {
            const startTime = performance.now();
            
            // Efficient clearing
            while (container.firstChild) {
                container.removeChild(container.firstChild);
            }
            
            const endTime = performance.now();
            output.innerHTML = `<p>Cleared items in ${(endTime - startTime).toFixed(2)}ms</p>`;
        }
        
        function measurePerformance() {
            const metrics = {
                totalElements: document.getElementsByTagName('*').length,
                listItems: document.querySelectorAll('.list-item').length,
                containerChildren: container.children.length
            };
            
            output.innerHTML = '<h3>Performance Metrics:</h3>';
            for (let [key, value] of Object.entries(metrics)) {
                output.innerHTML += `<p>${key}: ${value}</p>`;
            }
        }
        
        // Debounce function for performance
        function debounce(func, wait) {
            let timeout;
            return function executedFunction(...args) {
                const later = () => {
                    clearTimeout(timeout);
                    func(...args);
                };
                clearTimeout(timeout);
                timeout = setTimeout(later, wait);
            };
        }
        
        // Throttle function for performance
        function throttle(func, limit) {
            let inThrottle;
            return function(...args) {
                if (!inThrottle) {
                    func.apply(this, args);
                    inThrottle = true;
                    setTimeout(() => inThrottle = false, limit);
                }
            };
        }
        
        // Virtual Scrolling Demo (concept)
        class VirtualScroller {
            constructor(container, itemHeight, totalItems) {
                this.container = container;
                this.itemHeight = itemHeight;
                this.totalItems = totalItems;
                this.visibleItems = Math.ceil(container.clientHeight / itemHeight);
                this.scrollTop = 0;
                
                container.style.overflowY = 'auto';
                container.style.height = '400px';
                
                container.addEventListener('scroll', () => {
                    this.scrollTop = container.scrollTop;
                    this.render();
                });
                
                this.render();
            }
            
            render() {
                const startIndex = Math.floor(this.scrollTop / this.itemHeight);
                const endIndex = Math.min(startIndex + this.visibleItems + 1, this.totalItems);
                
                // Only render visible items
                const fragment = document.createDocumentFragment();
                for (let i = startIndex; i < endIndex; i++) {
                    const item = document.createElement('div');
                    item.style.height = `${this.itemHeight}px`;
                    item.textContent = `Virtual Item ${i + 1}`;
                    item.style.position = 'absolute';
                    item.style.top = `${i * this.itemHeight}px`;
                    fragment.appendChild(item);
                }
                
                this.container.innerHTML = '';
                this.container.appendChild(fragment);
                this.container.style.height = `${this.totalItems * this.itemHeight}px`;
            }
        }
    </script>
</body>
</html>
```

---

## Chapter 10: Practice Projects

### Project 1: Interactive Todo List

```html
<!DOCTYPE html>
<html>
<head>
    <title>Todo List App</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 600px;
            margin: 0 auto;
            background: white;
            border-radius: 10px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }
        
        h1 {
            color: #333;
            margin-bottom: 20px;
            text-align: center;
        }
        
        .input-section {
            display: flex;
            margin-bottom: 20px;
        }
        
        #todo-input {
            flex: 1;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 5px 0 0 5px;
            font-size: 16px;
            outline: none;
            transition: border-color 0.3s;
        }
        
        #todo-input:focus {
            border-color: #667eea;
        }
        
        #add-btn {
            padding: 12px 24px;
            background: #667eea;
            color: white;
            border: none;
            border-radius: 0 5px 5px 0;
            cursor: pointer;
            font-size: 16px;
            transition: background 0.3s;
        }
        
        #add-btn:hover {
            background: #5a67d8;
        }
        
        .filters {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }
        
        .filter-btn {
            flex: 1;
            padding: 8px;
            background: #f0f0f0;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .filter-btn.active {
            background: #667eea;
            color: white;
        }
        
        #todo-list {
            list-style: none;
            margin-bottom: 20px;
        }
        
        .todo-item {
            display: flex;
            align-items: center;
            padding: 12px;
            background: #f9f9f9;
            border-radius: 5px;
            margin-bottom: 10px;
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
        
        .todo-item.completed {
            background: #e8f5e9;
        }
        
        .todo-checkbox {
            margin-right: 10px;
            width: 20px;
            height: 20px;
            cursor: pointer;
        }
        
        .todo-text {
            flex: 1;
            font-size: 16px;
        }
        
        .todo-item.completed .todo-text {
            text-decoration: line-through;
            color: #888;
        }
        
        .todo-actions {
            display: flex;
            gap: 5px;
        }
        
        .edit-btn, .delete-btn {
            padding: 5px 10px;
            border: none;
            border-radius: 3px;
            cursor: pointer;
            transition: background 0.3s;
        }
        
        .edit-btn {
            background: #ffc107;
            color: #333;
        }
        
        .edit-btn:hover {
            background: #e0a800;
        }
        
        .delete-btn {
            background: #dc3545;
            color: white;
        }
        
        .delete-btn:hover {
            background: #c82333;
        }
        
        .stats {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding-top: 20px;
            border-top: 2px solid #f0f0f0;
            color: #666;
        }
        
        #clear-completed {
            padding: 5px 10px;
            background: #f0f0f0;
            border: none;
            border-radius: 3px;
            cursor: pointer;
            transition: background 0.3s;
        }
        
        #clear-completed:hover {
            background: #e0e0e0;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📝 Todo List App</h1>
        
        <div class="input-section">
            <input type="text" id="todo-input" placeholder="Enter a new task...">
            <button id="add-btn">Add Task</button>
        </div>
        
        <div class="filters">
            <button class="filter-btn active" data-filter="all">All</button>
            <button class="filter-btn" data-filter="active">Active</button>
            <button class="filter-btn" data-filter="completed">Completed</button>
        </div>
        
        <ul id="todo-list"></ul>
        
        <div class="stats">
            <span id="items-left">0 items left</span>
            <button id="clear-completed">Clear Completed</button>
        </div>
    </div>

    <script>
        // Todo App Class
        class TodoApp {
            constructor() {
                this.todos = this.loadTodos();
                this.currentFilter = 'all';
                this.initElements();
                this.bindEvents();
                this.render();
            }
            
            initElements() {
                this.todoInput = document.getElementById('todo-input');
                this.addBtn = document.getElementById('add-btn');
                this.todoList = document.getElementById('todo-list');
                this.filterBtns = document.querySelectorAll('.filter-btn');
                this.itemsLeftSpan = document.getElementById('items-left');
                this.clearCompletedBtn = document.getElementById('clear-completed');
            }
            
            bindEvents() {
                this.addBtn.addEventListener('click', () => this.addTodo());
                this.todoInput.addEventListener('keypress', (e) => {
                    if (e.key === 'Enter') this.addTodo();
                });
                
                this.filterBtns.forEach(btn => {
                    btn.addEventListener('click', (e) => {
                        this.filterBtns.forEach(b => b.classList.remove('active'));
                        btn.classList.add('active');
                        this.currentFilter = btn.dataset.filter;
                        this.render();
                    });
                });
                
                this.clearCompletedBtn.addEventListener('click', () => {
                    this.todos = this.todos.filter(todo => !todo.completed);
                    this.saveTodos();
                    this.render();
                });
            }
            
            addTodo() {
                const text = this.todoInput.value.trim();
                if (text) {
                    const todo = {
                        id: Date.now(),
                        text: text,
                        completed: false,
                        createdAt: new Date().toISOString()
                    };
                    
                    this.todos.push(todo);
                    this.saveTodos();
                    this.todoInput.value = '';
                    this.render();
                }
            }
            
            toggleTodo(id) {
                this.todos = this.todos.map(todo => 
                    todo.id === id ? { ...todo, completed: !todo.completed } : todo
                );
                this.saveTodos();
                this.render();
            }
            
            editTodo(id, newText) {
                if (newText.trim()) {
                    this.todos = this.todos.map(todo => 
                        todo.id === id ? { ...todo, text: newText.trim() } : todo
                    );
                    this.saveTodos();
                    this.render();
                }
            }
            
            deleteTodo(id) {
                if (confirm('Are you sure you want to delete this task?')) {
                    this.todos = this.todos.filter(todo => todo.id !== id);
                    this.saveTodos();
                    this.render();
                }
            }
            
            getFilteredTodos() {
                switch(this.currentFilter) {
                    case 'active':
                        return this.todos.filter(todo => !todo.completed);
                    case 'completed':
                        return this.todos.filter(todo => todo.completed);
                    default:
                        return this.todos;
                }
            }
            
            saveTodos() {
                localStorage.setItem('todos', JSON.stringify(this.todos));
            }
            
            loadTodos() {
                const saved = localStorage.getItem('todos');
                return saved ? JSON.parse(saved) : [];
            }
            
            render() {
                const filteredTodos = this.getFilteredTodos();
                const activeCount = this.todos.filter(todo => !todo.completed).length;
                
                this.itemsLeftSpan.textContent = `${activeCount} item${activeCount !== 1 ? 's' : ''} left`;
                
                if (filteredTodos.length === 0) {
                    this.todoList.innerHTML = '<li style="text-align: center; padding: 20px; color: #888;">No tasks to display</li>';
                    return;
                }
                
                this.todoList.innerHTML = '';
                
                filteredTodos.forEach(todo => {
                    const li = document.createElement('li');
                    li.className = `todo-item ${todo.completed ? 'completed' : ''}`;
                    li.dataset.id = todo.id;
                    
                    const checkbox = document.createElement('input');
                    checkbox.type = 'checkbox';
                    checkbox.className = 'todo-checkbox';
                    checkbox.checked = todo.completed;
                    checkbox.addEventListener('change', () => this.toggleTodo(todo.id));
                    
                    const textSpan = document.createElement('span');
                    textSpan.className = 'todo-text';
                    textSpan.textContent = todo.text;
                    
                    const actionsDiv = document.createElement('div');
                    actionsDiv.className = 'todo-actions';
                    
                    const editBtn = document.createElement('button');
                    editBtn.className = 'edit-btn';
                    editBtn.textContent = 'Edit';
                    editBtn.addEventListener('click', () => {
                        const newText = prompt('Edit task:', todo.text);
                        if (newText !== null) {
                            this.editTodo(todo.id, newText);
                        }
                    });
                    
                    const deleteBtn = document.createElement('button');
                    deleteBtn.className = 'delete-btn';
                    deleteBtn.textContent = 'Delete';
                    deleteBtn.addEventListener('click', () => this.deleteTodo(todo.id));
                    
                    actionsDiv.appendChild(editBtn);
                    actionsDiv.appendChild(deleteBtn);
                    
                    li.appendChild(checkbox);
                    li.appendChild(textSpan);
                    li.appendChild(actionsDiv);
                    
                    this.todoList.appendChild(li);
                });
            }
        }
        
        // Initialize the app
        const app = new TodoApp();
    </script>
</body>
</html>
```

---

## Summary

### Key Concepts Covered:

1. **DOM Selection**: Methods to find elements in the document
2. **DOM Manipulation**: Changing content, attributes, and structure
3. **Event Handling**: Responding to user interactions
4. **DOM Traversal**: Navigating the document tree
5. **Styling**: Dynamic CSS manipulation
6. **Performance**: Best practices for efficient DOM operations
7. **Projects**: Real-world applications combining all concepts

### Best Practices:

- ✅ Use `querySelector`/`querySelectorAll` for complex selections
- ✅ Use `textContent` instead of `innerHTML` for text (security)
- ✅ Use event delegation for dynamic elements
- ✅ Use document fragments for multiple insertions
- ✅ Cache DOM queries when reusing elements
- ✅ Remove event listeners when no longer needed
- ✅ Use classList for class manipulation
- ✅ Consider performance with large DOM manipulations

### Common Pitfalls to Avoid:

- ❌ Manipulating DOM inside loops (causes reflows)
- ❌ Using `innerHTML` with user input (XSS risk)
- ❌ Forgetting to remove event listeners (memory leaks)
- ❌ Excessive DOM queries (cache when possible)
- ❌ Not handling asynchronous operations properly

---

## Resources for Further Learning

1. **MDN Web Docs**: [DOM Documentation](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
2. **JavaScript.info**: [DOM Tutorial](https://javascript.info/document)
3. **W3Schools**: [DOM Tutorial](https://www.w3schools.com/js/js_htmldom.asp)
4. **Eloquent JavaScript**: [Chapter on DOM](https://eloquentjavascript.net/14_dom.html)

---

