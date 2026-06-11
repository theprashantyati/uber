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
