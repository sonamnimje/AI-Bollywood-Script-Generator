AI Bollywood Script Generator

A small web app that generates Bollywood-style short dramas using an LLM (via OpenRouter). Frontend is a React + Vite app and backend is a FastAPI service that calls an LLM to produce structured JSON dramas and scenes.

---

## Project overview

This project generates short Bollywood-style drama scripts from a user-provided situation and mood. The frontend provides an interactive UI for entering prompts, viewing generated scenes, and regenerating individual scenes. The backend builds prompts, calls an LLM (via OpenRouter), and validates the model's JSON output into typed responses.

## Features

- Generate a multi-scene drama from a short situation + mood
- Regenerate a single scene within an existing drama
- Stores and displays generation history in the UI
- Resilient parsing of model output (nudges model to return JSON)
- Simple API endpoints for integration or automation

## Tech stack

- Frontend: React (Vite), Tailwind CSS
- Backend: FastAPI, Python 3.11
- HTTP client: httpx (async)
- LLM integration: OpenRouter (configurable model via .env)
- Dev tooling: Uvicorn (ASGI server)

## Setup (development)

1. Clone the repo:

   ```bash
   git clone https://github.com/sonamnimje/AI-Bollywood-Script-Generator.git

If you want, I can also add example screenshots, a `.env.example`, or update README to include API schema details. Which would you like next?
```bash
curl -X POST "http://127.0.0.1:8000/api/generate" \
  -H "Content-Type: application/json" \
  -d '{"situation":"Two strangers meet on a train","mood":"romantic"}'
```

If the backend returns a 503 with a message about OPENROUTER_API_KEY, ensure your backend/.env contains a valid OpenRouter API key.

## Screenshots

Place screenshots in `frontend/public/screenshots/` or `docs/screenshots/` and reference them here. Example markdown:

```markdown
![App home](frontend/public/screenshots/home.png)
```

There are no committed screenshots in this repository by default. Add your own to demonstrate the UI.

## Architecture

- Frontend (React + Vite): UI, form handling, and calls the backend API.
- Backend (FastAPI): request validation, prompt building, calls to OpenRouter, response parsing and validation via Pydantic models in backend/app/models/schemas.py.
- LLM (OpenRouter): model generates textual output; backend extracts the first JSON blob and validates it against the `DramaResponse` schema.

Flow:

User -> Frontend -> Backend (/api) -> OpenRouter -> Backend validates -> Frontend displays

## AI usage explanation

- Prompts are constructed in backend/app/prompts/prompt_builder.py to request structured JSON output describing scenes, characters, and metadata.
- The service sends the prompt to OpenRouter (backend/app/services/llm_service.py) and attempts to parse JSON from the model response using extract_first_json in backend/app/utils/json_utils.py.
- If parsing fails the backend will retry several times, appending a nudging instruction to produce strictly JSON output.
- Models and keys are configurable via backend/.env using OPENROUTER_API_KEY and OPENROUTER_MODEL.

## Contributing

- Open an issue or submit a PR. Keep API/contract changes backwards compatible when possible.

## License

This project does not include a license file by default. Add one if you intend to publish under a specific license.

---


