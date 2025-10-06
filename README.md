## GoalSetter App

Full‑stack MERN application to register/login users and create, read, update, and delete personal goals. This repository contains a Node/Express + MongoDB backend and a React + Redux Toolkit frontend (Create React App).

### Tech stack
- **Backend**: Node.js, Express, Mongoose, JSON Web Tokens, bcryptjs
- **Frontend**: React 19, Redux Toolkit, React Router, Axios, React Toastify
- **Tooling**: Nodemon, Concurrently, CRA

### Monorepo structure
```
backend/
  config/
  controller/
  middleware/
  models/
  routes/
  server.js
frontend/
  src/
  public/
  package.json
package.json (root)
```

---

## Getting started

### Prerequisites
- Node.js 18+ (recommended)
- MongoDB database (MongoDB Atlas or local)

### Installation
1. Install root and frontend dependencies:
```bash
npm install
cd frontend && npm install
```

2. Create backend environment file `backend/.env`:
```bash
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_long_random_secret
PORT=5000
```

Notes:
- The frontend `package.json` includes `"proxy": "http://localhost:5000"`, so API requests during development are proxied to the backend.

### Run the app (development)
- Start backend and frontend together (concurrently):
```bash
npm run dev
```

- Start only the backend (Express API):
```bash
npm run server   # with nodemon
# or
npm start        # plain node
```

- Start only the frontend (React dev server):
```bash
npm run client
```

### Build the frontend (production bundle)
```bash
cd frontend
npm run build
```

---

## Environment variables
Create `backend/.env` with:
```bash
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_long_random_secret
PORT=5000
```

Required values:
- **MONGO_URI**: MongoDB connection string used by Mongoose
- **JWT_SECRET**: Secret for signing JSON Web Tokens
- **PORT**: Port for the API server (defaults to 5000 if not set)

---

## Available scripts

### Root scripts (from `package.json`)
- `npm start` — `node backend/server.js`
- `npm run server` — `nodemon backend/server.js`
- `npm run client` — `npm start --prefix frontend`
- `npm run dev` — runs backend and frontend concurrently

### Frontend scripts (from `frontend/package.json`)
- `npm start` — start CRA dev server
- `npm run build` — build production assets
- `npm test` — run tests via CRA
- `npm run eject` — eject CRA (irreversible)

---

## API reference
Base URL (dev): `http://localhost:5000`

### Authentication
- JWT returned on register/login. Include it in subsequent requests as:
  - Header: `Authorization: Bearer <token>`

### Users
- **Register** — `POST /api/users`
  - Body:
    ```json
    { "name": "Jane Doe", "email": "jane@example.com", "password": "strongpassword" }
    ```
  - Response:
    ```json
    { "_id": "...", "name": "Jane Doe", "email": "jane@example.com", "token": "<jwt>" }
    ```

- **Login** — `POST /api/users/login`
  - Body:
    ```json
    { "email": "jane@example.com", "password": "strongpassword" }
    ```
  - Response:
    ```json
    { "_id": "...", "name": "Jane Doe", "email": "jane@example.com", "token": "<jwt>" }
    ```

- **Get current user** — `GET /api/users/me` (Protected)
  - Headers: `Authorization: Bearer <token>`
  - Response: authenticated user object

### Goals (Protected)
All endpoints require `Authorization: Bearer <token>`

- **List goals** — `GET /api/goals`
  - Returns all goals owned by the authenticated user

- **Create goal** — `POST /api/goals`
  - Body:
    ```json
    { "text": "Learn Redux Toolkit" }
    ```
  - Response: created goal

- **Update goal** — `PUT /api/goals/:id`
  - Body can include any updatable fields, e.g.:
    ```json
    { "text": "Learn Redux Toolkit deeply" }
    ```
  - Response: updated goal

- **Delete goal** — `DELETE /api/goals/:id`
  - Response:
    ```json
    { "id": "<deletedId>" }
    ```

### Data models
- **User**: `{ name, email (unique), password (hashed) }` plus timestamps
- **Goal**: `{ text, user (ObjectId ref User) }` plus timestamps

### Error handling
Errors are centralized via Express error middleware and consistent HTTP status codes (e.g., `400`, `401`).

---

## Frontend overview
- React app using Redux Toolkit slices for auth and goals
- Axios for API calls to the proxied backend
- React Router for navigation; React Toastify for notifications

Development URL: `http://localhost:3000` (via CRA)

---

## Security notes
- Passwords are salted and hashed with `bcryptjs`
- JWT expires in 30 days by default
- Protected routes require a valid bearer token

---

## License
MIT © Goutham Balaji P S


