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

---

# Captain Register Endpoint

**Endpoint**
- **URL**: `POST /captain/register`
- **Description**: Registers a new captain. Creates a captain record with vehicle information, hashes the password, and returns an authentication token plus the created captain (password is omitted).

**Request**
- **Content-Type**: `application/json`
- **Body fields**:
  - `fullname.firstname` (string, required) — minimum 3 characters
  - `fullname.lastname` (string, optional) — minimum 3 characters if provided
  - `email` (string, required) — must be a valid email
  - `password` (string, required) — minimum 6 characters
  - `vehicle.color` (string, required) — minimum 3 characters
  - `vehicle.plate` (string, required) — minimum 3 characters
  - `vehicle.capacity` (number, required) — minimum 1
  - `vehicle.vehicleType` (string, required) — must be one of: 'car', 'motorcycle', 'auto'

Example request body:

```json
{
  "fullname": {
    "firstname": "John",
    "lastname": "Smith"
  },
  "email": "john.smith@example.com",
  "password": "securePass123",
  "vehicle": {
    "color": "red",
    "plate": "PB 33 XY 2421",
    "capacity": 4,
    "vehicleType": "car"
  }
}
```

Example curl:

```bash
curl -X POST http://localhost:4000/captain/register \
  -H "Content-Type: application/json" \
  -d '{"fullname": {"firstname":"John","lastname":"Smith"},"email":"john.smith@example.com","password":"securePass123","vehicle":{"color":"red","plate":"PB 33 XY 2421","capacity":4,"vehicleType":"car"}}'
```

**Responses / Status Codes**
- **201 Created**: Registration successful. Response body example:

```json
{
  "token": "<jwt-token>",
  "captain": {
    "_id": "<captain-id>",
    "fullname": { "firstname": "John", "lastname": "Smith" },
    "email": "john.smith@example.com",
    "vehicle": {
      "color": "red",
      "plate": "PB 33 XY 2421",
      "capacity": 4,
      "vehicleType": "car"
    },
    "status": "inactive",
    "socketId": null
  }
}
```

- **400 Bad Request**: Validation failed (missing/invalid fields) or captain already exists. Response contains validation errors or `{ "message": "Captain already exist" }`.
- **500 Internal Server Error**: Unexpected server error.

**Notes**
- The password is hashed before storage; the returned `captain` object does not include the password field.
- Vehicle type must be one of: 'car', 'motorcycle', or 'auto'.
- Validation rules are defined in the route for `POST /captain/register`.

## Captain Login Endpoint

**Endpoint**
- **URL**: `POST /captain/login`
- **Description**: Authenticates a captain using email and password. On success returns a JWT token and the captain object (password omitted).

**Request**
- **Content-Type**: `application/json`
- **Body fields**:
  - `email` (string, required) — must be a valid email
  - `password` (string, required) — minimum 6 characters

Example request body:

```json
{
  "email": "john.smith@example.com",
  "password": "securePass123"
}
```

Example curl:

```bash
curl -X POST http://localhost:4000/captain/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john.smith@example.com","password":"securePass123"}'
```

**Responses / Status Codes**
- **200 OK**: Login successful. Response body example:

```json
{
  "token": "<jwt-token>",
  "captain": {
    "_id": "<captain-id>",
    "fullname": { "firstname": "John", "lastname": "Smith" },
    "email": "john.smith@example.com",
    "vehicle": {
      "color": "red",
      "plate": "PB 33 XY 2421",
      "capacity": 4,
      "vehicleType": "car"
    },
    "status": "inactive",
    "socketId": null
  }
}
```

- **400 Bad Request**: Validation failed (missing/invalid fields).
- **401 Unauthorized**: Invalid email or password.
- **500 Internal Server Error**: Unexpected server error.

**Notes**
- The endpoint expects a `POST` request with JSON body. A `GET /captain/login` is not supported for authentication.
- The server sets a `token` cookie on successful login in addition to returning the token in the response body.

## Captain Profile Endpoint

**Endpoint**
- **URL**: `GET /captain/profile`
- **Description**: Returns the authenticated captain's profile. This route is protected and requires a valid JWT token (cookie or `Authorization` header).

**Request**
- Provide the token either in the `Authorization` header as `Bearer <token>` or as the `token` cookie sent by the server on login.

Example curl:

```bash
curl -X GET http://localhost:4000/captain/profile \
  -H "Authorization: Bearer <jwt-token>"
```

**Responses / Status Codes**
- **200 OK**: Returns the captain's profile (example below).
- **401 Unauthorized**: No token provided, token invalid, or token blacklisted.
- **500 Internal Server Error**: Unexpected server error.

Example success body:

```json
{
  "_id": "<captain-id>",
  "fullname": { "firstname": "John", "lastname": "Smith" },
  "email": "john.smith@example.com",
  "vehicle": {
    "color": "red",
    "plate": "PB 33 XY 2421",
    "capacity": 4,
    "vehicleType": "car"
  },
  "status": "inactive",
  "socketId": null,
  "location": {
    "ltd": null,
    "lng": null
  }
}
```

## Captain Logout Endpoint

**Endpoint**
- **URL**: `GET /captain/logout`
- **Description**: Logs out the authenticated captain by clearing the `token` cookie and blacklisting the token so it cannot be reused.

**Request**
- Provide the token either in the `Authorization` header as `Bearer <token>` or as the `token` cookie.

Example curl:

```bash
curl -X GET http://localhost:4000/captain/logout \
  -H "Authorization: Bearer <jwt-token>"
```

**Responses / Status Codes**
- **200 OK**: `{ "message": "Logout successfully" }` when logout succeeds.
- **401 Unauthorized**: No token provided, token invalid, or token already blacklisted.
- **500 Internal Server Error**: Unexpected server error.

**Notes**
- After logout the token is added to a blacklist; further requests with that token will be rejected.
- These routes use the `authMiddleware.authCaptain` middleware to validate tokens.
