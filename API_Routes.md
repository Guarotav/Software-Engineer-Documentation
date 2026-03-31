## 🌐 API Routes, Promises, Async/Await, and Data Fetching

---

## 📚 What is an API Route?

An **API Route** is a URL endpoint on your server that handles requests and sends back data.

**How it works:**

1. Client (browser) sends a request to an endpoint (e.g., `/puppies`)
2. Server receives the request with an HTTP method (GET, POST, etc.)
3. Server processes the request (fetch from database, validate, etc.)
4. Server sends back a response (data, success/error status)

Think of it like ordering at a restaurant:

- 📞 You call (send a request)
- 👨‍🍳 Chef processes your order (server logic)
- 🍽️ Food arrives (response with data)

---

### 🐶 RESTful API Routes (Puppies Example)

| HTTP Method | Endpoint     | What it does                      |
| ----------- | ------------ | --------------------------------- |
| `GET`       | `/puppies`   | Get **all** puppies               |
| `GET`       | `/puppies/2` | Get **puppy with ID = 2**         |
| `POST`      | `/puppies`   | Add a **new puppy**               |
| `DELETE`    | `/puppies/4` | Delete **puppy with ID = 4**      |
| `PATCH`     | `/puppies/7` | **Update part** of puppy 7’s info |
| `PUT`       | `/puppies/9` | **Replace all** data for puppy 9  |

---

### 🧠 REST Methods Explained

#### 🔍 **GET** – Read Data (Safe & Idempotent)

- **What:** Fetch data from the server
- **Why:** Display information to users
- **Idempotent:** Calling it multiple times returns the same result
- **Example use cases:**
  ```js
  GET / friends; // Get all your friends
  GET / friends / 123; // Get details of friend #123
  GET / notifications; // Get all notifications
  GET / profile; // Get your profile
  ```

#### ✏️ **POST** – Create New Data

- **What:** Send data to server to create a new resource
- **Why:** Add new items (posts, users, comments, etc.)
- **Example use cases:**
  ```js
  POST /puppies { name: "Buddy", breed: "Golden" }  // Create new puppy
  POST /friends/123/accept  // Accept friend request from user 123
  POST /messages { text: "Hello!", recipientId: 456 }  // Send message
  ```

#### ✏️ **PUT** – Replace Entire Resource

- **What:** Replace ALL data for a specific resource
- **When to use:** Less common; use when you're replacing everything
- **Example:**
  ```js
  PUT /puppies/9 { name: "Max", breed: "Labrador", age: 5 }
  // Replaces EVERYTHING for puppy #9
  ```

#### 🔄 **PATCH** – Partial Update

- **What:** Update ONLY the fields you send (the rest stays the same)
- **When to use:** More common than PUT
- **Example:**
  ```js
  PATCH /puppies/7 { name: "NewName" }
  // Only updates the name, other fields unchanged
  ```

#### 🗑️ **DELETE** – Remove Data

- **What:** Delete a resource
- **Why:** Remove items permanently
- **Example use cases:**
  ```js
  DELETE / puppies / 4; // Delete puppy #4
  DELETE / friends / 123; // Remove friend #123
  DELETE / messages / 999; // Delete message #999
  ```

---

### 📊 Why REST Matters

REST provides a **consistent, predictable way** to communicate with servers:

- ✅ Predictable URLs (`/resource`, `/resource/:id`)
- ✅ Standard HTTP methods (GET, POST, etc.)
- ✅ Works across different programming languages
- ✅ Easy for other developers to understand your API

---

### 🔁 Promises – Handling Async Operations

> "A Promise represents the eventual completion (or failure) of an asynchronous operation and its resulting value." – **MDN**

**Why Promises?** When you fetch data from a server, it takes TIME. You can't wait for it to complete before running other code. Promises let you say "When this finishes, do this."

#### Promise States:

1. **⏳ Pending:** Request is in progress
2. **✅ Fulfilled:** Got the data successfully
3. **❌ Rejected:** Request failed (error)

```
Pending ──> Fulfilled (success)
        \
         └─> Rejected (error)
```

**Important:** Once resolved (to fulfilled or rejected), a promise **cannot change** state again.

#### Basic Example:

```js
fetch("/puppies") // Returns a Promise
  .then((response) => response.json()) // When response arrives
  .then((data) => console.log(data)) // When data is parsed
  .catch((error) => console.error(error)); // If anything fails
```

**What's happening:**

1. `fetch()` starts the request and returns a Promise immediately
2. `.then()` waits for the response to arrive
3. `.catch()` catches any errors along the way

---

### ⏳ Async/Await – Modern Promise Handling

**What is it?** `async/await` is a cleaner way to handle Promises. It makes asynchronous code look and behave like synchronous code.

**Comparison:**

```js
// ❌ Promise chaining (hard to read)
fetch("/puppies")
  .then((res) => res.json())
  .then((data) => {
    console.log(data);
    return fetch("/users");
  })
  .then((res) => res.json())
  .catch((error) => console.error(error));

// ✅ Async/await (much cleaner!)
async function loadData() {
  try {
    const response = await fetch("/puppies");
    const data = await response.json();
    console.log(data);

    const userRes = await fetch("/users");
    const users = await userRes.json();
  } catch (error) {
    console.error(error);
  }
}
```

#### Rules of `await`:

- Can **only** be used inside an `async` function
- Code execution will **pause** on the `await` line until fulfilled
- Must use `try/catch` to handle errors (instead of `.catch()`)

#### Example:

```js
async function getPuppies() {
  const response = await fetch("/puppies"); // Wait for response
  const data = await response.json(); // Wait for JSON parsing
  return data;
}

// Call the async function
getPuppies().then((puppies) => console.log(puppies));
```

**Why use async/await?**

- ✅ Easier to read (looks like regular code)
- ✅ Easier to debug
- ✅ Better error handling with try/catch
- ✅ Easier to combine multiple requests sequentially

---

### 📡 Fetch – Native JavaScript HTTP Client

`fetch()` is the built-in JavaScript function to make HTTP requests (it's what browsers use by default).

#### How Fetch Works (2-Step Process):

1. **Step 1:** `fetch()` returns a **response** object (with status code, headers, etc.)
2. **Step 2:** Call `.json()` on the response to extract and parse the actual data

```js
fetch("/puppies")
  .then((res) => res.json()) // Step 2: parse response
  .then((data) => console.log(data)); // Step 3: use the data
```

**Why 2 steps?** The response object contains metadata like:

- Status code (200 = success, 404 = not found, etc.)
- Headers
- The body (raw data)

You must call `.json()` to extract and parse the body.

#### Examples:

```js
// GET request (default)
fetch("/puppies")
  .then((res) => res.json())
  .then((data) => console.log(data));

// POST request (create data)
fetch("/puppies", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Buddy", breed: "Golden" }),
})
  .then((res) => res.json())
  .then((data) => console.log("Created:", data));

// Error handling
fetch("/puppies")
  .then((res) => res.json())
  .catch((error) => console.error("Failed to fetch:", error));
```

#### Fetch vs other methods:

- ✅ **Pros:** Built-in (no install needed), works everywhere
- ❌ **Cons:** Requires 2-step parsing, no built-in timeout, basic features

---

### 📦 Axios – Modern HTTP Client (Recommended)

**What is it?** Axios is a popular JavaScript library for making HTTP requests. It's built on top of Fetch but **much easier to use**.

**Why use Axios instead of Fetch?**

- ✅ **One-step parsing:** Automatically converts JSON (no need for `.json()`)
- ✅ **Better defaults:** Automatically sets content type, handles errors better
- ✅ **Request/response interceptors:** Add auth headers automatically
- ✅ **Timeout support:** Built-in request timeouts
- ✅ **Cancellation:** Cancel requests easily
- ✅ **Cleaner API:** More intuitive for complex requests

#### Install:

```bash
npm install axios
```

#### Comparison:

```js
// ❌ Fetch (verbose)
fetch("/puppies")
  .then((res) => res.json()) // Extra step needed
  .then((data) => console.log(data));

// ✅ Axios (simple)
axios.get("/puppies").then((res) => console.log(res.data)); // Already parsed!
```

#### Axios Examples:

```js
import axios from "axios";

// GET request
async function getPuppies() {
  const response = await axios.get("/puppies");
  console.log(response.data); // Already parsed!
}

// POST request
async function createPuppy() {
  const response = await axios.post("/puppies", {
    name: "Buddy",
    breed: "Golden",
  });
  console.log("Created:", response.data);
}

// PATCH request (update)
async function updatePuppy(id) {
  const response = await axios.patch(`/puppies/${id}`, {
    name: "NewName",
  });
  console.log("Updated:", response.data);
}

// DELETE request
async function deletePuppy(id) {
  await axios.delete(`/puppies/${id}`);
  console.log("Deleted!");
}

// Error handling
async function safeFetch() {
  try {
    const response = await axios.get("/puppies");
    console.log(response.data);
  } catch (error) {
    console.error("Error:", error.response?.status, error.message);
  }
}

// With auth headers (common pattern)
async function fetchWithAuth() {
  const response = await axios.get("/puppies", {
    headers: {
      Authorization: `Bearer ${authToken}`,
    },
  });
  return response.data;
}
```

#### Response Structure:

```js
response = {
  data: { ... },        // The actual data (already parsed)
  status: 200,          // HTTP status code
  statusText: "OK",     // Status text
  headers: { ... },     // Response headers
  config: { ... }       // Request config
}
```

**Bottom line:** Use Axios for real projects. It saves time and reduces errors.

---

### ⚛️ `useEffect()` – React Hook for Side Effects

**What is a side effect?** Code that runs _outside_ your component (fetching data, timers, subscriptions, etc.)

**Why useEffect?** React components are meant to return JSX. When you need to do things like fetch data, you use `useEffect()`.

#### Import:

```js
import { useEffect, useState } from "react";
```

#### The Dependency Array Controls When It Runs:

```js
// ❌ No dependency array = runs EVERY render (infinite loops!)
useEffect(() => {
  fetch("/puppies"); // This runs every time component re-renders!
});

// ✅ Empty array [] = run ONCE on mount
useEffect(() => {
  fetch("/puppies"); // This runs only when component loads
}, []);

// ✅ With dependencies = run when they change
const [puppyId, setPuppyId] = useState(1);
useEffect(() => {
  fetch(`/puppies/${puppyId}`); // Runs when puppyId changes
}, [puppyId]);
```

#### Complete Example – Fetching Data:

```js
import { useEffect, useState } from "react";
import axios from "axios";

export function PuppyList() {
  const [puppies, setPuppies] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Fetch puppies when component loads
    axios
      .get("/puppies")
      .then((res) => {
        setPuppies(res.data);
        setLoading(false);
      })
      .catch((err) => {
        setError(err.message);
        setLoading(false);
      });
  }, []); // Empty array = run once on mount

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {puppies.map((puppy) => (
        <li key={puppy.id}>{puppy.name}</li>
      ))}
    </ul>
  );
}
```

#### Dependency Array Patterns:

| Dependencies        | Behavior                 | Use Case                            |
| ------------------- | ------------------------ | ----------------------------------- |
| `[]`                | Runs once on mount       | Load data on page load              |
| `[userId]`          | Runs when userId changes | Load user data when user changes    |
| `[puppyId, status]` | Runs when either changes | Update filter results               |
| (none)              | Runs every render        | ⚠️ Avoid - can cause infinite loops |

#### Cleanup Function (Preventing Memory Leaks):

```js
useEffect(() => {
  const socket = io(); // Connect to server

  socket.on("message", (data) => {
    console.log(data);
  });

  // Cleanup: disconnect when component unmounts
  return () => {
    socket.disconnect();
  };
}, []);
```

**Why cleanup?** If you don't disconnect/unsubscribe, you'll have multiple connections when the component remounts, causing memory leaks.

---

### ⚠️ ESLint – Code Quality Tool

**What is it?** ESLint automatically checks your code for style issues, potential bugs, and bad patterns.

**Why use it?**

- ✅ Catches bugs before they happen (unused variables, typos, etc.)
- ✅ Enforces consistent style (indentation, naming, etc.)
- ✅ Prevents common mistakes (infinite loops, unhandled errors)
- ✅ Helps your team write similar code

#### Common ESLint Errors:

```
"Unexpected console statement"
→ You used console.log() in production code (usually a mistake)

"'userData' is assigned a value but never used"
→ Remove unused variables to keep code clean

"React Hook useEffect has a missing dependency"
→ Add the missing variables to the dependency array
```

#### Example (ESLint in action):

```js
// ❌ ESLint error: Missing return statement
const getPuppies = async () => {
  const response = await axios.get("/puppies");
  // Missing return!
};

// ✅ Fixed
const getPuppies = async () => {
  const response = await axios.get("/puppies");
  return response.data;
};
```

**Tip:** Most modern code editors (VS Code, etc.) show ESLint errors in real-time as you type.

#### Setup:

```bash
npm install --save-dev eslint
npx eslint --init  # Interactive setup
```

---

## 🔗 Complete Real-World Example – Friend Profile Fetcher

Here's how all of these concepts work together:

### Backend API Route (Express.js):

```js
// GET /friends/:id - Fetch a specific friend's profile
app.get("/friends/:id", async (req, res) => {
  try {
    const friend = await db.query("SELECT * FROM users WHERE id = ?", [
      req.params.id,
    ]);
    res.json(friend);
  } catch (error) {
    res.status(500).json({ error: "Failed to fetch friend" });
  }
});
```

### Frontend Component (React):

```js
import { useEffect, useState } from "react";
import axios from "axios";

export function FriendProfile({ friendId }) {
  const [friend, setFriend] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchFriend = async () => {
      try {
        // API call: GET request to fetch friend data
        const response = await axios.get(`/friends/${friendId}`);
        setFriend(response.data);
        setLoading(false);
      } catch (err) {
        setError(err.message);
        setLoading(false);
      }
    };

    fetchFriend();
  }, [friendId]); // Re-run when friendId changes

  if (loading) return <p>Loading profile...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <h2>{friend.name}</h2>
      <p>Status: {friend.status}</p>
    </div>
  );
}
```

**What's happening:**

1. Component mounts → `useEffect` runs
2. `useEffect` calls `axios.get()` (async)
3. Promise resolves → response data arrives
4. Component state updates → component re-renders with new data
5. User sees friend profile

---

### 🔄 Complete Flow Summary

```
User Action
  ↓
UI Handler (onClick, etc.)
  ↓
axios.get/post/patch/delete() → Returns Promise
  ↓
Sent to Server as HTTP Request
  ↓
Server Process (authentication, database query, etc.)
  ↓
Server sends HTTP Response (status + data)
  ↓
Promise resolves, data arrives in .then() or await
  ↓
setState() updates React component
  ↓
Component re-renders with new data
  ↓
User sees updated UI
```

---

## 📊 Quick Reference Table

| Concept              | Purpose                      | Example                                            |
| -------------------- | ---------------------------- | -------------------------------------------------- |
| **API Route**        | Server endpoint for requests | `GET /puppies`, `POST /puppies`                    |
| **REST Method**      | Type of action               | GET=read, POST=create, PATCH=update, DELETE=remove |
| **Promise**          | Handle async operations      | `fetch().then().catch()`                           |
| **async/await**      | Clean Promise handling       | `await axios.get()` inside `async function`        |
| **Fetch**            | Built-in HTTP client         | `fetch("/puppies").then()`                         |
| **Axios**            | Better HTTP client           | `axios.get("/puppies")`                            |
| **useEffect**        | Run side effects in React    | Fetch data on component mount                      |
| **Dependency array** | Control when useEffect runs  | `[]` = once, `[id]` = when id changes              |
| **ESLint**           | Code quality checker         | Catches errors and style issues                    |

---

## 🎯 Best Practices

1. **Always handle errors** – Use `try/catch` or `.catch()`
2. **Show loading states** – Users know something is happening
3. **Use async/await** over `.then()` – Cleaner and easier
4. **Use Axios over Fetch** – Simpler API, more features
5. **Put API calls in useEffect** – Not in render function
6. **Add dependencies to useEffect** – Avoid infinite loops
7. **Validate data from server** – Never trust user/external data
8. **Use ESLint** – Catch bugs early

---

Let me know if you'd like this exported as a `.md` file or have questions about any concept!
