<div align="center">

# 🦙 Spring Boot + Ollama + Qwen3.8

### A minimal, production-shaped REST API that talks to a locally-running LLM via Spring AI

![Java](https://img.shields.io/badge/Java-17+-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-4.1.0-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)
![Spring AI](https://img.shields.io/badge/Spring_AI-2.0.0-1E88E5?style=for-the-badge)
![Ollama](https://img.shields.io/badge/Ollama-Local_LLM-000000?style=for-the-badge&logo=ollama&logoColor=white)
![Qwen3.8](https://img.shields.io/badge/Qwen3.8-27B-7C3AED?style=for-the-badge)

**No cloud API key. No per-token billing. Just Spring Boot talking to a model running on your own machine.**

[Overview](#-overview) • [Endpoints](#-api-endpoints) • [Setup](#️-setup) • [Testing](#-testing) • [Project Structure](#-project-structure) • [Roadmap](#-roadmap)

</div>

---

## 🎯 Overview

This project wires a Spring Boot application to a local [Ollama](https://ollama.com) server running **Qwen3.8** (27B, vision + tools + thinking) using the official **Spring AI Ollama starter** — no custom HTTP client, no manual JSON plumbing.

```
Client (browser / Postman / curl)
        │  HTTP GET
        ▼
Spring Boot — ChatController
        │  OllamaChatModel.call() / .stream()
        ▼
Ollama server (localhost:11434)
        │  local inference
        ▼
Qwen3.8
        │  generated text
        ▼
Spring Boot ──▶ JSON / SSE stream ──▶ Client
```

Spring AI's `OllamaChatModel` is injected straight into the controller, so the integration is a handful of lines — a clean base to extend into a chatbot, coding assistant, or RAG app.

---

## ✨ Features

- 🔌 **Spring AI ↔ Ollama integration** via `spring-ai-starter-model-ollama` — no manual REST calls to the Ollama API
- 💬 **Synchronous generation** — `GET /ai/generate`, returns a plain JSON response
- 📡 **Streaming generation** — `GET /ai/generateStream`, returns a reactive `Flux<ChatResponse>` (token-by-token)
- 🧠 **Runs entirely locally** — Qwen3.8 runs on your machine through Ollama, no cloud key required
- 🧱 **Minimal footprint** — one controller, one application class, config-driven model selection

---

## 🧰 Tech Stack

| Layer | Technology |
|---|---|
| Language | Java 17 |
| Framework | Spring Boot 4.1.0 |
| AI integration | Spring AI 2.0.0 (`spring-ai-starter-model-ollama`) |
| LLM runtime | Ollama |
| Model | Qwen3.8 (27B · vision · tools · thinking) |
| Reactive streaming | Project Reactor (`Flux`) |
| Build tool | Maven |

---

## 🔌 API Endpoints

### 1. Generate (blocking)

```
GET /ai/generate?message=Tell me a joke
```

**Response**

```json
{
  "generation": "Why did the developer go broke? Because he used up all his cache."
}
```

`message` defaults to `"Tell me a joke"` if omitted.

### 2. Generate (streaming)

```
GET /ai/generateStream?message=Explain dependency injection
```

Returns a `Flux<ChatResponse>` — the response streams to the client as Server-Sent Events, chunk by chunk, instead of waiting for the full completion.

### Example requests

```bash
# Blocking
curl "http://localhost:8080/ai/generate?message=What is Spring Boot?"

# Streaming
curl "http://localhost:8080/ai/generateStream?message=Explain REST APIs in 3 lines"
```

> Both endpoints are `GET` with a query parameter, not `POST` with a JSON body — that's an intentional, quick-to-demo design (great for pasting a link straight into a browser to test).

---

## 📋 Prerequisites

| Tool | Check |
|---|---|
| Java 17+ | `java -version` |
| Maven | `mvn -version` (or use the bundled `./mvnw`) |
| Ollama | `ollama --version` |
| Qwen3.8 pulled | `ollama list` |

---

## ⚙️ Setup

### 1. Clone the repository

```bash
git clone https://github.com/Badalsingh25/springboot-ollama-ai.git
cd springboot-ollama-ai
```

### 2. Start Ollama and pull Qwen3.8

```bash
ollama serve          # starts the Ollama server (default: http://localhost:11434)
ollama pull qwen3.8
ollama run qwen3.8    # sanity check — try "Hello" and confirm it responds
```

### 3. Configure the model

Set the model name in `src/main/resources/application.properties`:

```properties
spring.application.name=deepseek

spring.ai.ollama.chat.model=qwen3.8
```

> ⚠️ In this repo the property currently ships **blank**. Spring AI won't know which model to call until you set it — fill in `qwen3.8` (or override it, see below) before running the app.

Spring AI defaults to `http://localhost:11434` for the Ollama base URL, so no extra config is needed if Ollama is running locally on its default port. To point at a different host, add:

```properties
spring.ai.ollama.base-url=http://localhost:11434
```

### 4. Run the application

```bash
./mvnw spring-boot:run
```

or build and run the jar directly:

```bash
./mvnw clean package
java -jar target/deepseek-0.0.1-SNAPSHOT.jar
```

The API is now live at `http://localhost:8080`.

---

## 🧪 Testing

Once the app is running:

```bash
curl "http://localhost:8080/ai/generate?message=Hello"
```

Or open in a browser:

```
http://localhost:8080/ai/generate?message=Write a haiku about Java
```

For the streaming endpoint, use a tool that renders SSE (Postman, or `curl -N`):

```bash
curl -N "http://localhost:8080/ai/generateStream?message=Count to 5 slowly"
```

### Suggested test prompts

| Category | Prompt |
|---|---|
| Basic | `Hello, how are you?` |
| Coding | `Write a Java binary search implementation.` |
| Reasoning | `Explain why REST APIs are stateless.` |
| SQL | `Write a MySQL query to find duplicate emails.` |
| Summarization | `Summarize the concept of dependency injection in 2 lines.` |

---

## 📁 Project Structure

```
deepseek/
├── src/
│   ├── main/
│   │   ├── java/com/spring/deepseek/
│   │   │   ├── ChatController.java        # /ai/generate + /ai/generateStream
│   │   │   └── DeepseekApplication.java   # Spring Boot entry point
│   │   └── resources/
│   │       ├── application.properties     # Ollama model config
│   │       ├── static/
│   │       └── templates/
│   └── test/
│       └── java/com/spring/deepseek/
│           └── DeepseekApplicationTests.java
├── pom.xml
└── README.md
```
---

## 🔐 Security Notes

This project is built for **local development**. Before exposing it beyond `localhost`:

- Add authentication/authorization on the endpoints
- Rate-limit requests (a local LLM is expensive per call)
- Validate/sanitize the `message` input
- Put it behind HTTPS if deployed
- Configure CORS explicitly instead of leaving it open

---

## 🚀 Roadmap

- [x] Ollama + Spring AI integration
- [x] Blocking generation endpoint
- [x] Streaming generation endpoint
- [ ] Conversation memory / chat history
- [ ] Prompt templates
- [ ] Switch endpoints to `POST` with a request body / DTOs
- [ ] Simple React or Thymeleaf chat UI
- [ ] RAG (document upload → embeddings → vector store → grounded answers)
- [ ] Dockerfile + docker-compose (app + Ollama)
- [ ] Basic auth / rate limiting for non-local deployment


---

## 📄 License

Licensed for learning and personal development use — see [`LICENSE`](./LICENSE) for details. Check Qwen3.8's own model license via Ollama before any commercial or redistribution use.

---

<div align="center">

Made with ☕ Java, 🌱 Spring Boot, and a locally-running 🧠 Qwen3.8

</div>
