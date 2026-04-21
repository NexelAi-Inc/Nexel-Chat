# Nexel Chat

Nexel Chat is the GPT-style workspace for Nexel Ai. It contains the signed-in chat frontend, FastAPI backend, local model routing, persistent memory, and optional VS Code extension integration.

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

## Run Chat Frontend

```powershell
cd web
npm install
npm run dev
```

For production:

```powershell
cd web
npm run build
```

## Netlify Frontend

If you host the Nexel Chat frontend on Netlify, use:

```text
Build command: cd web && npm install && npm run build
Publish directory: web/dist
```

Set:

```text
VITE_API_BASE_URL=https://your-nexel-chat-api-host.example.com
VITE_FIREBASE_DATABASE_URL=https://nexel-ai-default-rtdb.firebaseio.com
VITE_NEXEL_AI_URL=https://your-nexel-ai-company-site.example.com
```

`VITE_API_BASE_URL` must be a live hosted Nexel Chat API. Netlify only hosts the static React frontend, so leaving this blank makes chat requests fail because `/generate` points at Netlify instead of FastAPI.

The API host must allow your Netlify domain in `CORS_ORIGINS`, for example:

```text
CORS_ORIGINS=https://nexelchat.netlify.app,http://localhost:5173,http://127.0.0.1:5173
```

## Do Not Commit

Keep these local only:

- `.env`
- `venv/`
- `models/`
- `data/`
