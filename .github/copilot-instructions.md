# Copilot instructions for this repository

Quick orientation
- This is a small FastAPI app located at `src/app.py` that mounts a static frontend from `src/static`.
- Data is stored in-memory in the `activities` dict inside `src/app.py` (no DB). Changes reset on restart.

What to edit and why
- Backend endpoints: `GET /activities` and `POST /activities/{activity_name}/signup` are defined in `src/app.py`.
  - `signup_for_activity` appends the provided `email` to the `participants` list; there is no max-enforcement logic.
- Frontend JS in `src/static/app.js` fetches `/activities` and calls the signup endpoint with `fetch('/activities/.../signup?email=...')`.
  - If you change endpoint names or query parameters, update `src/static/app.js` accordingly.

Run & debug (how maintainers run the project)
- Install deps: `pip install -r requirements.txt` (or `pip install fastapi uvicorn`).
- Start server (preferred):
  - `uvicorn src.app:app --reload --host 0.0.0.0 --port 8000` or `python -m uvicorn src.app:app --reload`
  - Note: `src/README.md` suggests `python app.py`, but `src/app.py` has no `if __name__ == '__main__'` runner — use `uvicorn`.
- Static UI: open `http://localhost:8000/static/index.html` (root `/` redirects there).

Project-specific conventions & gotchas
- Files live under `src/` (backend) and `src/static/` (frontend). Keep imports relative to `src`.
- The app mounts `StaticFiles` at `/static` using the folder `src/static` — preserve that path when moving static assets.
- Tests: there are no tests included; `pytest.ini` sets `pythonpath = .` for convenience if tests are added.

When making changes
- If you modify data shape (the `activities` dict structure), update both `src/app.py` and how `src/static/app.js` reads fields
  (for example `max_participants`, `participants` are used directly in the UI).
- For dependencies, update `requirements.txt` so contributors can install them reproducibly.

Examples of useful edits
- Add validation on signup: check `max_participants` before appending to `participants` (edit `signup_for_activity`).
- Add a persistence layer: replace `activities` with a DB or file-backed store and wire up CRUD operations.
- Improve frontend UX: reflect signup failures using the JSON `detail` field returned by FastAPI errors.

Where to look
- Backend: `src/app.py`
- Frontend: `src/static/index.html`, `src/static/app.js`, `src/static/styles.css`
- Project docs: `README.md`, `src/README.md`, `requirements.txt`

If anything here is unclear, tell me which area (backend, frontend, run commands) you'd like expanded and I'll iterate.
