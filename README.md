# EduGenie – Google Gemini 3.8 Flash Powered Learning Assistant

EduGenie is an AI-powered educational learning assistant designed to help learners understand concepts, ask questions, summarize educational content, generate quizzes, and create personalized learning paths.

The application uses **Google Gemini 3.8 Flash** through a **FastAPI** backend and provides a simple web interface using **HTML, CSS, and JavaScript**.

---

## Features

EduGenie provides the following educational features:

* Ask educational questions
* Explain topics based on learner level
* Summarize educational text
* Generate quizzes
* Generate personalized learning recommendations
* Support Beginner, Intermediate, and Advanced learner levels
* Optional local explanation model
* Structured AI responses using Pydantic schemas
* API validation using FastAPI and Pydantic
* Automated API testing using Pytest
* Responsive web interface

---

## Technology Stack

### Backend

* Python
* FastAPI
* Pydantic
* Pydantic Settings
* Jinja2

### Generative AI

* Google Gemini 3.8 Flash
* Google GenAI Python SDK

### Frontend

* HTML5
* CSS3
* JavaScript

### Testing

* Pytest
* HTTPX
* FastAPI TestClient

### Optional Local AI Model

* MBZUAI/LaMini-Flan-T5-783M
* Hugging Face Transformers

---

## Project Structure

```text
EduGenie/
│
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── config.py
│   ├── schemas.py
│   ├── gemini_service.py
│   ├── local_explainer.py
│   └── services.py
│
├── templates/
│   └── index.html
│
├── static/
│   ├── style.css
│   └── app.js
│
├── tests/
│   └── test_api.py
│
├── scripts/
│   └── test_api.ps1
│
├── .vscode/
│   └── settings.json
│
├── .env
├── .env.example
├── .gitignore
├── requirements.txt
└── README.md
```

---

# Application Architecture

```text
User
  │
  ▼
Web Interface
HTML + CSS + JavaScript
  │
  ▼
FastAPI Backend
  │
  ▼
Application Services
  │
  ├── Question Answering
  ├── Topic Explanation
  ├── Text Summarization
  ├── Quiz Generation
  └── Learning Recommendations
  │
  ▼
Google Gemini 3.8 Flash
  │
  ▼
AI Generated Response
  │
  ▼
Web Interface
```

---

# Main Features

## 1. Ask a Question

Users can enter an educational question and select their learner level.

Example:

```text
Explain photosynthesis in simple terms.
```

The application sends the question to Gemini and generates an educational response.

API endpoint:

```text
POST /qa
```

Request:

```json
{
    "question": "Explain photosynthesis",
    "learner_level": "beginner"
}
```

---

## 2. Explain a Topic

Users can provide a topic and select their learner level.

Example:

```text
Newton's Laws of Motion
```

The AI explanation is structured into:

1. Simple definition
2. Core idea
3. Step-by-step explanation
4. Practical example
5. Common mistakes
6. Short recap

API endpoint:

```text
POST /explain
```

---

## 3. Summarize Text

Users can paste educational content and receive a concise summary.

The generated result contains:

* Concise summary
* Five key points
* Important terms, if applicable

API endpoint:

```text
POST /summarize
```

---

## 4. Generate Quiz

Users can provide educational content and generate a quiz.

The application requires:

* Exactly 3 questions
* Exactly 4 options for each question
* Exactly 1 correct option for each question

API endpoint:

```text
POST /quiz
```

The response is validated using the `QuizResponse` Pydantic model.

---

## 5. Learning Recommendations

Users can provide:

* Topic
* Learner level
* Learning goal

The application generates an ordered learning path containing 5 to 7 learning steps.

Each step contains:

* Step number
* Topic
* Description
* Activity

API endpoint:

```text
POST /learn/recommendations
```

---

# API Endpoints

| Method | Endpoint                 | Description                      |
| ------ | ------------------------ | -------------------------------- |
| GET    | `/`                      | Opens the EduGenie web interface |
| GET    | `/health`                | Checks application health        |
| POST   | `/qa`                    | Answers educational questions    |
| POST   | `/explain`               | Explains a topic                 |
| POST   | `/summarize`             | Summarizes educational text      |
| POST   | `/quiz`                  | Generates a structured quiz      |
| POST   | `/learn/recommendations` | Generates a learning path        |

---

# Configuration

EduGenie uses environment variables for configuration.

Create a `.env` file in the project root.

Example:

```env
GEMINI_API_KEY=YOUR_GEMINI_API_KEY
GEMINI_MODEL=gemini-3.8-flash
GEMINI_THINKING_LEVEL=medium

APP_HOST=127.0.0.1
APP_PORT=8000

USE_LOCAL_EXPLAINER=false
LOCAL_MODEL_NAME=MBZUAI/LaMini-Flan-T5-783M
```

> Never publish your real Gemini API key in GitHub, documentation, screenshots, or source code.

---

# Installation

## Step 1 – Open the Project

Open the EduGenie project folder in VS Code.

---

## Step 2 – Create Virtual Environment

Open the VS Code terminal and run:

```powershell
python -m venv .venv
```

---

## Step 3 – Activate Virtual Environment

For Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

After activation, the terminal should show:

```text
(.venv)
```

---

## Step 4 – Install Dependencies

Run:

```powershell
pip install -r requirements.txt
```

---

## Step 5 – Configure Environment Variables

Copy the example environment file:

```powershell
Copy-Item .env.example .env
```

Open `.env` and add your Gemini API key:

```env
GEMINI_API_KEY=YOUR_GEMINI_API_KEY
```

---

# Running the Application

Start the FastAPI application with:

```powershell
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

The application will run at:

```text
http://127.0.0.1:8000
```

Open the address in your browser.

---

# Health Check

You can verify that the backend is running by opening:

```text
http://127.0.0.1:8000/health
```

Expected response:

```json
{
    "status": "ok",
    "service": "EduGenie",
    "model": "gemini-3.8-flash"
}
```

---

# Testing

EduGenie includes automated API tests.

Run:

```powershell
pytest
```

The tests verify:

* Home page availability
* Health endpoint
* Q&A validation
* Topic explanation validation
* Quiz validation

---

# PowerShell API Test

A PowerShell health-check script is also provided.

Run:

```powershell
.\scripts\test_api.ps1
```

The script sends a request to:

```text
http://127.0.0.1:8000/health
```

and displays the response as JSON.

---

# Validation

The application uses Pydantic models to validate API requests and AI-generated structured responses.

For example, quiz validation requires:

```text
3 questions
     │
     ├── Question 1
     │      ├── 4 options
     │      └── 1 correct answer
     │
     ├── Question 2
     │      ├── 4 options
     │      └── 1 correct answer
     │
     └── Question 3
            ├── 4 options
            └── 1 correct answer
```

Invalid input is rejected by FastAPI with HTTP status:

```text
422 Unprocessable Entity
```

---

# Optional Local Explainer

EduGenie contains an optional local explanation model.

Configuration:

```env
USE_LOCAL_EXPLAINER=false
```

When enabled, the application uses:

```text
MBZUAI/LaMini-Flan-T5-783M
```

If the local model fails, the application automatically falls back to Gemini for topic explanation.

The local model requires the appropriate Transformers dependency to be installed.

---

# Security

Important security practices:

1. Never expose the Gemini API key.
2. Store API credentials inside `.env`.
3. Do not commit `.env` to Git.
4. Keep `.env` in `.gitignore`.
5. Use a placeholder in `.env.example`.
6. Rotate or revoke an API key if it has been exposed.
7. Do not include API keys in screenshots or project documentation.

---

# Error Handling

The FastAPI endpoints use exception handling to return an HTTP `502` response when an AI service operation fails.

Example:

```json
{
    "detail": "Request failed."
}
```

The frontend displays the returned error message to the user.

---

# Frontend

The frontend consists of:

### `templates/index.html`

Provides the application interface.

### `static/style.css`

Provides:

* Layout
* Cards
* Forms
* Buttons
* Quiz styling
* Responsive design

### `static/app.js`

Handles:

* Task selection
* Form updates
* API requests
* Loading state
* Response rendering
* Quiz rendering
* Learning recommendation rendering
* Error handling

---

# Backend

The backend consists of:

### `app/main.py`

Creates the FastAPI application and API endpoints.

### `app/config.py`

Loads environment-based application settings.

### `app/schemas.py`

Defines request and response validation models.

### `app/gemini_service.py`

Connects EduGenie with Google Gemini.

### `app/local_explainer.py`

Provides optional local AI topic explanation.

### `app/services.py`

Contains the educational application logic and prompts.

---

# Testing Structure

```text
tests/
└── test_api.py
```

The test suite currently checks:

```text
GET /
GET /health
POST /qa     → invalid input
POST /explain → invalid input
POST /quiz    → invalid input
```

---

# Example User Flow

```text
1. User opens EduGenie
        ↓
2. Selects "Explain a Topic"
        ↓
3. Sel
```
