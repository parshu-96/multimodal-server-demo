# MultiModal Server Demo

A Spring Boot demo application that provides a multimodal AI chat experience with **text and image inputs**, powered by OpenAI's `gpt-4o-mini` model through Spring AI. The application includes a built-in, single-page chat UI for interacting with the AI assistant.

## Features

- 💬 **Conversational AI chat** with in-memory message history.
- 🖼️ **Image upload support** for vision-based queries using `multipart/form-data`.
- 🧠 **OpenAI integration** through Spring AI's `ChatClient` abstraction.
- 🎨 **Dark-themed chat interface** with image previews and a thinking indicator.
- 🔁 **Conversation reset** with a single click.
- 📱 **Single-page frontend** built using HTML, CSS, and vanilla JavaScript.

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Spring Boot 4.1.1 (WebMVC) |
| AI Integration | Spring AI 2.0.1 |
| AI Model | OpenAI `gpt-4o-mini` |
| Language | Java 21 |
| Frontend | HTML, CSS, Vanilla JavaScript |
| Build Tool | Gradle |
| API Communication | REST API, Multipart Form Data |

## Prerequisites

Before running the application, ensure you have:

- **JDK 21 or later**
- **An OpenAI API key**
- Git (optional, for cloning the repository)

## Getting Started

### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd <your-repository-directory>
```

Replace the repository URL and directory with your actual GitHub repository details.

### 2. Configure Your OpenAI API Key

The application reads the API key from the `OPENAI_API_KEY` environment variable.

**Linux / macOS:**

```bash
export OPENAI_API_KEY=your_openai_api_key
```

**Windows PowerShell:**

```powershell
$env:OPENAI_API_KEY="your_openai_api_key"
```

Replace `your_openai_api_key` with your actual API key. Do not commit your API key to GitHub.

### 3. Run the Application

Using the Gradle wrapper:

**Linux / macOS:**

```bash
./gradlew bootRun
```

**Windows:**

```powershell
.\gradlew.bat bootRun
```

Gradle will build and start the Spring Boot application.

### 4. Open the Chat Interface

Once the application has started, navigate to:

**http://localhost:8080/**

You can now chat with the AI assistant, upload images, ask questions about their contents, and reset the conversation.

## API Reference

### `POST /api/chat`

Sends a text message to the AI assistant, optionally including an image attachment.

**Request type:** `multipart/form-data`

| Parameter | Type | Required | Description |
|---|---|---|---|
| `message` | String | Yes | The user's text prompt |
| `image` | MultipartFile | No | Image attachment (maximum 10 MB) |

#### Example: Send a Text Message and Image

```bash
curl -X POST http://localhost:8080/api/chat \
  -F "message=What is in this image?" \
  -F "image=@photo.png"
```

Replace `photo.png` with the path to your image file.

#### Example: Send a Text-Only Message

```bash
curl -X POST http://localhost:8080/api/chat \
  -F "message=Explain what multimodal AI is."
```

**Response:** The assistant's reply as plain text.

### `DELETE /api/chat`

Clears the in-memory conversation history.

#### Example

```bash
curl -X DELETE http://localhost:8080/api/chat
```

**Response:** An HTTP success status indicating that the conversation history has been cleared.

## Configuration

Application settings are defined in `src/main/resources/application.yaml`.

| Property | Value | Purpose |
|---|---|---|
| `spring.ai.openai.api-key` | `${OPENAI_API_KEY}` | OpenAI API credentials |
| `spring.ai.openai.chat.model` | `gpt-4o-mini` | AI chat and vision model |
| `spring.ai.openai.chat.temperature` | `0.4` | Controls response variability |
| `spring.servlet.multipart.max-file-size` | `10MB` | Maximum individual file size |
| `spring.servlet.multipart.max-request-size` | `10MB` | Maximum multipart request size |

The `gpt-4o-mini` model supports both text and image inputs, making it suitable for this multimodal demonstration.

## Project Structure

```text
MultiModalDemo/
├── src/
│   └── main/
│       ├── java/
│       │   └── com/
│       │       └── parshu/
│       │           └── MultiModalDemo/
│       │               ├── controller/
│       │               │   └── ChatController.java
│       │               └── service/
│       │                   └── ChatService.java
│       └── resources/
│           ├── application.yaml
│           └── static/
│               └── index.html
├── gradlew
├── gradlew.bat
├── build.gradle
└── README.md
```

### Key Components

- **`ChatController.java`** — Exposes the REST endpoints for sending messages and clearing conversation history.
- **`ChatService.java`** — Handles Spring AI `ChatClient` interactions, image processing, and conversation history.
- **`application.yaml`** — Contains OpenAI integration and multipart upload configuration.
- **`index.html`** — Provides the single-page chat interface, including image previews and chat interactions.

## How It Works

1. The user enters a message and optionally uploads an image through the web interface.
2. The frontend sends the request to `POST /api/chat` using multipart form data.
3. The Spring Boot controller receives the request and passes it to the chat service.
4. The service prepares the text and optional image input and invokes the OpenAI model through Spring AI.
5. The assistant generates a response, which is returned to the frontend and displayed in the conversation.
6. The user can reset the conversation using `DELETE /api/chat` or the UI's reset action.

## Notes and Limitations

- **In-memory conversation history:** A single `List<Message>` in `ChatService` is shared across all clients. Conversation history is lost when the application restarts.
- **No user-specific sessions:** Messages from different clients may share the same conversation context.
- **Hardcoded system prompt:** The current system prompt is `"You are a helpful AI assistant…"`.
- **No authentication:** The application is intended for local development and demonstration, not direct public exposure.
- **Image upload limits:** Individual files and total multipart requests are limited to 10 MB. Supported image formats also depend on the model and request handling implementation.
- **OpenAI API usage:** Requests may incur charges according to your OpenAI API account and model pricing.

## Future Improvements

- 🔐 Add authentication and authorization.
- 👤 Maintain separate conversation histories for individual users.
- 💾 Persist conversations using a database.
- 🧪 Add unit and integration tests.
- 🛡️ Add image validation, rate limiting, and request security.
- 🚀 Containerize the application with Docker.
- ☁️ Deploy the application to a cloud platform.

## Learning Objectives

This project demonstrates how to:

- Integrate OpenAI models into a Spring Boot application using Spring AI.
- Build conversational AI applications with `ChatClient`.
- Process multimodal requests containing both text and images.
- Expose REST APIs that accept multipart file uploads.
- Serve a lightweight frontend from Spring Boot's static resources.

## License

This project is intended for learning and demonstration purposes. Add a license file if you plan to distribute or open-source the project.
