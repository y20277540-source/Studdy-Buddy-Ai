# StudyBuddy AI

StudyBuddy AI is a friendly general-purpose assistant for learning, everyday questions, planning, brainstorming, and writing, powered by Google Gemini.

## Run & Operate

- `pnpm --filter @workspace/studybuddy-ai run dev` — start the app in development
- `pnpm --filter @workspace/studybuddy-ai run start` — start the Express server
- `pnpm --filter @workspace/studybuddy-ai run build` — check the server and browser JavaScript syntax
- Set `GEMINI_API_KEY` in Replit Secrets. The server reads it from `process.env`; never add it to browser code or a committed `.env` file.
- `POST /api/chat` accepts `{ "message": "...", "history": [], "images": [] }` and returns `{ "reply": "..." }`. Images are optional base64 attachments.
- `POST /api/images` accepts a prompt and optional reference images, then returns a generated image as base64.
- `POST /api/voice` accepts assistant reply text and returns Gemini-generated WAV audio.
- `GET /healthz` reports server health.

## Stack

- Node.js ESM and Express 5
- Official `@google/genai` SDK, called only from the server
- `express-rate-limit` for per-IP chat, image-generation, and voice limits
- Plain HTML, CSS, and JavaScript served from `artifacts/studybuddy-ai/public/`

## Where things live

- `artifacts/studybuddy-ai/server.js` — Express server, Gemini endpoint, rate limiting, input validation, and static file serving
- `artifacts/studybuddy-ai/public/` — browser UI assets
- `artifacts/studybuddy-ai/package.json` — app scripts and runtime dependencies

## Product

- One-page general-purpose chat with suggested prompts, typing feedback, session history, screenshot paste/photo upload, image understanding and generation, natural voice playback/stop controls, and recoverable errors
- Replies follow the language of the latest user message
- New chats are grouped into topic-based projects, with manual project creation and recent-chat navigation
- Project names, chat titles, questions, and replies persist in the current browser's local storage; image bytes are not saved there
- Gemini responses are stateless per request; the browser sends the current conversation history
- API credentials stay server-side in Replit Secrets

## Workspace

- The app is one package in the existing pnpm workspace; see the `pnpm-workspace` skill for workspace conventions.
