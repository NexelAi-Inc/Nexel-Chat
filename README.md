# Nexel Chat

Nexel Chat is the AI/API workspace for Nexel Ai. It contains the FastAPI backend, local model routing, persistent memory, and optional VS Code extension integration.

## Setup

```powershell
python -m venv venv
.\venv\Scripts\activate
pip install -r requirements.txt
copy .env.example .env
```

Edit `.env` with your model paths and allowed frontend origins.

## Run API

```powershell
.\run_api.ps1
```

Or run the launcher:

```powershell
.\venv\Scripts\python.exe .\launcher.py --no-browser
```

## Frontend Connection

The Nexel Ai website should set:

```text
VITE_API_BASE_URL=https://your-nexel-chat-api-host.example.com
```

Add the deployed website domain to `CORS_ORIGINS` in this repo's `.env`.

## Do Not Commit

Keep these local only:

- `.env`
- `venv/`
- `models/`
- `data/`
