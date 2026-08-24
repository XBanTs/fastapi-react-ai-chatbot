# Full-Stack AI Chatbot

A full-stack AI chatbot built with **FastAPI** and **React**, integrating the **OpenAI API** to provide intelligent conversational responses through a modern web interface.

The project demonstrates how a React frontend can communicate with a Python FastAPI backend through a REST API, while keeping the OpenAI API credentials securely on the server side.

## Overview

This project consists of two separate applications working together:

* **Backend:** FastAPI-based REST API responsible for receiving user messages and communicating with OpenAI.
* **Frontend:** React application built with Vite that provides the chatbot user interface.
* **AI Layer:** OpenAI API used to generate assistant responses.
* **Persistence:** Browser `localStorage` is used to preserve the conversation history on the client side.

### Architecture

```text
┌─────────────────────────────┐
│       React Frontend        │
│     chatbot-frontend        │
│                             │
│  Chat UI → User Messages    │
└──────────────┬──────────────┘
               │
               │ HTTP POST /chat
               │
               ▼
┌─────────────────────────────┐
│       FastAPI Backend       │
│      chatbot-backend        │
│                             │
│  Request Validation         │
│  CORS Configuration         │
│  OpenAI API Integration     │
└──────────────┬──────────────┘
               │
               │ OpenAI API
               ▼
┌─────────────────────────────┐
│         OpenAI API          │
│                             │
│     AI-generated response   │
└──────────────┬──────────────┘
               │
               ▼
        FastAPI Response
               │
               ▼
        React Chat Interface
```

## Features

* AI-powered conversational chat
* FastAPI REST backend
* React frontend powered by Vite
* OpenAI API integration
* Secure backend-side API key management
* CORS configuration for frontend/backend communication
* Request validation using Pydantic
* Chat history persistence using browser `localStorage`
* Loading state while waiting for AI responses
* Error handling for failed API requests
* FastAPI automatic interactive API documentation
* Lightweight and easy-to-understand full-stack architecture

## Technology Stack

### Backend

* Python
* FastAPI
* Uvicorn
* Pydantic
* python-dotenv
* OpenAI Python SDK

### Frontend

* React
* Vite
* JavaScript
* HTML5
* CSS3
* Browser Local Storage API

### AI

* OpenAI API

## Project Structure

```text
full-stack-ai-chatbot/
│
├── chatbot-backend/
│   ├── .env
│   ├── .env.example
│   ├── .venv/
│   ├── main.py
│   └── requirements.txt
│
├── chatbot-frontend/
│   ├── node_modules/
│   ├── public/
│   ├── src/
│   │   ├── App.jsx
│   │   ├── App.css
│   │   ├── index.css
│   │   └── main.jsx
│   ├── index.html
│   ├── package.json
│   └── package-lock.json
│
├── .gitignore
└── README.md
```

> `.venv/`, `node_modules/`, and `.env` should not be committed to version control.

## Prerequisites

Before running the project, make sure you have:

* Python 3.7 or later
* Node.js and npm
* An OpenAI API key
* Git

You can verify your installations with:

```bash
python --version
node --version
npm --version
git --version
```

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/XBanTs/fastapi-react-ai-chatbot.git
cd fastapi-react-ai-chatbot
```

---

# Backend Setup

## 2. Navigate to the Backend

```bash
cd chatbot-backend
```

## 3. Create a Virtual Environment

### Windows

```bash
python -m venv .venv
```

Activate it:

```bash
.venv\Scripts\activate
```

### Linux/macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
```

## 4. Install Backend Dependencies

```bash
pip install -r requirements.txt
```

## 5. Configure Environment Variables

Create a `.env` file inside the `chatbot-backend` directory:

```env
OPENAI_API_KEY=your_openai_api_key_here
```

Never commit the `.env` file to GitHub.

For other developers, provide a `.env.example` file:

```env
OPENAI_API_KEY=your_openai_api_key_here
```

The `.env.example` file does not contain a real secret and can safely be committed.

## 6. Start the FastAPI Server

From inside `chatbot-backend`:

```bash
uvicorn main:app --reload
```

The backend will be available at:

```text
http://127.0.0.1:8000
```

### API Health Check

Open:

```text
http://127.0.0.1:8000/
```

A successful response should look similar to:

```json
{
  "status": "ok"
}
```

### Interactive API Documentation

FastAPI automatically provides Swagger UI at:

```text
http://127.0.0.1:8000/docs
```

The OpenAPI specification is also available at:

```text
http://127.0.0.1:8000/openapi.json
```

---

# Frontend Setup

## 7. Open a New Terminal

Keep the FastAPI server running and open another terminal.

Navigate to the frontend:

```bash
cd chatbot-frontend
```

## 8. Install Frontend Dependencies

```bash
npm install
```

## 9. Start the React Development Server

```bash
npm run dev
```

Vite will provide a local development URL, typically:

```text
http://localhost:5173
```

Open the displayed URL in your browser.

---

# How It Works

The application follows a straightforward request and response flow.

### 1. User enters a message

The user types a message into the React chat interface.

### 2. React sends the message

The frontend sends an HTTP `POST` request to the FastAPI backend:

```text
POST /chat
```

with a JSON payload:

```json
{
  "user_message": "Hello!"
}
```

### 3. FastAPI validates the request

The backend uses a Pydantic model to validate the incoming request.

### 4. FastAPI communicates with OpenAI

The backend sends the user's message to the OpenAI API using the API key stored in the backend environment.

The API key is never exposed to the React application.

### 5. OpenAI generates a response

The AI model processes the user's message and returns an assistant response.

### 6. FastAPI returns the response

The backend sends the generated response back to the React application.

Example:

```json
{
  "bot_response": "Hello! How can I help you today?"
}
```

### 7. React updates the chat

The frontend displays the assistant's response and stores the updated conversation in browser `localStorage`.

## API Endpoint

### Health Check

```text
GET /
```

Returns:

```json
{
  "status": "ok"
}
```

### Chat

```text
POST /chat
```

Request:

```json
{
  "user_message": "What is FastAPI?"
}
```

Response:

```json
{
  "bot_response": "FastAPI is a modern Python web framework..."
}
```

## CORS

The FastAPI backend is configured to allow requests from the local React development servers.

Typical development origins include:

```text
http://localhost:5173
http://localhost:3000
```

This allows the React frontend to communicate with the FastAPI backend during local development.

For production, CORS should be restricted to the actual frontend domain instead of allowing unnecessary origins.

## Environment Variables

The backend requires the following environment variable:

| Variable         | Description                                     |
| ---------------- | ----------------------------------------------- |
| `OPENAI_API_KEY` | API key used to authenticate requests to OpenAI |

Example:

```env
OPENAI_API_KEY=your_openai_api_key_here
```

### Security

Never:

* Commit `.env` to Git
* Put the OpenAI API key inside React source code
* Expose the API key through frontend environment variables
* Hard-code the API key inside `main.py`
* Share the API key publicly

The OpenAI API request is intentionally handled by the FastAPI backend so that the secret remains server-side.

## Development Workflow

Run the backend and frontend in separate terminals.

### Terminal 1

```bash
cd chatbot-backend
```

Activate the virtual environment if necessary:

```bash
.venv\Scripts\activate
```

Start FastAPI:

```bash
uvicorn main:app --reload
```

### Terminal 2

```bash
cd chatbot-frontend
npm run dev
```

Then open the frontend URL provided by Vite.

## Git and GitHub

The repository uses a single Git repository containing both applications:

```text
fastapi-react-ai-chatbot/
├── chatbot-backend/
├── chatbot-frontend/
├── .gitignore
└── README.md
```

The root `.gitignore` should exclude sensitive information and generated dependencies such as:

```text
.env
.venv/
node_modules/
__pycache__/
dist/
```

Before committing changes, verify the files Git is tracking:

```bash
git status
```

Then:

```bash
git add .
git commit -m "Describe your changes"
git push
```

## Troubleshooting

### Backend does not start

Make sure the virtual environment is activated and dependencies are installed:

```bash
pip install -r requirements.txt
```

Then try:

```bash
uvicorn main:app --reload
```

### OpenAI API errors

Check that your `.env` file exists inside `chatbot-backend` and contains:

```env
OPENAI_API_KEY=your_openai_api_key_here
```

Also verify that the key is valid and available to the OpenAI API.

### Frontend cannot communicate with backend

Make sure the FastAPI server is running:

```text
http://127.0.0.1:8000
```

Also verify that the frontend is sending requests to the correct backend URL and that the configured CORS origins match the frontend development URL.

### `npm` dependencies are missing

From `chatbot-frontend`, run:

```bash
npm install
```

Then:

```bash
npm run dev
```

### Chat history is not appearing

The application uses the browser's `localStorage` to persist chat history. Check that browser storage is enabled and that the application is running from the expected origin.

## Future Improvements

Possible enhancements for future versions include:

* Streaming AI responses
* Conversation/session management
* User authentication
* Database-backed conversation history
* Multiple AI models
* Configurable system prompts
* Markdown rendering for AI responses
* Code syntax highlighting
* Rate limiting
* Request logging
* Structured application logging
* Automated tests
* Production-ready CORS configuration
* Docker containerization
* Production deployment
* API versioning
* Improved error handling
* Responsive and accessible UI
* Token usage monitoring
* Persistent conversations across devices

## Learning Objectives

This project demonstrates practical concepts including:

* Building REST APIs with FastAPI
* Creating request models with Pydantic
* Configuring CORS
* Managing environment variables
* Integrating an external AI API
* Building React components
* Managing React state
* Handling asynchronous HTTP requests
* Connecting a JavaScript frontend to a Python backend
* Persisting client-side application state
* Using Vite for React development
* Running and managing separate frontend and backend services
* Structuring a full-stack application for Git-based collaboration

## License

This project is available under the MIT License.

## Acknowledgements

This project was built as a practical implementation of the concepts presented in the tutorial:

**Building a Full-Stack AI Chatbot with FastAPI (Backend) and React (Frontend)**

by Victor Pascal Dike.

The project uses FastAPI for the backend, React with Vite for the frontend, and the OpenAI API for AI-generated responses.
