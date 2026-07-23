# Complete English

A full-stack English learning and quiz application rebuilt from the original
Streamlit project with:

- React + Vite frontend
- Node.js + Express backend
- RethinkDB persistence
- HTTP-only JWT session cookies
- Docker and Railway-ready production packaging

The two original courses, 41 learning modules, 38 challenge rounds, lesson
content, questions, explanations, and lesson images are included.

## Preserved learning behavior

- Register and sign in with a username and password.
- Choose either English course.
- Complete learning modules in sequence.
- Score 100% to pass and unlock the next module.
- Complete challenge rounds in sequence with the same perfect-score rule.
- Review per-question answers and explanations after submission.
- Keep attempts, high scores, pass status, and separate progress per course.
- Review progress from the learner profile.
- Restore and refresh a two-hour login session through secure cookies.

Quiz answers are graded by the backend and are not sent to the browser.
Unlock rules are enforced by both the interface and the API.

## Project structure

```text
Complete_English/
├── backend/
│   ├── data/                 # Migrated curricula and question banks
│   ├── src/
│   │   ├── config/           # RethinkDB connection
│   │   ├── init/             # Database/table/index initialization
│   │   ├── logic/            # Authentication, course, and grading logic
│   │   ├── models/           # Progress persistence
│   │   └── routes/           # REST API
│   └── docker-compose.yml
├── frontend/
│   ├── public/assets/        # Migrated lesson artwork
│   └── src/                  # React pages and components
├── .github/workflows/ci.yml  # Main-branch CI gate
├── deploy/railway/           # Railway setup and variable template
├── Dockerfile
└── railway.json
```

## Run locally

Prerequisites: Node.js 22+, npm, Docker Desktop.

1. Start RethinkDB:

   ```powershell
   docker compose -f backend/docker-compose.yml up -d
   ```

2. Create the backend environment file:

   ```powershell
   Copy-Item backend/.env.example backend/.env
   ```

   Replace both JWT secrets in `backend/.env` before using the app outside
   local development.

3. Install dependencies if needed:

   ```powershell
   npm run install:all
   ```

4. Start the React frontend and Node backend:

   ```powershell
   npm run dev
   ```

5. Open `http://localhost:5173`.

RethinkDB's local administration page is available at
`http://localhost:39080`.

## Production

Build the frontend:

```powershell
npm run build
```

The root Dockerfile builds the React application and serves it from the Node
backend on port `8000`. In production, set:

- `RETHINKDB_SERVERS`
- `RETHINKDB_DB`
- `JWT_ACCESS_SECRET`
- `JWT_REFRESH_SECRET`
- `CORS_ORIGIN` when the frontend and API use different origins

For the complete two-service Railway setup and CI/CD instructions, see
[`deploy/railway/README.md`](deploy/railway/README.md).

## Validation

```powershell
npm run check
```

This checks the backend source, lints the React source, and creates a production
frontend build.
