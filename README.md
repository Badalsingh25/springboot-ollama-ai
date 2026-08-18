::: {align="center"}
# 🚀 Spring Boot + Ollama AI

### Build AI-powered applications with a local LLM using Spring Boot, Ollama & Qwen3.8

```{=html}
<p>
```
`<img src="https://img.shields.io/badge/Java-17+-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white" alt="Java">`{=html}
`<img src="https://img.shields.io/badge/Spring_Boot-3.x-6DB33F?style=for-the-badge&logo=springboot&logoColor=white" alt="Spring Boot">`{=html}
`<img src="https://img.shields.io/badge/Ollama-Local_LLM-black?style=for-the-badge&logo=ollama&logoColor=white" alt="Ollama">`{=html}
`<img src="https://img.shields.io/badge/Qwen3.8-LLM-7C3AED?style=for-the-badge" alt="Qwen3.8">`{=html}
```{=html}
</p>
```
```{=html}
<p>
```
`<img src="https://img.shields.io/badge/API-REST-0EA5E9?style=flat-square" alt="REST API">`{=html}
`<img src="https://img.shields.io/badge/AI-Local_Inference-22C55E?style=flat-square" alt="Local AI">`{=html}
`<img src="https://img.shields.io/badge/License-MIT-yellow?style=flat-square" alt="License">`{=html}
```{=html}
</p>
```
**A developer-friendly reference project for connecting a Spring Boot
backend to a locally running Ollama LLM.**

[✨ Features](#-features) • [🏗️ Architecture](#️-architecture) • [⚙️
Setup](#️-setup) • [🔌 API](#-api) • [🧪 Testing](#-testing) • [📁
Structure](#-project-structure) • [🚀 Run](#-run-the-project) • [🛠️
Troubleshooting](#️-troubleshooting)
:::

------------------------------------------------------------------------

## 📌 Table of Contents

-   [🎯 About](#-about)
-   [✨ Features](#-features)
-   [🧰 Tech Stack](#-tech-stack)
-   [🏗️ Architecture](#️-architecture)
-   [🔄 Request Flow](#-request-flow)
-   [📋 Prerequisites](#-prerequisites)
-   [⚙️ Setup](#️-setup)
    -   [1. Clone](#1-clone-the-repository)
    -   [2. Start Ollama](#2-start-ollama)
    -   [3. Pull the model](#3-pull-the-model)
    -   [4. Configure Spring Boot](#4-configure-spring-boot)
    -   [5. Run Spring Boot](#5-run-spring-boot)
-   [🔌 API](#-api)
-   [🧪 Testing](#-testing)
-   [📁 Project Structure](#-project-structure)
-   [🛠️ Configuration](#️-configuration)
-   [🧯 Troubleshooting](#-troubleshooting)
-   [🔐 Security Notes](#-security-notes)
-   [📈 Future Improvements](#-future-improvements)
-   [🤝 Contributing](#-contributing)
-   [📄 License](#-license)
-   [👨‍💻 Author](#-author)

------------------------------------------------------------------------

## 🎯 About

**Spring Boot + Ollama AI** demonstrates how to integrate a locally
hosted Large Language Model (LLM) with a Spring Boot application.

Instead of sending prompts to a cloud AI provider, the application
communicates with an Ollama server running on your own machine:

``` text
Client
  │
  │ HTTP Request
  ▼
Spring Boot REST API
  │
  │ Prompt
  ▼
Ollama
  │
  │ Local inference
  ▼
Qwen3.8
  │
  │ Generated response
  ▼
Spring Boot
  │
  ▼
Client
```

This makes the project useful for learning:

-   Local LLM integration
-   REST API development
-   Spring Boot AI application architecture
-   Prompt/response handling
-   Local AI development without a cloud API key
-   Building a foundation for RAG, chat history, tools, and AI agents

> \[!IMPORTANT\] The LLM runs locally through Ollama. Your application
> still needs enough system memory/compute to load the selected model.

------------------------------------------------------------------------

## ✨ Features

```{=html}
<table>
```
```{=html}
<tr>
```
```{=html}
<td width="50%">
```
### 🤖 AI

-   🧠 Local LLM inference
-   💬 Natural-language prompts
-   🔌 Ollama integration
-   🧩 Model-based response generation
-   🔒 No mandatory cloud AI API key

```{=html}
</td>
```
```{=html}
<td width="50%">
```
### ☕ Backend

-   🚀 Spring Boot REST API
-   📡 HTTP communication
-   🧱 Clean backend structure
-   ⚙️ Externalized configuration
-   🧪 Easy API testing with Postman/cURL

```{=html}
</td>
```
```{=html}
</tr>
```
```{=html}
</table>
```
### 🔮 Designed for extension

The project can later be extended with:

-   Streaming responses
-   Conversation history
-   Prompt templates
-   RAG
-   Vector databases
-   Embeddings
-   Function/tool calling
-   Authentication & authorization
-   WebSocket/SSE
-   AI agents
-   React frontend

------------------------------------------------------------------------

## 🧰 Tech Stack

  Technology              Purpose
  ----------------------- ------------------------------------
  ☕ **Java 17+**         Backend language
  🌱 **Spring Boot**      Backend framework
  🦙 **Ollama**           Local LLM runtime
  🧠 **Qwen3.8**          Local language model
  🌐 **REST API**         Client ↔ Spring Boot communication
  🧪 **Postman / cURL**   API testing
  🛠️ **Maven**            Dependency & build management
  💻 **Git + GitHub**     Version control

------------------------------------------------------------------------

## 🏗️ Architecture

``` mermaid
flowchart LR
    A[Client / Postman / Frontend] -->|HTTP POST| B[Spring Boot REST API]
    B --> C[AI Service]
    C -->|HTTP API| D[Ollama]
    D --> E[Qwen3.8]
    E --> D
    D --> C
    C --> B
    B --> A
```

### Components

#### 1. Client

Sends a prompt to the Spring Boot API.

#### 2. Spring Boot

Receives the request, validates it, and delegates AI processing to the
service layer.

#### 3. AI Service

Builds the request sent to Ollama and handles the generated response.

#### 4. Ollama

Runs the selected LLM locally and exposes an HTTP API.

#### 5. Qwen3.8

Processes the prompt and generates the response.

------------------------------------------------------------------------

## 🔄 Request Flow

``` text
1. User enters a prompt
          ↓
2. Client sends POST request
          ↓
3. Spring Boot Controller receives request
          ↓
4. Service prepares the prompt
          ↓
5. Spring Boot calls Ollama
          ↓
6. Ollama loads/uses Qwen3.8
          ↓
7. Qwen3.8 generates response
          ↓
8. Ollama returns response
          ↓
9. Spring Boot returns JSON
          ↓
10. Client displays response
```

------------------------------------------------------------------------

# 📋 Prerequisites

Before running the project, install:

### ☕ Java

Java 17 or later.

Verify:

``` bash
java -version
```

Expected:

``` text
java version "17.x.x"
```

### 📦 Maven

Verify:

``` bash
mvn -version
```

### 🦙 Ollama

Install Ollama for your operating system and verify:

``` bash
ollama --version
```

### 🧠 Qwen3.8

Make sure the model is available:

``` bash
ollama list
```

You should see something similar to:

``` text
qwen3.8:latest
```

------------------------------------------------------------------------

# ⚙️ Setup

## 1. Clone the repository

``` bash
git clone https://github.com/YOUR_USERNAME/springboot-ollama-ai.git
cd springboot-ollama-ai
```

> Replace `YOUR_USERNAME` with your GitHub username.

------------------------------------------------------------------------

## 2. Start Ollama

Start the Ollama application.

You can also verify that the Ollama server is reachable:

``` bash
ollama list
```

The default Ollama API is:

``` text
http://localhost:11434
```

------------------------------------------------------------------------

## 3. Pull the model

If Qwen3.8 is not already installed:

``` bash
ollama pull qwen3.8
```

Check:

``` bash
ollama list
```

Test it directly:

``` bash
ollama run qwen3.8
```

Try:

``` text
Hello
```

If the model responds, Ollama is ready.

------------------------------------------------------------------------

## 4. Configure Spring Boot

Configure the Ollama connection in:

``` text
src/main/resources/application.properties
```

Example:

``` properties
spring.application.name=springboot-ollama-ai

server.port=8080

ollama.base-url=http://localhost:11434
ollama.model=qwen3.8
```

> If your implementation uses Spring AI's configuration properties
> instead, keep the property names required by your actual Spring AI
> version. Do not copy both configuration styles into the same
> application unless your code is designed for them.

------------------------------------------------------------------------

## 5. Run Spring Boot

Using Maven:

``` bash
mvn spring-boot:run
```

Or build first:

``` bash
mvn clean package
```

Then:

``` bash
java -jar target/*.jar
```

The backend should be available at:

``` text
http://localhost:8080
```

------------------------------------------------------------------------

# 🔌 API

The exact endpoint depends on your controller implementation.

A typical endpoint can look like:

``` http
POST /api/ai/chat
```

### Request

``` json
{
  "prompt": "Explain Spring Boot in simple words"
}
```

### Response

``` json
{
  "response": "Spring Boot is a Java framework..."
}
```

------------------------------------------------------------------------

## Example cURL Request

``` bash
curl -X POST http://localhost:8080/api/ai/chat \
  -H "Content-Type: application/json" \
  -d "{\"prompt\":\"Explain REST API in 3 lines\"}"
```

> Update `/api/ai/chat` and the JSON fields if your actual controller
> uses different names.

------------------------------------------------------------------------

# 🧪 Testing

## Test Ollama independently

Before debugging Spring Boot, make sure Ollama itself works:

``` bash
ollama run qwen3.8
```

If this works, test the backend.

## Test the Spring Boot API

Using Postman:

``` text
POST http://localhost:8080/api/ai/chat
```

Headers:

``` text
Content-Type: application/json
```

Body → raw → JSON:

``` json
{
  "prompt": "What is dependency injection in Spring?"
}
```

------------------------------------------------------------------------

## 🧪 Suggested Test Prompts

  Test            Prompt
  --------------- -------------------------------------------------
  Basic           `Hello, how are you?`
  Java            `Explain Java interfaces.`
  Spring Boot     `What is dependency injection?`
  SQL             `Write a MySQL query to find duplicate emails.`
  Coding          `Write a Java binary search implementation.`
  Reasoning       `Explain why REST APIs are stateless.`
  Summarization   `Summarize the following text: ...`

------------------------------------------------------------------------

# 📁 Project Structure

A recommended structure:

``` text
springboot-ollama-ai/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/ollama/
│   │   │       ├── controller/
│   │   │       │   └── AiController.java
│   │   │       │
│   │   │       ├── service/
│   │   │       │   └── AiService.java
│   │   │       │
│   │   │       ├── dto/
│   │   │       │   ├── ChatRequest.java
│   │   │       │   └── ChatResponse.java
│   │   │       │
│   │   │       └── OllamaApplication.java
│   │   │
│   │   └── resources/
│   │       └── application.properties
│   │
│   └── test/
│
├── .gitignore
├── pom.xml
└── README.md
```

> Adjust the package and filenames to match your actual project.

------------------------------------------------------------------------

# 🛠️ Configuration

A clean configuration strategy is to keep model and server settings
outside your Java code.

Example:

``` properties
ollama.base-url=${OLLAMA_BASE_URL:http://localhost:11434}
ollama.model=${OLLAMA_MODEL:qwen3.8}
```

This allows you to change the model without modifying Java source code.

### Example

Default:

``` text
OLLAMA_MODEL=qwen3.8
```

Another local model:

``` text
OLLAMA_MODEL=another-model
```

------------------------------------------------------------------------

# 🧯 Troubleshooting

```{=html}
<details>
```
```{=html}
<summary>
```
`<strong>`{=html}❌ Ollama connection refused`</strong>`{=html}
```{=html}
</summary>
```
Make sure Ollama is running.

Check:

``` bash
ollama list
```

Also verify the server URL:

``` text
http://localhost:11434
```

```{=html}
</details>
```
```{=html}
<details>
```
```{=html}
<summary>
```
`<strong>`{=html}❌ Model not found`</strong>`{=html}
```{=html}
</summary>
```
Check installed models:

``` bash
ollama list
```

If necessary:

``` bash
ollama pull qwen3.8
```

Make sure the model name in Spring Boot exactly matches the installed
model.

```{=html}
</details>
```
```{=html}
<details>
```
```{=html}
<summary>
```
`<strong>`{=html}❌ Out of memory`</strong>`{=html}
```{=html}
</summary>
```
A local LLM needs enough system memory/VRAM to load and run.

Check your available memory and use a smaller model if necessary.

For example:

``` bash
ollama pull qwen3:8b
```

Then configure:

``` properties
ollama.model=qwen3:8b
```

```{=html}
</details>
```
```{=html}
<details>
```
```{=html}
<summary>
```
`<strong>`{=html}❌ Port 8080 already in use`</strong>`{=html}
```{=html}
</summary>
```
Change:

``` properties
server.port=8080
```

to another available port:

``` properties
server.port=8081
```

```{=html}
</details>
```
```{=html}
<details>
```
```{=html}
<summary>
```
`<strong>`{=html}❌ Spring Boot cannot connect to
Ollama`</strong>`{=html}
```{=html}
</summary>
```
Check these three things:

1.  Ollama is running.
2.  The base URL is correct.
3.  The configured model exists in `ollama list`.

```{=html}
</details>
```

------------------------------------------------------------------------

# 🔐 Security Notes

This project is designed primarily for **local development**.

If you expose the Spring Boot API publicly, add appropriate security
controls:

-   🔐 Authentication
-   🛡️ Authorization
-   🚦 Rate limiting
-   🧹 Input validation
-   📝 Request logging
-   🔒 HTTPS
-   🌐 CORS configuration
-   🚫 Prompt abuse protection

Do not expose an unauthenticated local LLM endpoint directly to the
public internet.

------------------------------------------------------------------------

# 💡 Why Ollama?

Ollama makes it possible to run supported LLMs locally and communicate
with them through an API.

### Advantages

-   🏠 Local inference
-   💰 No per-request cloud API cost
-   🔐 Better control over local data
-   🧪 Easy experimentation
-   🔌 Simple API integration
-   🧑‍💻 Developer-friendly workflow

### Local vs Cloud

``` text
                AI Application
                     │
          ┌──────────┴──────────┐
          │                     │
       Cloud AI              Local AI
          │                     │
      Internet                Ollama
          │                     │
      API Provider          Qwen3.8
```

------------------------------------------------------------------------

# 🚀 Future Improvements

The current project can become a much more complete AI platform.

### Phase 1 --- Core

-   [x] Ollama integration
-   [x] Local LLM
-   [x] Spring Boot REST API
-   [x] Prompt → response flow

### Phase 2 --- Better UX

-   [ ] Streaming responses
-   [ ] Chat history
-   [ ] Conversation sessions
-   [ ] Prompt templates
-   [ ] React frontend

### Phase 3 --- RAG

-   [ ] Document upload
-   [ ] Text extraction
-   [ ] Chunking
-   [ ] Embeddings
-   [ ] Vector database
-   [ ] Retrieval
-   [ ] Context-aware answers

### Phase 4 --- Advanced AI

-   [ ] Tool/function calling
-   [ ] AI agents
-   [ ] Structured output
-   [ ] Model selection
-   [ ] Conversation memory
-   [ ] Evaluation pipeline

### Phase 5 --- Production

-   [ ] JWT authentication
-   [ ] Role-based access
-   [ ] Rate limiting
-   [ ] Docker
-   [ ] CI/CD
-   [ ] Monitoring
-   [ ] Centralized logging

------------------------------------------------------------------------

# 🧠 Example Use Cases

This integration can be used as the foundation for:

``` text
🤖 AI Chatbot
      │
      ├── 💬 General Chat
      ├── 👨‍💻 Coding Assistant
      ├── 📚 Study Assistant
      ├── 📄 Document Q&A
      ├── 🔎 RAG Application
      ├── ✉️ Email Assistant
      ├── 🧾 Resume Analyzer
      └── 🏢 Enterprise AI Assistant
```

------------------------------------------------------------------------

# 📊 High-Level Comparison

  Feature                             Local Ollama         Cloud LLM API
  --------------------------------- -------------- ---------------------
  Runs locally                                  ✅                    ❌
  Requires internet for inference               ❌            Usually ✅
  Per-request API cost                          ❌            Usually ✅
  Data stays on local machine                 ✅\*   Depends on provider
  Easy local development                ⭐⭐⭐⭐⭐              ⭐⭐⭐⭐
  Hardware requirement                      Higher         Lower locally

\* Your application, OS, network, logging, and other components can
still transmit data if configured to do so.

------------------------------------------------------------------------

# 🌟 Project Highlights

If you're using this project in your portfolio, you can describe it as:

> **Developed a Spring Boot REST API integrated with Ollama to run a
> local Qwen3.8 LLM, enabling AI-powered responses without relying on a
> cloud inference API. Designed the integration with configurable
> model/server properties and a service-oriented backend architecture.**

### Resume-friendly skills

``` text
Java • Spring Boot • REST API • Ollama • Local LLM
Qwen3.8 • Maven • JSON • AI Integration
```

------------------------------------------------------------------------

# 🤝 Contributing

Contributions are welcome!

### 1. Fork

``` text
Fork → Clone → Create Branch
```

### 2. Create a branch

``` bash
git checkout -b feature/your-feature
```

### 3. Commit

``` bash
git add .
git commit -m "feat: add your feature"
```

### 4. Push

``` bash
git push origin feature/your-feature
```

### 5. Open a Pull Request

Explain:

-   What changed
-   Why it changed
-   How to test it

------------------------------------------------------------------------

# 📄 License

This project is intended for learning and development.

If this repository is released under MIT, add the standard MIT license
text in a `LICENSE` file.

``` text
MIT License
```

> Check the individual licenses of Ollama models you redistribute or
> deploy. A model being locally runnable through Ollama does not
> automatically mean every model has identical licensing terms.

------------------------------------------------------------------------

# ⭐ Star This Repository

If this project helps you understand **Spring Boot + Local LLM
integration**, consider giving the repository a ⭐.

------------------------------------------------------------------------

::: {align="center"}
### 🦙 Build Local. Build Smart. Build with AI.

**Spring Boot × Ollama × Qwen3.8**

`<br>`{=html}

Made with ☕ Java & 🤖 AI
:::
