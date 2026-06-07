# **05 - Authentication**

## **Secure Login, JWTs, Refresh Tokens, and Best Practices**

This module covers authentication for Node.js Express applications with a focus on:

- JWT-based login and protected routes
- Token verification middleware
- Refresh token rotation and secure cookie handling
- Session-based vs stateless authentication
- Best practices for production-ready auth

## What You Will Learn

- How to register users safely with hashed passwords
- How to sign and verify JWTs
- How to protect Express routes using authentication middleware
- How to implement refresh tokens with cookie support
- How to revoke tokens and handle logout securely
- When to use JWTs, sessions, OAuth2, API keys, and MFA

## Required Setup

Before you begin, ensure you have:

- Node.js and npm installed
- An Express app scaffolded
- `dotenv` configured for environment variables
- `bcryptjs`, `jsonwebtoken`, `cookie-parser`, and `mongoose` installed if using persistence

Example `.env` keys:

```
JWT_SECRET=your_access_secret
REFRESH_TOKEN_SECRET=your_refresh_secret
NODE_ENV=development
```

## Core Authentication Flow

1. User registers with email and password.
2. Password is hashed and stored securely.
3. User logs in and receives an access token.
4. A refresh token is issued and stored in an HTTP-only cookie.
5. Protected routes verify the access token on every request.
6. When the access token expires, the client calls the refresh endpoint.
7. The server rotates refresh tokens and returns a new access token.

## Example Implementation

### 1. User Registration

- Hash passwords with bcrypt.
- Store only the hashed password.
- Keep user email and ID for lookup.

```js
const bcrypt = require('bcryptjs');
const hashedPassword = await bcrypt.hash(password, 10);
```

### 2. Login and Token Issuance

Login should:

- verify credentials
- sign a short-lived access token
- sign a long-lived refresh token
- persist refresh metadata securely
- set the refresh token as an HTTP-only cookie

Example access token:

```js
const jwt = require('jsonwebtoken');
const accessToken = jwt.sign({ id: user._id, email: user.email }, process.env.JWT_SECRET, { expiresIn: '15m' });
```

### 3. Protecting Routes with Middleware

Create middleware that:

- reads `Authorization: Bearer <token>`
- verifies the JWT
- attaches the user payload to `req.user`

Example:

```js
const jwt = require('jsonwebtoken');

function auth(req, res, next) {
  const authHeader = req.headers.authorization || '';
  const [scheme, token] = authHeader.split(' ');
  if (scheme !== 'Bearer' || !token) {
    return res.status(401).json({ message: 'Missing or invalid Authorization header' });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = { id: decoded.id, email: decoded.email };
    next();
  } catch (err) {
    if (err.name === 'TokenExpiredError') {
      return res.status(401).json({ message: 'Access token expired' });
    }
    return res.status(401).json({ message: 'Invalid token' });
  }
}

module.exports = auth;
```

### 4. Refresh Token Rotation

Refresh tokens should be:

- long-lived but not infinite
- stored in an HTTP-only cookie
- hashed before saving in the database
- rotated on each refresh request
- revoked on logout

This prevents stolen refresh tokens from being reused indefinitely.

### 5. Logout and Revocation

When logging out:

- revoke the active refresh token in the database
- clear the refresh cookie
- keep access token expiration short

## Stateless vs Stateful Authentication

- Stateless: no session stored on the server, typically JWT-based. Each request carries its own proof.
- Stateful: session stored server-side, client holds an opaque session ID (usually in a cookie).

Use stateless JWTs for APIs and mobile apps. Use stateful sessions for traditional server-rendered apps and simpler session control.

## Authentication Methods Overview

- JWT Authentication: good for SPAs, mobile apps, and microservices.
- Session-Based Authentication: good for server-side rendered apps and apps that need easy logout.
- OAuth2 / Social Login: good for third-party authentication and delegated sign-in.
- API Key: good for service-to-service or public API access.
- MFA: add extra verification for high-security scenarios.

## Best Practices

- Never store plain passwords. Always hash them.
- Use HTTPS in production.
- Keep secret keys in environment variables.
- Use short-lived access tokens.
- Use separate secrets for access and refresh tokens.
- Mark cookies as `httpOnly` and `secure` in production.
- Use `sameSite: 'strict'` for auth cookies.
- Validate and sanitize user input.
- Implement rate limiting on auth endpoints.
- Log authentication activity for auditing.

## Recommended Files Structure

- `models/user.js`
- `models/refreshToken.js`
- `routes/auth.js`
- `middleware/auth.js`
- `utils/tokens.js`

## Conclusion

This Authentication module gives you a robust foundation for building secure Express applications. It covers user registration, login, protected routes, refresh token handling, logout, and best practices. Use this as a reference to build a production-ready auth system with clean token handling and safer session management.
