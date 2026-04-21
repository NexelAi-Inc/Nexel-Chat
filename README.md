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

The Nexel Chat frontend needs a public FastAPI backend URL:

```text
VITE_API_BASE_URL=https://your-public-nexel-chat-api.example.com
```

Do not use `http://127.0.0.1:8000` in Netlify for public use. That address only means "this same computer", so it works for local testing but fails for everyone else.

Add the deployed frontend domains to `CORS_ORIGINS` in this repo's backend `.env`.

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
VITE_API_BASE_URL=https://your-public-nexel-chat-api.example.com
VITE_FIREBASE_DATABASE_URL=https://nexel-ai-default-rtdb.firebaseio.com
VITE_NEXEL_AI_URL=https://nexelai.netlify.app
```

`VITE_API_BASE_URL` must be a live hosted Nexel Chat API. Netlify only hosts the static React frontend, so leaving this blank makes chat requests fail because `/generate` points at Netlify instead of FastAPI.

The API host must allow your Netlify domain in `CORS_ORIGINS`, for example:

```text
CORS_ORIGINS=https://nexelchat.netlify.app,https://nexelai.netlify.app,http://localhost:5173,http://127.0.0.1:5173
```

## Make It Public

For Nexel Chat to work for everyone, the Python API must be reachable from the internet. Choose one of these paths:

1. Fastest testing path: run the API on your PC and expose it with Cloudflare Tunnel or ngrok. Use the tunnel URL as `VITE_API_BASE_URL` in Netlify.
2. Demo path without using your PC: run the API in Google Colab using `colab/Nexel_Chat_API_Colab.ipynb`. Use the generated Cloudflare Tunnel URL as `VITE_API_BASE_URL` in Netlify.
3. Production path: deploy the FastAPI backend to a cloud machine with enough RAM or GPU for your model. Use that public HTTPS URL as `VITE_API_BASE_URL`.

Example public setup:

```text
Nexel Ai website: https://nexelai.netlify.app
Nexel Chat frontend: https://nexelchat.netlify.app
Nexel Chat API: https://api.your-domain.com
```

Netlify variables for Nexel Chat:

```text
VITE_API_BASE_URL=https://api.your-domain.com
VITE_FIREBASE_DATABASE_URL=https://nexel-ai-default-rtdb.firebaseio.com
VITE_NEXEL_AI_URL=https://nexelai.netlify.app
```

Backend `.env` on the API host:

```text
CORS_ORIGINS=https://nexelchat.netlify.app,https://nexelai.netlify.app
```

## Google Colab API

Colab is useful when you do not want your PC hosting the backend. It is still temporary: sessions can disconnect, sleep, or reset, and the quick tunnel URL changes each time.

Open `colab/Nexel_Chat_API_Colab.ipynb` in Google Colab, enable a GPU runtime, run all cells, then copy the printed values into Netlify:

```text
VITE_API_BASE_URL=https://generated-name.trycloudflare.com
VITE_FIREBASE_DATABASE_URL=https://nexel-ai-default-rtdb.firebaseio.com
VITE_NEXEL_AI_URL=https://nexelai.netlify.app
```

Redeploy Nexel Chat after changing the Netlify variables.

## Do Not Commit

Keep these local only:

- `.env`
- `venv/`
- `models/`
- `data/`
