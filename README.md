<div align="center">
	<h1>Smart Library Management System</h1>
	<p>Role-based library operations with dashboards for admins, librarians, and students.</p>
</div>

## Live App

- Base URL: https://e2e-smart-library.vercel.app/
- Student login: https://e2e-smart-library.vercel.app/
- Student signup request: https://e2e-smart-library.vercel.app/request-signup
- Librarian login: https://e2e-smart-library.vercel.app/librarian/login
- Librarian signup request: https://e2e-smart-library.vercel.app/librarian/request-signup
- Admin login: https://e2e-smart-library.vercel.app/admin/login
- Admin signup: https://e2e-smart-library.vercel.app/admin/signup

## Overview

The project pairs a React + Vite frontend with an Express + MongoDB backend. It supports multi-role access, circulation workflows (borrowing, returns, reservations), signup approvals, overdue notifications, and analytics dashboards.

## Project Structure

```
backend/   Express API + MongoDB models
frontend/  React + TypeScript + Vite web client
```

## Features

- Role-based portals for admin, librarian, and student journeys
- Borrow, return, and reservation flows with availability tracking
- Signup request/approval for students and librarians
- Overdue detection with fine calculation and daily notifications
- Analytics dashboards (trends, top books, fines, recent activity)
- JWT auth with interceptor-driven token handling on the client

## Tech Stack

- Frontend: React 19, TypeScript, Vite, React Router, Axios, Recharts
- Backend: Express, MongoDB/Mongoose, JSON Web Tokens, dotenv, CORS

## Getting Started

### Prerequisites

- Node.js 18+
- MongoDB database (Atlas or local)

### Backend Setup

1) Install dependencies
```
cd backend
npm install
```

2) Add a `.env` file (sample values shown)
```
PORT=5000
MONGODB_URI=mongodb+srv://<user>:<pass>@<cluster>/<db>
JWT_SECRET=change-me

ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=adminpass
LIBRARIAN_EMAIL=librarian@example.com
LIBRARIAN_PASSWORD=librarianpass
STUDENT_EMAIL=student@example.com
STUDENT_PASSWORD=studentpass

# Optional overrides
ADMIN_ID=admin
LIBRARIAN_ID=librarian1
STUDENT_ID=student1
```

3) Run the API (defaults to port 5000)
```
npm start
```

Notes: On startup the server ensures default users/students exist in MongoDB and keeps a local `users.json` for quick auth seeding. JWTs are signed with `JWT_SECRET`.

### Frontend Setup

1) Install dependencies
```
cd frontend
npm install
```

2) Configure the API base URL (optional if using the hosted backend)
```
# frontend/.env
VITE_API_BASE_URL=http://localhost:5000
```
If unset, the client falls back to the hosted API at https://e2e-backend-1zjm.onrender.com.

3) Run the dev server
```
npm run dev
```

## Deployment

- Frontend: Vercel (see `frontend/vite.config.ts` for base config)
- Backend: Any Node host with MongoDB connectivity (Render/Heroku/etc.). Provide the `.env` values above.

## Additional References

- API overview: backend/API_END_POINTS.md
- Data models: backend/SCHEMAS.md

---
Created for quick GitHub onboarding. Feel free to tailor copy and URLs to your deployment.
