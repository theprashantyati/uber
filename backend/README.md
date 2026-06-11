# User Register Endpoint

**Endpoint**
- **URL**: `POST /user/register`
- **Description**: Registers a new user. Creates a user record, hashes the password, and returns an authentication token plus the created user (password is omitted).

**Request**
- **Content-Type**: `application/json`
- **Body fields**:
  - `fullname.firstname` (string, required) — minimum 3 characters
  - `fullname.lastname` (string, optional) — minimum 3 characters if provided
  - `email` (string, required) — must be a valid email
  - `password` (string, required) — minimum 6 characters

Example request body:

```json
{
  "fullname": {
    "firstname": "Jane",
    "lastname": "Doe"
  },
  "email": "jane.doe@example.com",
  "password": "securePass123"
}
```

Example curl:

```bash
curl -X POST http://localhost:3000/user/register \
  -H "Content-Type: application/json" \
  -d '{"fullname": {"firstname":"Jane","lastname":"Doe"},"email":"jane.doe@example.com","password":"securePass123"}'
```

**Responses / Status Codes**
- **201 Created**: Registration successful. Response body example:

```json
{
  "token": "<jwt-token>",
  "user": {
    "_id": "<user-id>",
    "fullname": { "firstname": "Jane", "lastname": "Doe" },
    "email": "jane.doe@example.com",
    "socketId": null
  }
}
```

- **400 Bad Request**: Validation failed (missing/invalid fields) or user already exists. Response contains validation errors or `{ "message": "User already exist" }`.
- **500 Internal Server Error**: Unexpected server error.

**Notes**
- The password is hashed before storage; the returned `user` object does not include the password field.
- Validation rules are defined in the route for `POST /user/register`.

## User Login Endpoint

**Endpoint**
- **URL**: `POST /users/login`
- **Description**: Authenticates a user using email and password. On success returns a JWT token and the user object (password omitted).

**Request**
- **Content-Type**: `application/json`
- **Body fields**:
  - `email` (string, required) — must be a valid email
  - `password` (string, required) — minimum 6 characters

Example request body:

```json
{
  "email": "jane.doe@example.com",
  "password": "securePass123"
}
```

Example curl:

```bash
curl -X POST http://localhost:4000/users/login \
  -H "Content-Type: application/json" \
  -d '{"email":"jane.doe@example.com","password":"securePass123"}'
```

**Responses / Status Codes**
- **200 OK**: Login successful. Response body example:

```json
{
  "token": "<jwt-token>",
  "user": {
    "_id": "<user-id>",
    "fullname": { "firstname": "Jane", "lastname": "Doe" },
    "email": "jane.doe@example.com",
    "socketId": null
  }
}
```

- **400 Bad Request**: Validation failed (missing/invalid fields).
- **401 Unauthorized**: Invalid email or password.
- **500 Internal Server Error**: Unexpected server error.

**Notes**
- The endpoint expects a `POST` request with JSON body. A `GET /users/login` is not supported for authentication.
- The server sets a `token` cookie on successful login in addition to returning the token in the response body.

## User Profile Endpoint

**Endpoint**
- **URL**: `GET /users/profile`
- **Description**: Returns the authenticated user's profile. This route is protected and requires a valid JWT token (cookie or `Authorization` header).

**Request**
- Provide the token either in the `Authorization` header as `Bearer <token>` or as the `token` cookie sent by the server on login.

Example curl:

```bash
curl -X GET http://localhost:4000/users/profile \
  -H "Authorization: Bearer <jwt-token>"
```

**Responses / Status Codes**
- **200 OK**: Returns the user's profile (example below).
- **401 Unauthorized**: No token provided, token invalid, or token blacklisted.
- **500 Internal Server Error**: Unexpected server error.

Example success body:

```json
{
  "_id": "<user-id>",
  "fullname": { "firstname": "Jane", "lastname": "Doe" },
  "email": "jane.doe@example.com",
  "socketId": null
}
```

## User Logout Endpoint

**Endpoint**
- **URL**: `GET /users/logout`
- **Description**: Logs out the authenticated user by clearing the `token` cookie and blacklisting the token so it cannot be reused.

**Request**
- Provide the token either in the `Authorization` header as `Bearer <token>` or as the `token` cookie.

Example curl:

```bash
curl -X GET http://localhost:4000/users/logout \
  -H "Authorization: Bearer <jwt-token>"
```

**Responses / Status Codes**
- **200 OK**: `{ "message": "Logged out" }` when logout succeeds.
- **401 Unauthorized**: No token provided, token invalid, or token already blacklisted.
- **500 Internal Server Error**: Unexpected server error.

**Notes**
- After logout the token is added to a blacklist; further requests with that token will be rejected.
- These routes use the `authMiddleware.authUser` middleware to validate tokens.
