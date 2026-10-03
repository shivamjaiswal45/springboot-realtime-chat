<div align="center">

# 💬 Spring Boot Real-Time Chat

### Instant messaging, powered by WebSocket, STOMP and SockJS.

A lightweight real-time chat application built with **Java** and **Spring Boot**. Messages are broadcast to every connected client the moment they are sent, with no page refresh needed.

<br>

![Java](https://img.shields.io/badge/Java-21-ED8B00?logo=openjdk&logoColor=white&style=for-the-badge)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-4.1-6DB33F?logo=springboot&logoColor=white&style=for-the-badge)
![Maven](https://img.shields.io/badge/Build-Maven-C71A36?logo=apachemaven&logoColor=white&style=for-the-badge)
![STOMP](https://img.shields.io/badge/Protocol-STOMP-0A66C2?style=for-the-badge)
![SockJS](https://img.shields.io/badge/Client-SockJS-F59E0B?style=for-the-badge)

[![Stars](https://img.shields.io/github/stars/shivamjaiswal45/springboot-realtime-chat?style=flat-square&color=F59E0B)](https://github.com/shivamjaiswal45/springboot-realtime-chat/stargazers)
[![Forks](https://img.shields.io/github/forks/shivamjaiswal45/springboot-realtime-chat?style=flat-square&color=10B981)](https://github.com/shivamjaiswal45/springboot-realtime-chat/fork)

<br>

<img src="./screenshot/Person 1.png" alt="Real-time chat application" width="90%">

</div>

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Screenshots](#-screenshots)
- [Features](#-features)
- [How It Works](#-how-it-works)
- [WebSocket API](#-websocket-api)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Try It with Two Users](#-try-it-with-two-users)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [Author](#-author)

---

## 🌟 Overview

This project shows how to build a real-time, multi-user chat with the Spring WebSocket stack. Open the app in two browser windows, pick a name in each, and watch messages appear on both sides instantly.

It is a compact, readable example of **WebSocket + STOMP messaging** in Spring Boot, a good starting point for learning or for building something bigger.

---

## 📸 Screenshots

<div align="center">

| 👤 Shivam's window | 👤 Parth's window |
| :---: | :---: |
| <img src="./screenshot/Person 1.png" alt="Chat as Shivam" width="450"> | <img src="./screenshot/person 2.png" alt="Chat as Parth" width="450"> |

*Two users, two browser windows, one live conversation.*

</div>

---

## ✨ Features

| | Feature | Details |
| :-: | :--- | :--- |
| ⚡ | **Real-time messaging** | Messages are delivered instantly over WebSocket, with no refresh or polling |
| 📡 | **Broadcast to everyone** | Every message is pushed to all connected clients |
| 👥 | **Multiple users** | Enter your own name and chat alongside others |
| 🔌 | **SockJS fallback** | Keeps working in browsers or networks where raw WebSocket is blocked |
| 📬 | **STOMP messaging** | Clean publish/subscribe model using the `/topic/messages` destination |
| 🎨 | **Simple, clean UI** | A responsive chat page with a message box, name field and Send button |

---

## 🔍 How It Works

```text
  ┌──────────────┐  1. SEND /app/sendMessage  ┌──────────────────────┐
  │   Browser    │ ─────────────────────────▶ │     Spring Boot      │
  │ (SockJS +    │       over STOMP           │   ChatController     │
  │  STOMP.js)   │                            └──────────┬───────────┘
  └──────▲───────┘                                       │
         │                                               │ 2. @SendTo
         │   3. every subscriber receives                │    /topic/messages
         └───────────────────────────────────────────────┘
```

1. The page opens a **SockJS** connection to the `/chat` endpoint and subscribes to `/topic/messages`.
2. When a user sends a message, the browser publishes a **STOMP** frame to `/app/sendMessage`.
3. `ChatController` receives it (`@MessageMapping("/sendMessage")`) and returns it with `@SendTo("/topic/messages")`.
4. The simple in-memory broker pushes the message to **every subscribed client**, so it appears instantly in all open windows.

---

## 🔌 WebSocket API

| Purpose | Destination | Direction |
| :--- | :--- | :--- |
| Connect (SockJS + STOMP endpoint) | `/chat` | Client → Server |
| Send a message | `/app/sendMessage` | Client → Server |
| Receive messages | `/topic/messages` | Server → All clients |
| Open the chat page | `GET /chat` | Browser |

**Message format** (`ChatMessage`)

```json
{
  "id": 1,
  "sender": "Shivam",
  "content": "Hey Parth, I just pushed the latest changes!"
}
```

> **Note:** the WebSocket endpoint currently allows the origin `http://localhost:8080` only (see `WebSocketConfig`). If you deploy the app to another domain, update `setAllowedOrigins` accordingly.

---

## 🛠 Tech Stack

| Layer | Technology |
| :--- | :--- |
| **Language** | Java 21 |
| **Framework** | Spring Boot 4.1 |
| **Real-time** | Spring WebSocket with STOMP messaging |
| **Templating** | Thymeleaf |
| **Boilerplate reduction** | Lombok |
| **Client** | SockJS and STOMP.js |
| **Frontend** | HTML, JavaScript and Bootstrap |
| **Build tool** | Maven (wrapper included) |

---

## 🗂 Project Structure

```text
springboot-realtime-chat/
└── app/
    ├── pom.xml
    ├── .mvn/
    └── src/
        ├── main/
        │   ├── java/com/chat/app/
        │   │   ├── AppApplication.java        # Spring Boot entry point
        │   │   ├── config/
        │   │   │   └── WebSocketConfig.java   # WebSocket + STOMP broker setup
        │   │   ├── controller/
        │   │   │   └── ChatController.java    # Message handling + /chat page
        │   │   └── model/
        │   │       └── ChatMessage.java       # Message data model
        │   └── resources/
        │       └── templates/
        │           └── chat.html              # Chat UI (Thymeleaf)
        └── test/
            └── java/com/chat/app/
                └── AppApplicationTests.java
```

---

## 🚀 Getting Started

**Prerequisites**

- **Java 21** or newer
- Any modern browser (Maven is not required, the wrapper is included)

**1. Clone the repository**

```bash
git clone https://github.com/shivamjaiswal45/springboot-realtime-chat.git
cd springboot-realtime-chat/app
```

**2. Run the application**

On macOS or Linux:

```bash
./mvnw spring-boot:run
```

On Windows:

```bash
mvnw.cmd spring-boot:run
```

You can also open the project in **IntelliJ IDEA** and run `AppApplication`.

**3. Open the chat**

Go to **http://localhost:8080/chat** in your browser.

---

## 👥 Try It with Two Users

1. Open **http://localhost:8080/chat** in one browser window and type the name `Shivam`.
2. Open the same address in a second window (or an incognito tab) and type the name `Parth`.
3. Send a message from either window and watch it show up in both at once. ⚡

---

## 🗺 Roadmap

- [x] Real-time messaging with WebSocket and STOMP
- [x] Broadcast messages to all connected clients
- [x] SockJS fallback support
- [ ] 🕒 Message timestamps
- [ ] 🎨 Improved UI with chat bubbles and avatars
- [ ] 💾 Message history with a database
- [ ] 🟢 Online users list and join/leave notifications
- [ ] 🔐 User authentication
- [ ] 💬 Private messages and chat rooms
- [ ] 🐳 Docker support

Have an idea? [Open an issue](https://github.com/shivamjaiswal45/springboot-realtime-chat/issues) and let's talk.

---

## 🤝 Contributing

Contributions are welcome!

1. **Fork** the project
2. **Create** a feature branch: `git checkout -b feature/amazing-feature`
3. **Commit** your changes: `git commit -m "Add amazing feature"`
4. **Push** to the branch: `git push origin feature/amazing-feature`
5. **Open** a Pull Request

---

## 👨‍💻 Author

**Shivam Jaiswal**

[![GitHub](https://img.shields.io/badge/GitHub-shivamjaiswal45-181717?style=flat-square&logo=github)](https://github.com/shivamjaiswal45)

---

<div align="center">

### If this project helped you learn something new, drop a ⭐. It means a lot!

<sub>Built with ☕, Java and a lot of WebSocket frames.</sub>

</div>
