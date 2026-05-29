
# 📡 WebSockets in Web Development

  

> Learn what WebSockets are, when to use them, and how to implement them.

  

---

  

## 🔍 What are WebSockets?

  

> [!note]

> WebSockets allow **real-time**, **two-way communication** between client and server using a **persistent connection**.

  

- Full-duplex: both sides can send/receive anytime

- Avoids the need for repeated HTTP requests (polling)

- Common for chat, live updates, and collaboration apps

  

---

  

## 📈 When to Use WebSockets

  

> [!tip]+ Ideal use cases

> Use WebSockets when your app needs **instant updates** or **active user-to-user interaction**.

  

- 💬 Chat messaging

- 🔔 Live notifications

- 📅 Real-time calendar updates

- 🧑‍🤝‍🧑 Collaborative tools

- 🕹️ Online games

- 📊 Live dashboards

  

---

  

## 🧠 How It Works

  

```text

Client Server

| |

|------ WebSocket Handshake ---->|

|<------ Connection Established -|

| |

|-- send --> <-- receive -- |

|-- receive <-- send --> |

```

  

> [!note]

> One connection stays open, and both the client and server can send messages **anytime**.

  

---

  

## ⚙️ Setup Using Socket.io

  

### 📦 Install

  

```bash

npm install socket.io socket.io-client

```

  

### 🖥️ Server (Node.js)

  

```js

// Import required modules

const http = require("http"); // Create a basic HTTP server

const { Server } = require("socket.io"); // Import socket.io server

  

// Create the HTTP server

const server = http.createServer();

  

// Create the WebSocket server using the HTTP server

const io = new Server(server);

  

// Listen for a new client connection

io.on("connection", (socket) => {

console.log("User connected");

  

// Listen for a 'sendMessage' event from the client

socket.on("sendMessage", (data) => {

// Forward the message to the recipient by their ID

io.to(data.recipientId).emit("receiveMessage", data);

});

  

// Handle user disconnection

socket.on("disconnect", () => {

console.log("User disconnected");

});

});

  

// Start listening on port 3000

server.listen(3000);

```

  

### 🌐 Client (React or JS)

  

```js

// Import the socket.io client library

import { io } from "socket.io-client";

  

// Connect to the WebSocket server

const socket = io("http://localhost:3000");

  

// Listen for a successful connection

socket.on("connect", () => {

console.log("Connected to server");

});

  

// Send a message to another user via the server

socket.emit("sendMessage", {

recipientId: "user123", // ID of the user you're messaging

text: "Hey there!" // Message content

});

  

// Listen for incoming messages from the server

socket.on("receiveMessage", (data) => {

console.log("Message received:", data);

});

```

  

---

  

## 🧪 Key WebSocket Events

  

> [!example]+ Events to know

- `connect` — client successfully connected

- `disconnect` — client disconnected

- `emit(event, data)` — send data to server

- `on(event, callback)` — listen for messages

  

---

  

## ✅ Pros vs ❌ Cons

  

> [!summary]+ Pros

- 🔁 Real-time UI

- ⚡ Fast interactions

- 🧠 Great for interactive apps

  

> [!warning]+ Cons

- Requires more setup than REST

- Can be harder to scale

- Not useful for static apps

  

---

  

## 🧭 Summary

  

- WebSockets = **live, real-time interaction**

- REST = **basic data management**

- Combine both in apps like:

- Calendars with live attendance

- Messaging systems

- Notification platforms