# BuilderAI

An AI website builder. Describe a website in plain text and the app plans the file structure, generates a complete React project file by file, and shows a live preview you can keep editing.
## Features

- Sign up and log in (JWT stored in an httpOnly cookie)
- Generate a full React website from a single prompt
- Two-phase generation: plan the files first, then generate each file in parallel with automatic retries
- Live progress while the project is being generated
- Live preview powered by Sandpack
- File explorer and code view for every generated file
- Project list on the home page, with delete

In progress: chat-based edits, manual code editing with auto-save, publish and export (zip).

## Tech Stack

**Frontend:** React (Vite), React Router, Tailwind CSS, Axios, Sandpack, react-hot-toast, lucide-react

**Backend:** Node.js, Express, MongoDB with Mongoose, JWT, bcrypt, cookie-parser, CORS

**AI:** Vercel AI SDK with zod schemas, called through OpenRouter (free models supported)

## Project Structure

```
Builder-AI/
├── backend/
│   ├── config/         # database connection
│   ├── controllers/    # auth, project, chat
│   ├── middleware/     # auth middleware
│   ├── models/         # User, Project
│   ├── routes/         # auth and project routes
│   ├── services/       # AI generation, prompts, code validation
│   └── server.js
└── frontend/
    └── src/
        ├── api/        # axios instance
        ├── components/ # chat panel, file explorer, preview, etc.
        ├── context/    # app-wide state
        └── pages/      # home, builder, preview, auth
```

## Getting Started

### Prerequisites

- Node.js 18 or newer
- A MongoDB database (MongoDB Atlas free tier works)
- An OpenRouter API key

### 1. Clone

```bash
git clone https://github.com/Annanyamishra04/Builder-AI.git
cd Builder-AI
```

### 2. Backend

```bash
cd backend
npm install
```

Create `backend/.env`:

```
PORT=3000
ORIGINS=http://localhost:5173
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_long_random_secret
OPENROUTER_API_KEY=your_openrouter_key
OPENROUTER_MODEL=openrouter/free
AI_MAX_CONCURRENCY=2
```

Start the server:

```bash
npm run dev
```

### 3. Frontend

```bash
cd frontend
npm install
```

Create `frontend/.env`:

```
VITE_BASE_URL=http://localhost:3000
```

Start the app:

```bash
npm run dev
```

Open http://localhost:5173.

## Environment Variables

| Variable | Where | Purpose |
|---|---|---|
| `MONGODB_URI` | backend | MongoDB connection string |
| `JWT_SECRET` | backend | Secret used to sign login tokens |
| `ORIGINS` | backend | Comma-separated list of allowed frontend URLs (no spaces) |
| `OPENROUTER_API_KEY` | backend | Key for calling the AI model |
| `OPENROUTER_MODEL` | backend | Model name, for example a free model from openrouter.ai/models |
| `AI_MAX_CONCURRENCY` | backend | How many files are generated at the same time |
| `VITE_BASE_URL` | frontend | Backend URL. Leave unset in production if you proxy `/api` through Vercel |

Never commit your `.env` files.

## Deployment

- **Database:** MongoDB Atlas (allow access from `0.0.0.0/0` for Render)
- **Backend:** Render web service, root directory `backend`, start command `npm start`, `NODE_ENV=production`
- **Frontend:** Vercel, root directory `frontend`. `vercel.json` proxies `/api` requests to the backend, so login cookies work across browsers.

## Notes

- Free OpenRouter models change often and vary in quality. If generation fails with a model error, pick another free model and update `OPENROUTER_MODEL`.
- Generated code can occasionally contain syntax errors. You can fix them from the code view or generate the project again.
