# User Management REST API

A beginner-friendly REST API built with **Node.js + Express.js** that performs full CRUD operations on users stored in an in-memory hashmap.

---

## Project Structure

```
user-management-api/
├── server.js       ← All API logic lives here
└── package.json    ← Project metadata & dependencies
```

---

## Setup & Run

```bash
# 1. Install dependencies
npm install

# 2. Start the server
npm start

# Or with auto-restart on file changes (uses nodemon)
npm run dev
```

Server starts at: **http://localhost:3000**

---

## API Endpoints

### POST /users — Create a user
```bash
curl -X POST http://localhost:3000/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "email": "alice@example.com", "age": 28}'
```
**Response 201:**
```json
{
  "success": true,
  "data": {
    "id": "f47ac10b-58cc-4372-a567-0e02b2c3d479",
    "name": "Alice",
    "email": "alice@example.com",
    "age": 28,
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T10:30:00.000Z"
  }
}
```

---

### GET /users — Get all users
```bash
curl http://localhost:3000/users
```
**Response 200:**
```json
{
  "success": true,
  "count": 1,
  "data": [ { ...user }, { ...user } ]
}
```

---

### GET /users/:id — Get one user
```bash
curl http://localhost:3000/users/f47ac10b-58cc-4372-a567-0e02b2c3d479
```
**Response 200:** `{ "success": true, "data": { ...user } }`
**Response 404:** `{ "success": false, "errors": ["User with id ... not found."] }`

---

### PUT /users/:id — Update a user (partial update OK)
```bash
curl -X PUT http://localhost:3000/users/f47ac10b-58cc-4372-a567-0e02b2c3d479 \
  -H "Content-Type: application/json" \
  -d '{"age": 29}'
```
**Response 200:** `{ "success": true, "data": { ...updatedUser } }`

---

### DELETE /users/:id — Delete a user
```bash
curl -X DELETE http://localhost:3000/users/f47ac10b-58cc-4372-a567-0e02b2c3d479
```
**Response 200:** `{ "success": true, "message": "User \"Alice\" deleted successfully.", "data": { ...user } }`

---

## Validation Rules

| Field | Rules |
|-------|-------|
| `name` | Required on create. Non-empty string. |
| `email` | Required on create. Must match `user@domain.tld` format. Must be unique. |
| `age` | Required on create. Must be a positive integer. |

**Validation error response (400):**
```json
{
  "success": false,
  "errors": ["age must be a positive integer."]
}
```

---

## HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | Success |
| 201 | User created |
| 400 | Bad request / validation error |
| 404 | User not found |
| 500 | Unexpected server error |

---

## Important Note

Data is stored **in memory** — it resets every time the server restarts. To add persistence, replace the `users` object with a database like SQLite, PostgreSQL, or MongoDB.
