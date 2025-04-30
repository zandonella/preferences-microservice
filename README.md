# Preferences Microservice

This microservice manages user preference settings for the CatCall platform. It
allows saving and updating preferences (such as age, breed, and location radius)
and integrates with the Auth service to validate users. It also geocodes
user-supplied locations for location-aware matching.

## Notes

- This service is designed to run as part of the
  [CatCall application](https://github.com/zandonella/CatCall) and is not
  intended for standalone production use.
- MongoDB must be running and accessible using the URI defined in your `.env`
  file.
- The service queries the
  [**Auth Service**](https://github.com/zandonella/auth-microservice)
  (AUTH_SERVICE_URL) to verify that users exist before creating or updating
  preferences.

## Base Configuration

- **Base URL:** `http://localhost:<PORT>/api/preferences`
- **Default Port:** `3000`
- **Port in CatCall:** Determined by `PORT_<SERVICE>` in the root `.env`
- **Change the Port:** Set `PORT=<your_port>` in a `.env` file (see
  `.env.example`)
- **Content Type:** `application/json`
- **Response Format:** `JSON`

---

## Endpoints

> All endpoints expect and return JSON. If a server or database error occurs, a
> `500 Internal Server Error` will be returned with a generic error message.

| Method | Route                  | Description                                  |
| ------ | ---------------------- | -------------------------------------------- |
| GET    | `/api/preferences/:id` | Get or initialize a user’s saved preferences |
| PUT    | `/api/preferences/:id` | Update a user’s preferences                  |

---

### `POST /api/preferences`

**Save or update a user's preferences.**

**Request Body:**

```json
{
  "userID": "user@example.com",
  "preferences": {
    "minAge": 1,
    "maxAge": 5,
    "radius": 25,
    "breed": "Tabby",
    "sex": "Female",
    "color": "Orange"
  }
}
```

**Success Response:**

```json
{ "message": "Preferences saved" }
```

**Error Responses:**

```json
{ "error": "Missing userID or preferences" }
```

---

### `GET /api/preferences/:userID`

**Retrieve saved preferences for a specific user.**

**Success Response:**

```json
{
  "userID": "user@example.com",
  "preferences": {
    "minAge": 1,
    "maxAge": 5,
    "radius": 25,
    "breed": "Tabby",
    "sex": "Female",
    "color": "Orange"
  }
}
```

**Error Response:**

```json
{ "error": "Preferences not found" }
```

---

## Environment Setup To Run Locally

1. Copy the example environment file:

```bash
cp .env.example .env
```

2. Modify the values in `.env` as needed:

**Example `.env` contents:**

```env
PORT=3000
MONGO_URI=mongodb://localhost:27017
AUTH_SERVICE_URL=http://localhost:3001
```

---

## Running Locally (Without Docker)

Make sure [Node.js](https://nodejs.org/) is installed and MongoDB is running.

```bash
npm install
npm start
```

Expected output:

```
Connected to MongoDB successfully
Server is running on port 3000.
```

---

## Running with Docker

You can also run this microservice in isolation using Docker:

### 1. Build the image

```bash
docker build -t preferences-microservice .
```

### 2. Run the container

```bash
docker run -p 3000:3000 --env-file .env preferences-microservice
```

> ⚠️ Ensure your `.env` file is in the root and includes `MONGO_URI` and
> `AUTH_SERVICE_URL`.

---
