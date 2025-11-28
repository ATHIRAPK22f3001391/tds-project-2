# Quiz Solver Agent

An autonomous quiz-solving agent powered by LangGraph and Google Gemini 2.5 Flash. The agent navigates quiz pages, extracts questions, solves tasks using various tools, and submits answers automatically — all through a simple FastAPI interface.

## How It Works

1. You send a quiz URL to the `/solve` endpoint
2. The agent loads the page and extracts instructions, parameters, and the submit endpoint
3. It uses available tools (code execution, web scraping, OCR, audio transcription, etc.) to solve the task
4. Submits the answer to the correct endpoint
5. Follows any new URLs until all quizzes are completed

The agent uses a state graph architecture with automatic retry handling for malformed responses and a 180-second timeout per task.

## Setup

```bash
# Install dependencies using uv
uv sync

# Install Playwright browsers (required for web scraping)
playwright install
```

## Environment Variables

Create a `.env` file in the project root:

```env
EMAIL=your_email@example.com
SECRET=your_secret_key
```

- `EMAIL` - Your registered email for quiz submissions
- `SECRET` - Secret key for API authentication

## Running the Server

```bash
python main.py
```

The server starts on `http://localhost:7860`

## API Endpoints

### POST /solve

Triggers the agent to solve quizzes starting from the given URL.

**Request:**

```json
{
  "url": "https://quiz-url-here.com/start",
  "secret": "your_secret_key"
}
```

**Response:**

```json
{
  "status": "ok"
}
```

The solving happens in the background — the endpoint returns immediately.

### GET /healthz

Health check endpoint. Returns server status and uptime.

```json
{
  "status": "ok",
  "uptime_seconds": 120
}
```

## Available Tools

| Tool                     | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| `run_code`               | Execute Python code dynamically to solve computational tasks |
| `get_rendered_html`      | Scrape and render web pages using Playwright                 |
| `download_file`          | Download files from URLs                                     |
| `post_request`           | Submit answers to quiz endpoints                             |
| `add_dependencies`       | Install additional Python packages at runtime                |
| `ocr_image_tool`         | Extract text from images using Tesseract OCR                 |
| `transcribe_audio`       | Transcribe audio files to text                               |
| `encode_image_to_base64` | Convert images to base64 encoding                            |

## Docker

Build and run with Docker:

```bash
docker build -t quiz-agent .
docker run -p 7860:7860 --env-file .env quiz-agent
```

## Tech Stack

- **LangGraph** - Agent orchestration and state management
- **LangChain** - LLM integration
- **Google Gemini 2.5 Flash** - Language model
- **FastAPI** - REST API framework
- **Playwright** - Browser automation for web scraping
- **Tesseract** - OCR for image text extraction

---

STUDENT_EMAIL=23f2000006@ds.study.iitm.ac.in
