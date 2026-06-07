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

> Next steps: If you want, I can add detailed starter code for any exercise (e.g., a minimal Express app with `/register`, `/login`, and `authenticateToken`).
