# 🟦 Module 05 — Authentication: Exercises & Answers

> 💡 **How to use this file:**
> - Work through each problem in a separate project or branch.
> - Try before checking answers. Explanations are where the learning happens.
> - Run Node examples with `node filename.js` and use Postman/curl for HTTP exercises.

---

## 📖 Table of Contents

1. [Problems & Challenges](#problems--challenges)
2. [Exercises](#exercises)
3. [Additional Exercises](#additional-exercises)
4. [Self-Review Quiz](#self-review-quiz)

---

## Problems & Challenges

### Module 1: Foundational Password Security & Hashing
- Plaintext Rejection: `/register` must reject weak plaintext passwords (min 8 chars, include number and special char).
- Async Hashing: Hash passwords with `bcrypt` (`saltRounds = 12`) asynchronously.
- Login Verification: Use `bcrypt.compare()` and return a generic 401 on failure to avoid user enumeration.
- Adaptive Salting: Migrate old weak hashes to bcrypt/argon2 on next login.
- Slow-Down Rate Limiter: Use `express-rate-limit` to slow repeated failed logins (e.g., 2s delay after 3 failures).

### Module 2: Sessions (Cookies & express-session)
- Memory store prototype for sessions.
- Secure cookie flags: `httpOnly`, `secure`, `sameSite: 'lax'`.
- Absolute session timeout (30min) and rolling extension on activity.
- `/logout` should destroy server session and clear cookie.

### Module 3: JWT Architecture
- Stateless token signing (`jwt.sign`) with `expiresIn: '15m'`.
- `isAuth` middleware to extract `Authorization: Bearer <token>` and `jwt.verify()`.
- Handle `TokenExpiredError` explicitly (return 401 + { error: "TokenExpired" }).
- Optionally support RS256 (asymmetric) signing with keypairs.

### Module 4: Refresh Tokens & Rotation
- Return `accessToken` (15m) and `refreshToken` (7d httpOnly cookie).
- `/refresh` validates cookie against whitelist and issues new access token.
- Rotate and whitelist refresh tokens; revoke on logout.

### Module 5: OAuth 2.0 & Social Login (Passport)
- Integrate `passport-google-oauth20` and `passport-github2`.
- Implement callback routes, state parameter protection, and minimal scopes.

### Module 6: MFA / 2FA
- Implement TOTP setup (`/mfa/setup`) and verify (`/mfa/verify`) using `speakeasy` or `otplib`.
- Provide QR URI for authenticator apps and backup codes hashed with bcrypt.

### Module 7–10: Advanced Topics
- RBAC/ABAC middlewares, security hardening (`helmet`, CSP), email verification, password reset flows, API keys, centralized auth gateway and JWT blacklisting (Redis), and microservices design patterns.

---

## Problem Solutions

### Module 1: Foundational Password Security & Hashing
- Validate password strength before registration, rejecting weak passwords with a 400 response.
- Hash passwords with `bcrypt.hash(password, 12)` and store only the hash.
- Verify login with `bcrypt.compare(password, storedHash)` and return a generic 401 on failure.
- Detect weak legacy hashes at login and re-hash the password using a strong algorithm.
- Add `express-rate-limit` to slow repeated failed login attempts.

### Module 2: Sessions (Cookies & express-session)
- Configure `express-session` with a secure store; do not use the default memory store in production.
- Use `cookie: { httpOnly: true, secure: true, sameSite: 'lax' }` and set `rolling: true`.
- Store the session user ID in `req.session.userId` after login.
- Protect private routes by checking `req.session.userId` and call `req.session.destroy()` on logout.
- Enforce a maximum session lifetime of 30 minutes.

### Module 3: JWT Architecture
- Issue access tokens with `jwt.sign(payload, process.env.JWT_SECRET, { expiresIn: '15m' })`.
- Build middleware that reads `Authorization: Bearer <token>`, verifies with `jwt.verify`, and attaches `req.user`.
- Handle `TokenExpiredError` separately and return an explicit 401 error.
- Optionally support RS256 by using private/public key pairs for signing and verification.

### Module 4: Refresh Tokens & Rotation
- Issue a short-lived access token and a long-lived refresh token stored as an `httpOnly` cookie.
- Maintain a whitelist or token store to validate refresh tokens.
- On `/refresh`, verify and rotate the refresh token: issue a new refresh token and invalidate the previous one.
- Remove refresh tokens from the store on logout and when passwords are reset.

### Module 5: OAuth 2.0 & Social Login (Passport)
- Configure Passport strategies for Google and GitHub with client ID/secret.
- Use `/auth/google` to start the flow and `/auth/google/callback` as the redirect URI.
- In the callback, find or create a user record by email and serialize the user into the session.
- Keep the state parameter to protect against CSRF during OAuth login.

### Module 6: MFA / 2FA
- Generate a TOTP secret with `speakeasy.generateSecret()` and save it securely with the user.
- Provide a QR code URL to the frontend and verify codes with `speakeasy.totp.verify()`.
- Enable MFA only after a successful verification step and store backup codes hashed with bcrypt.
- Require the second factor during login when MFA is enabled.

### Module 7–10: Advanced Topics
- Use role-based middleware to grant `admin` or `editor` access and enforce ownership checks.
- Harden security with `helmet`, strict CSP, CORS allowlists, and cache-control headers.
- Add email verification, password reset workflows, and invalidate refresh tokens after sensitive changes.
- For microservices or gateway patterns, centralize auth logic and use token blacklisting or a revocation store where needed.

---

## Exercises

Below are practical exercises and challenges. Try them before reading sample answers.

### Exercise 1 — Safe Password Storage (Beginner)
Task:
- Build a `/register` POST route that accepts `email` and `password`.
- Hash the password asynchronously with `bcrypt` (saltRounds = 12) and store the user.

Test:
- Print stored users to verify only hashed passwords are present.

### Exercise 2 — Basic JWT Issuance & Login (Beginner)
Task:
- Implement `/login` POST to validate credentials with `bcrypt.compare()`.
- On success return an `accessToken` (JWT) signed with `process.env.JWT_SECRET`.

Test:
- Wrong password → HTTP 401. Correct password → JSON `{ token: "..." }`.

### Exercise 3 — Route Guard Middleware (Intermediate)
Task:
- Implement `authenticateToken(req, res, next)` that reads `Authorization: Bearer <token>` and verifies it.
- Attach decoded payload to `req.user`.

Test:
- Protect `/dashboard` route; ensure requests without valid token are rejected and valid tokens pass.

### Exercise 4 — Refresh Tokens & Rotation (Advanced)
Task:
- Return `accessToken` (15m) and `refreshToken` (7d httpOnly cookie) from `/login`.
- Implement `/refresh` to validate the refresh token from the cookie against a whitelist, rotate it, and issue a new access token.

Test:
- Logout should remove the refresh token from the whitelist so cookie can no longer be exchanged.

### Exercise 5 — MFA (Advanced)
Task:
- Implement `/2fa/setup` to generate a TOTP secret and provide a QR URI.
- Implement `/2fa/verify` to accept a 6-digit code and enable MFA for the user.

Test:
- Use an authenticator app (Google Authenticator, Authy) to verify codes.

---

## Additional Exercises

### Exercise 6 — Session-Based Authentication
Task:
- Build an Express app using `express-session`.
- Create `/login`, `/logout`, and `/profile` routes.
- Store the session ID server-side and protect `/profile` with session checking.

Test:
- Login should create a session cookie.
- Logout should destroy the session and reject access to `/profile`.

### Exercise 7 — OAuth / Social Login
Task:
- Add Google OAuth with `passport-google-oauth20`.
- Create `/auth/google` and `/auth/google/callback` routes.
- On successful login, create or match a user record by email.

Test:
- Visit `/auth/google` and confirm redirect to Google.
- After authentication, ensure user data is returned or stored.

### Exercise 8 — RBAC & Permissions
Task:
- Add role-based middleware `checkRole(['admin', 'editor'])`.
- Protect a route like `/admin/dashboard` so only admin access is allowed.
- Add ownership logic so users can only edit their own profile unless admin.

Test:
- A user with role `user` should be denied admin access.
- A user should be allowed to update their own profile.

### Exercise 9 — Account Lifecycle Workflows
Task:
- Implement `/forgot-password` and `/reset-password` routes.
- Generate a short-lived reset token and store a hash in the user record.
- Invalidate existing refresh tokens when the password is reset.

Test:
- Request password reset and verify a token is generated.
- Use `/reset-password` with the token and a new password.

### Exercise 10 — Security Hardening
Task:
- Add `helmet`, `cors`, and cache-control headers for protected routes.
- Reject wildcard origins and allow only a trusted frontend domain.
- Add input sanitization to protect against injection attacks.

Test:
- Confirm `X-Powered-By` is removed and CORS policies are enforced.
- Confirm protected responses include `Cache-Control: no-store`.

---

## Sample Solutions

### Solution 1 — Safe Password Storage
- Use `bcrypt.hash(password, 12)` before saving.
- Store only `{ email, passwordHash }` in the user store.
- Validate with `await bcrypt.compare(req.body.password, user.passwordHash)`.

### Solution 2 — Basic JWT Issuance
- On login, verify credentials with `bcrypt.compare`.
- Sign token with `jwt.sign({ id: user.id }, process.env.JWT_SECRET, { expiresIn: '15m' })`.
- Return `{ token }` in JSON.

### Solution 3 — Route Guard Middleware
- Read `Authorization` header and split on `Bearer`.
- Use `jwt.verify(token, process.env.JWT_SECRET)`.
- Attach decoded payload to `req.user` and call `next()`.
- Return 401 for missing, malformed, or invalid tokens.

### Solution 4 — Refresh Token Rotation
- Store refresh tokens in a whitelist or database table.
- Issue cookie with `httpOnly`, `secure`, and `sameSite: 'lax'`.
- On `/refresh`, verify the refresh token and replace it with a new one.
- Delete the old refresh token from the whitelist on logout.

### Solution 5 — MFA Setup
- Generate a TOTP secret and save it to the user record.
- Return the QR URI or secret to the frontend.
- Verify codes using `speakeasy.totp.verify({ secret, token })`.
- Flag the user as `mfaEnabled = true` after successful verification.

### Solution 6 — Session-Based Auth
- Use `express-session` with a secure session store.
- On login, set `req.session.userId = user.id`.
- Protect `/profile` with middleware that checks `req.session.userId`.
- Call `req.session.destroy()` on logout.

### Solution 7 — OAuth / Social Login
- Configure Passport with `GoogleStrategy`.
- Use `/auth/google` to start authentication and `/auth/google/callback` for the redirect.
- In the callback, find or create a user by email and call `done(null, user)`.
- Serialize the user into the session.

### Solution 8 — RBAC & Permissions
- Add a middleware `checkRole(allowedRoles)` that verifies `req.user.role`.
- Protect admin routes with `checkRole(['admin'])`.
- For profile updates, compare `req.user.id === req.params.id` or allow admin override.

### Solution 9 — Account Lifecycle Workflows
- Generate a reset token and store its hash with expiration.
- Send the unhashed token via email link or log it for test purposes.
- Verify token on `/reset-password`, then hash the new password and clear the reset token.
- Revoke refresh tokens connected to the user when password changes.

### Solution 10 — Security Hardening
- Add `app.use(helmet())` and `app.use(cors({ origin: trustedOrigin }))`.
- Set `res.set('Cache-Control', 'no-store')` on sensitive routes.
- Remove `X-Powered-By`, enable strict CORS origins, and validate/sanitize inputs.

---

## Self-Review Quiz
Find the security flaw in this snippet:

```javascript
// app.js snippet
app.post('/login', async (req, res) => {
  const user = await db.findUser(req.body.email);
  
  if (user && req.body.password === user.password) {
    const token = jwt.sign({ id: user.id }, "MY_SECRET_123");
    return res.json({ token });
  }
  res.status(401).send('Invalid');
});
```

Answer:
- The code compares plaintext passwords and uses a hardcoded secret. Use `bcrypt.compare()` for verification and read secrets from `process.env`.

---
