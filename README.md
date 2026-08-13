# Login Demo API

A small FastAPI backend for user registration and login, using SQLite for storage,
bcrypt for password hashing, and JWT bearer tokens for authenticated requests.

## Setup

```bash
pip install -r requirements.txt
uvicorn app.main:app --reload
```

The API will be at http://127.0.0.1:8000, with interactive docs at
http://127.0.0.1:8000/docs. A `app.db` SQLite file is created automatically on
first run.

## Endpoints

| Method | Path        | Description                                  | Auth required |
|--------|-------------|-----------------------------------------------|----------------|
| POST   | `/users`    | Register a new user (`email`, `password`)     | No             |
| POST   | `/login`    | Log in (form fields `username`=email, `password`); returns a JWT | No |
| GET    | `/users/me` | Get the currently authenticated user          | Yes (Bearer)   |

### Example (curl)

```bash
# Register
curl -X POST http://127.0.0.1:8000/users \
  -H "Content-Type: application/json" \
  -d '{"email": "alice@example.com", "password": "hunter22"}'

# Log in
curl -X POST http://127.0.0.1:8000/login \
  -d "username=alice@example.com&password=hunter22"

# Use the returned access_token
curl http://127.0.0.1:8000/users/me \
  -H "Authorization: Bearer <token>"
```

## Notes

- Set the `SECRET_KEY` environment variable before running in anything beyond
  local dev (`app/auth.py` falls back to an insecure dev key otherwise).
- This is a backend-only API — no HTML login page. Pair it with a frontend of
  your choice (or hit it via `/docs`).
