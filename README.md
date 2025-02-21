# Real-Time Chat Application Using WebSockets, SockJS, and STOMP

## Introduction

This project is a real-time chat application built using **Spring Boot (backend)** and **Thymeleaf with JavaScript (frontend)**. It leverages **WebSockets** for bi-directional communication between clients and the server, ensuring instant message delivery. To handle WebSocket connections effectively and provide fallback options, we use **SockJS** and **STOMP (Simple Text Oriented Messaging Protocol)**.

## What Are WebSockets?

WebSockets are a communication protocol that enables full-duplex communication between a client and a server over a persistent connection. Unlike traditional HTTP, which follows a request-response cycle, WebSockets maintain an open connection, allowing both parties to send and receive messages in real-time.

### Advantages of WebSockets Over Other Technologies:

1. **Real-Time Communication**: Messages are pushed instantly, reducing latency compared to HTTP polling or long polling.
2. **Bi-Directional**: Unlike HTTP, where the client initiates requests, WebSockets allow both client and server to send messages anytime.
3. **Lower Overhead**: WebSockets reduce unnecessary HTTP headers and handshakes, optimizing bandwidth usage.
4. **Persistent Connection**: The connection remains open, eliminating repeated request-response cycles.
5. **Scalability**: Efficient use of network resources makes WebSockets suitable for high-load applications like chats, stock market updates, and online gaming.

## Why Use SockJS?

SockJS is a JavaScript library that provides a WebSocket-like API but includes **fallback mechanisms** for browsers or environments where native WebSockets are not available.

### Benefits of SockJS:

- **Fallback Support**: If WebSockets are not supported by the browser or blocked by firewalls, SockJS seamlessly switches to techniques like XHR-streaming or long polling.
- **Improved Compatibility**: Works in older browsers that don’t support WebSockets.
- **Resilient Connections**: Handles disconnects and reconnects automatically.

## Why Use STOMP?

STOMP (**Simple Text Oriented Messaging Protocol**) is a lightweight messaging protocol that runs over WebSockets, providing a structured way to send and receive messages.

### Benefits of STOMP:

- **Easy Message Routing**: Messages can be routed to specific destinations using a publish-subscribe model.
- **Subscription-Based Communication**: Clients can subscribe to specific topics to receive only relevant messages.
- **Interoperability**: Can be used with message brokers like **RabbitMQ** or **ActiveMQ** in enterprise applications.

## How WebSockets Are Used in This Project

### Backend (Spring Boot)

The backend is responsible for setting up WebSocket endpoints, handling messages, and broadcasting them to connected clients.

1. **WebSocket Configuration:**

   - WebSockets are enabled using `@EnableWebSocketMessageBroker`.
   - An endpoint (`/chat`) is exposed for clients to connect using SockJS.
   - A simple in-memory message broker (`/topic`) is used to relay messages.

2. **Message Handling:**

   - Clients send messages to `/app/sendMessage`.
   - The server processes these messages and broadcasts them to all subscribers of `/topic/messages`.

### Frontend (Thymeleaf & JavaScript)

The frontend connects to the WebSocket endpoint and renders messages in real-time.

1. **Establishing a Connection:**

   - A SockJS client creates a connection to `/chat`.
   - STOMP is used to subscribe to the topic `/topic/messages`.

2. **Sending Messages:**

   - Users input their name and message.
   - Messages are sent to `/app/sendMessage` using STOMP.

3. **Receiving Messages:**

   - Messages from `/topic/messages` are displayed dynamically in the chat window.

## Messaging Flow

1. **Client connects** → Establishes a WebSocket connection.
2. **User sends a message** → Message is sent via STOMP to the backend.
3. **Backend processes the message** → The message is broadcast to all subscribers.
4. **Clients receive the message** → The frontend updates the chat window instantly.

## Steps to Run the Application

1. **Clone the Repository:**

   ```sh
   git clone https://github.com/your-repo/chat-app.git
   cd chat-app
   ```

2. **Run the Backend (Spring Boot):**

   - Ensure you have Java and Maven installed.
   - Navigate to the project root and run:

   ```sh
   mvn spring-boot:run
   ```

   - The backend will start on `http://localhost:8080`.

3. **Run the Frontend (Thymeleaf-Based UI):**

   - Open `http://localhost:8080/chat` in a web browser.
   - The chat interface should load and establish a WebSocket connection.

4. **Test the Application:**

   - Open the chat interface in multiple browser tabs.
   - Enter names and messages in the input fields.
   - Verify that messages appear instantly across all open chat windows.

## Conclusion

This project demonstrates the power of WebSockets for real-time messaging. By integrating SockJS and STOMP, we ensure robust and scalable WebSocket communication that works seamlessly across different browsers and network conditions. This architecture is ideal for **chat applications, live notifications, and real-time data streaming**.

