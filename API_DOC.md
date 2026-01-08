# Task Management API — Documentation

## Base URL
- Local: `http://localhost:3000/api`
- Production: set by DEPLOY target (see environment variables)

## Authentication
- JWT Bearer in `Authorization` header: `Authorization: Bearer <token>`
- Endpoints requiring auth are noted.

## Common headers
- `Content-Type: application/json`
- `Accept: application/json`

---

## Endpoints

### POST /auth/register
- Description: Create a new user
- Body:
    {
        "name": "string",
        "email": "user@example.com",
        "password": "string"
    }
- Response: `201` user object (id, name, email)

### POST /auth/login
- Description: Authenticate and receive JWT
- Body:
    {
        "email": "user@example.com",
        "password": "string"
    }
- Response: `200`
    {
        "token": "jwt-token",
        "user": { "id": "...", "email": "...", "name": "..." }
    }

### GET /tasks
- Auth: required
- Query params:
    - `page` (int), `limit` (int)
    - `status` (`pending|in_progress|done`)
    - `assignedTo` (user id)
    - `sortBy` (`createdAt|-createdAt|dueDate`)
- Response: `200` paginated list of tasks

### POST /tasks
- Auth: required
- Body:
    {
        "title": "string",
        "description": "string",
        "dueDate": "ISO8601",
        "priority": "low|medium|high",
        "assignedTo": "userId" (optional)
    }
- Response: `201` created task

### GET /tasks/{id}
- Auth: required
- Response: `200` task object

### PUT /tasks/{id}
- Auth: required
- Body: partial or full task fields (same as create)
- Response: `200` updated task

### DELETE /tasks/{id}
- Auth: required
- Response: `204` no content

### GET /users
- Auth: required (admin)
- Query: pagination
- Response: `200` list of users

---

## Validation & Errors
- `400` bad request (validation)
- `401` unauthorized (missing/invalid token)
- `403` forbidden (insufficient permissions)
- `404` not found
- Error format:
    {
        "error": "message",
        "details": { ... } // optional
    }

---

## Environment variables (example)
- `PORT`
- `DATABASE_URL`
- `JWT_SECRET`
- `NODE_ENV`
- `SMTP_*` (optional for email)

## Run (example)
- Install: `npm install`
- Dev: `npm run dev`
- Start: `npm start`
- Migrations: `npm run migrate`

---

## Notes
- Use HTTPS in production.
- Limit response fields for non-admin users.
- Implement rate limiting and input sanitization.
