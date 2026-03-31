# 🌳 JavaScript DOM & Events

---

## 🌐 What is the DOM (Document Object Model)?

The **DOM** is the browser's runtime (while the page is loaded) in-memory (temporarily stored in RAM) representation of parsed HTML markup, accessible and modifiable via JavaScript APIs.

**Fundamental distinction:** HTML source files are static; the DOM is a dynamic, mutable (changeable) object tree instantiated (created) by the browser.

- **HTML file** – Static markup file on disk (`.html`)
- **DOM** – Runtime object tree in browser memory

**Instantiation flow:**

1. Browser fetches and parses (reads and breaks down) HTML markup
2. Parser constructs DOM tree corresponding to element hierarchy
3. Browser renders tree to viewport
4. JavaScript can now query and mutate (change) DOM nodes at runtime

**Relevance:** JavaScript operates exclusively on DOM nodes, not the source HTML file. All DOM modifications are transient (temporary) unless persisted (saved) server-side.

---

### 🆚 HTML vs DOM

| HTML                            | DOM                                     |
| ------------------------------- | --------------------------------------- |
| Static file (`.html`)           | Live in-memory structure                |
| What you write                  | What the browser created from your HTML |
| Never changes unless you reload | Changes every time JavaScript runs      |
| Text-based                      | JavaScript object tree                  |

**Example:**

```html
<!-- HTML file -->
<div id="message">Hello</div>
```

```js
// JavaScript manipulation (changes the DOM, not the HTML file)
const div = document.getElementById("message");
div.textContent = "Goodbye"; // DOM changed, but HTML file unchanged
```

Close the laptop and reopen → HTML says "Hello" (unchanged)
Reload the page → DOM gets recreated from HTML file (says "Hello")

---

## 🌳 The DOM is a Tree Structure

Document Object Model = Objects arranged in a **tree structure**

```
document (root)
  ├── html
  │   ├── head
  │   │   ├── title
  │   │   └── link (stylesheet)
  │   └── body
  │       ├── header
  │       ├── main
  │       │   ├── h1
  │       │   ├── p
  │       │   └── button
  │       └── footer
```

Each item is called a **node**. Nodes can be:

- **Parent:** Contains other nodes
- **Child:** Contained by another node
- **Sibling:** Have the same parent

**Why this matters?** To find elements in the DOM, you navigate this tree.

---

### 📦 `Array.isArray()` – Type Checking

- This method validates whether a value is an array instance, distinct from array-like objects
- Returns boolean indicating array status

#### ✅ Example:

```js
Array.isArray([1, 2, 3]); // true
Array.isArray("hello"); // false
Array.isArray({}); // false
```

**Why?** Arrays are technically objects in JavaScript. `Array.isArray()` specifically checks if something is an array.

- DOM = **Document Object Model**
- It is the browser's runtime representation of parsed HTML markup.

---

### 🆚 HTML vs DOM

| HTML                              | DOM                                   |
| --------------------------------- | ------------------------------------- |
| Static markup file                | Runtime object tree in browser memory |
| Source code on disk               | Parsed and instantiated structure     |
| Immutable (without server reload) | Mutable via JavaScript APIs           |
| Text-based                        | Interconnected node objects           |

---

### 🔍 DOM Navigation & Selection

**Getting elements:**

```js
// Get by ID (most specific)
const elem = document.getElementById("myId");

// Get by class (returns multiple)
const elems = document.getElementsByClassName("myClass");

// Modern way (more flexible)
const elem = document.querySelector("#myId"); // ID
const elems = document.querySelectorAll(".myClass"); // Class
const elem = document.querySelector("div"); // Tag
const elem = document.querySelector("[data-id='123']"); // Attribute

// Traverse the tree
const parent = element.parentNode;
const children = element.children; // Only direct children
const firstChild = element.firstChild;
const siblings = element.previousSibling / nextSibling;
```

**Best practice:** Use `querySelector`/`querySelectorAll` - they're modern and flexible.

#### Get children of an element:

```js
const parent = document.getElementById("container");
const children = parent.children; // HTMLCollection (array-like)
children.forEach((child) => {
  console.log(child);
});
```

#### Remove an element:

```js
// Old way (verbose)
const item = document.getElementById("to-remove");
item.parentNode.removeChild(item);

// Modern way (simpler)
item.remove();
```

---

### 🛠️ DOM Manipulation – Changing the Page

```js
// Change text content
element.textContent = "New text";

// Change HTML (be careful with user input!)
element.innerHTML = "<strong>Bold</strong>";

// Update attributes
element.setAttribute("src", "image.jpg");
element.id = "newId";

// Add/remove classes
element.classList.add("active");
element.classList.remove("inactive");
element.classList.toggle("highlight"); // Toggle on/off

// Change styles
element.style.color = "red";
element.style.backgroundColor = "blue";
```

---

### 🔁 Array-like Objects Workaround

Many DOM collections (`children`, `NodeList`) look like arrays but aren't:

```js
// ❌ This doesn't work (not a real array)
const children = parent.children;
children.map(child => ...);  // Error! map is not a function

// ✅ Convert to real array
const childrenArray = Array.from(parent.children);
childrenArray.forEach(child => {
  console.log(child);
});

// ✅ Or use spread operator
const childrenArray = [...parent.children];
```

---

## 🧠 What is a JavaScript Event?

**Events** represent user interactions and browser lifecycle changes that an application can subscribe to (listen for) and handle.

**Event types:**

- `click` – Mouse button clicked on element
- `submit` – Form submission triggered
- `input` / `change` – Form input value changed
- `keydown` / `keyup` – Keyboard key pressed/released
- `mouseover` / `mouseout` – Mouse enters/leaves element boundary
- `load` – Document fully parsed and resources loaded
- `scroll` – Viewport scrolled

#### Event Propagation:

```text
User Interaction (click element)
        ↓
Browser Event System fires event
        ↓
Executes attached event listeners
        ↓
Listeners invoke callback functions
        ↓
Application logic executes
        ↓
DOM/State modifications occur
```

#### Listener Registration:

```js
element.addEventListener("click", (event) => {
  // Callback execution triggered on event emission
});
```

**Event handler** = Callback function registered as listener. Executes asynchronously (independently, when event occurs) upon event occurrence.

---

### 📋 Common Event Patterns

#### Click Listener:

```js
button.addEventListener("click", (event) => {
  console.log("Clicked!");
  console.log(event.target); // The element that was clicked
});
```

#### Form submission:

```js
const form = document.querySelector("form");

form.addEventListener("submit", (event) => {
  event.preventDefault(); // Stop page reload

  const input = form.querySelector("input");
  console.log("User submitted:", input.value);
});
```

#### Input changes (real-time):

```js
const input = document.querySelector("input");

input.addEventListener("input", (event) => {
  console.log("User typed:", event.target.value);
});
```

#### Event object (event parameter):

```js
element.addEventListener("click", (event) => {
  event.target; // The element clicked
  event.type; // "click", "submit", etc.
  event.preventDefault(); // Stop default behavior
  event.stopPropagation(); // Stop event bubbling
});
```

---

## 🌐 Client vs Server

**Understanding the architecture:**

**Client** = Browser environment with JavaScript runtime
**Server** = Backend process managing persistent state and business logic

### Client-Side (Browser - Your Domain):

- ✅ Change DOM, update UI instantly
- ✅ Handle events (clicks, typing, etc.)
- ✅ No page reload needed for changes
- ✅ **Limitation:** Can't access server databases directly

```js
// Client can do this (instantly)
document.getElementById("message").textContent = "You won!";
```

### Server-Side (Web Server - Their Domain):

- ✅ Store persistent data in databases
- ✅ Run business logic safely
- ✅ Handle authentication & security
- ✅ Process requests from multiple clients
- ❌ Can't directly manipulate user's DOM

### Communication Flow:

```text
Browser (Client)           Server
    |                        |
    |-- HTTP Request ------->|
    |  (e.g., GET /friends)  |
    |                        |
    |   Query Database       |
    |<------ Response --------|
    |   (JSON data)          |
    |                        |
    Update DOM
    User sees results
```

**Key point:** After the page loads, the server doesn't know what the client does with JavaScript. They communicate by making explicit requests.

#### Example:

```js
// Client-side: Handle button click
button.addEventListener("click", async () => {
  // 1. Send request to server
  const response = await fetch("/api/friends");

  // 2. Server processes
  // (Client doesn't know what's happening)

  // 3. Server sends back data
  const friends = await response.json();

  // 4. Client updates DOM with the data
  document.getElementById("friendsList").innerHTML = friends
    .map((f) => `<div>${f.name}</div>`)
    .join("");
});
```

**Timeline:**

1. User clicks button → Client-side function runs
2. Client sends HTTP request to server
3. Server fetches from database
4. Server sends response back
5. Client receives response → Updates DOM
6. **User sees the result**

**Important distinction:**

- Server has no control over what you do after it sends the response
- Everything you do with JavaScript (without making another request) is **temporary** and only affects that user's browser
- If user refreshes page → DOM goes back to original state

---

## 📚 Complete Real-World Example – Todo List

```html
<!-- HTML -->
<input id="todoInput" type="text" placeholder="Add a todo..." />
<button id="addBtn">Add</button>
<ul id="todoList"></ul>
```

```js
// JavaScript (Client-side)
const input = document.getElementById("todoInput");
const addBtn = document.getElementById("addBtn");
const list = document.getElementById("todoList");

addBtn.addEventListener("click", async () => {
  // 1. Get the text from input (DOM manipulation)
  const text = input.value;

  // 2. Send to server (request)
  const response = await fetch("/api/todos", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ text }),
  });

  // 3. Get response from server
  const newTodo = await response.json();

  // 4. Update DOM (client-side rendering)
  const li = document.createElement("li");
  li.textContent = newTodo.text;
  li.id = `todo-${newTodo.id}`;
  list.appendChild(li);

  // 5. Clear input
  input.value = "";
});
```

**What's happening:**

1. User types something and clicks "Add"
2. Client reads input value from DOM
3. Client sends request to server
4. Server creates todo in database
5. Server sends back the created todo
6. Client adds it to the DOM (visible immediately)
7. User sees the new todo without page reload

---

## ✅ Best Practices with the DOM

1. **Cache DOM selections** – Don't query the same element repeatedly

   ```js
   // ❌ Bad
   for (let i = 0; i < 100; i++) {
     document.getElementById("counter").textContent = i;
   }

   // ✅ Good
   const counter = document.getElementById("counter");
   for (let i = 0; i < 100; i++) {
     counter.textContent = i;
   }
   ```

2. **Use event delegation** – Attach listeners to parent, not children

   ```js
   // ✅ Good for many items
   document.getElementById("list").addEventListener("click", (e) => {
     if (e.target.matches(".item")) {
       console.log("Clicked item:", e.target);
     }
   });
   ```

3. **Clean up event listeners** – Prevent memory leaks (especially important in React)

   ```js
   button.addEventListener("click", handler);
   // Later, when needed:
   button.removeEventListener("click", handler);
   ```

4. **Prefer `textContent` over `innerHTML`** – Safer from XSS attacks

   ```js
   // ❌ Unsafe (user input could have malicious HTML)
   element.innerHTML = userInput;

   // ✅ Safe (just text, no HTML parsing)
   element.textContent = userInput;
   ```

---

## 🔗 DOM Quick Reference

| Task               | Code                                         |
| ------------------ | -------------------------------------------- |
| Select element     | `document.querySelector("#id")`              |
| Select all         | `document.querySelectorAll(".class")`        |
| Change text        | `element.textContent = "new text"`           |
| Add element        | `parent.appendChild(newElement)`             |
| Remove element     | `element.remove()`                           |
| Add class          | `element.classList.add("active")`            |
| Add event listener | `element.addEventListener("click", handler)` |
| Get input value    | `input.value`                                |
| Change CSS         | `element.style.color = "red"`                |

---

## 🎯 Common Mistakes

1. **Forgetting `.addEventListener()`** - Event listeners need to be attached
2. **Using `innerHTML` with user input** - XSS vulnerability
3. **Querying DOM in loops** - Cache the selection first
4. **Forgetting to prevent default behavior** - Use `event.preventDefault()`
5. **Not handling errors** - Always wrap async code in try/catch
