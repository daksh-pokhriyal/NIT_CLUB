# NIT Club Compass

## Quick Start

### 1. Backend
```powershell
cd backend
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate    # Mac / Linux
pip install -r requirements.txt
python seed_data.py
uvicorn server:app --host 0.0.0.0 --port 8001 --reload
```

### 2. Frontend (new terminal)
```powershell
cd frontend
python -m http.server 3000
```

Open **http://localhost:3000**

---

## Features
- Netflix-inspired Dark UI
- Club Discovery and Comparisons
- Events & Forum
- Staff & Owner Admin Portals
- **Forgot Password (OTP via EmailJS)**

---

## Accounts

| Role       | Credentials / How to sign up |
|------------|----------------|
| Student    | Any @nitkkr.ac.in email + sign up directly |
| Senior     | Same + secret key: `SENIOR2024SECRET` |
| Club Admin | Created via Admin Portal |
| Faculty    | Created via Admin Portal |
| Owner (Admin) | Email: `raunaktiwari1729@gmail.com` <br> Password: `raunak123` |

(Owner can log in and visit the Admin Panel to create accounts for Club Admins and Faculty In-charge).

---

## Environment Variables (optional)
```
MONGO_URL=mongodb+srv://...
JWT_SECRET_KEY=any-long-secret
GROQ_API_KEY=gsk_...          ← enables AI chatbot
OWNER_EMAIL=your@email.com
```

---

## Bugs Fixed (v6 → v7)

| # | File | Bug | Fix |
|---|------|-----|-----|
| 1 | server.py | `datetime.utcnow()` deprecated — crashes on Python 3.12 | Replaced with `datetime.now(timezone.utc)` throughout |
| 2 | server.py | `GET /api/dm/{user_id}` registered **before** `/api/dm/threads/list` and `/api/dm/unread/count` — FastAPI matched "threads"/"unread" as user IDs | Reordered routes: specific paths first |
| 3 | server.py | `GET /api/posts/v2` registered **after** `GET /api/posts/{post_id}` — FastAPI matched "v2" as a post_id | Moved `/posts/anon` and `/posts/v2` before `/{post_id}` |
| 4 | server.py | `datetime.fromisoformat()` comparison crashed with timezone-aware strings | Added `_safe_dt()` helper for robust parsing |
| 5 | forum.html | Called `api.postsV2(q)` which **does not exist** in api.js — forum never loaded posts | Changed to `api.posts(q)` |
| 6 | All pages | AI Chat widget only initialised on `index.html` — missing on 10 other pages | Added `initChatWidget()` to every page |
| 7 | All pages | Cache-bust version `?v=6` inconsistent | Bumped to `?v=7` uniformly |
| 8 | messages.html | Search-results dropdown had no `position:relative` parent — appeared in wrong position | Added `position:relative` to wrapper div |
| 9 | All pages | No favicon | Added emoji favicon to all pages |
| 10 | forum.html | `setPostType()` used bare global `event` variable — breaks in strict mode / Firefox | Passed `event` explicitly through `onclick` |
