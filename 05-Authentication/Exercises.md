
# 🟦 Module 05 — Authentication: Exercises & Answers

> 💡 **How to use this file:**
> - Work through each problem in a separate project or branch.
> - Try before checking answers. Explanations are where the learning happens.
> - Run Node examples with `node filename.js` and use Postman/curl for HTTP exercises.

---

## 📖 Table of Contents

1. [Problems & Challenges](#problems--challenges)
2. [Exercises](#exercises)

---

## Problems & Challenges

Module 1: Foundational Password Security & Hashing

Plaintext Rejection: Build a `/register` endpoint that returns a 400 Bad Request if a password contains fewer than 8 characters, lacks a number, or lacks a special character.

Async Hashing: Hash user passwords with bcrypt using an optimal workload factor (saltRounds = 12). Avoid synchronous methods (`hashSync`) to prevent blocking the Node event loop.

Login Verification: Implement `/login` using `bcrypt.compare()`. Ensure it returns a generic 401 Unauthorized message for both non-existent emails and wrong passwords to prevent user enumeration.

Adaptive Salting: Write a migration script that identifies user records hashed with old algorithms (like MD5 or SHA1) and updates them to modern bcrypt or argon2 hashes upon their next successful login.

Slow-Down Rate Limiter: Use `express-rate-limit` on `/login`. Force a 2-second delay on response delivery after 3 consecutive failed attempts from the same IP address.

Module 2: State-Based Sessions (Cookies & Express-Session)

Memory Store Setup: Implement session-based authentication using `express-session`. Store active session IDs in a server-side memory store array.

Secure Cookie Configuration: Configure your session cookie flags explicitly: set `httpOnly: true` to stop XSS extraction, `secure: true` for HTTPS enforcement, and `sameSite: 'lax'` to mitigate CSRF.

Session Lifetime Expiry: Configure an absolute session timeout of 30 minutes (`maxAge`). Force the server to destroy the session and wipe cookies once this window closes.

Dynamic Session Rolling: Implement rolling sessions. Extend the session expiration time by another 15 minutes automatically every time an authenticated user interacts with the API.

The Absolute Logout: Build a `/logout` endpoint that runs `req.session.destroy()` and clears the client-side session cookie simultaneously. Verify that clicking "Back" in the browser doesn't expose data.

*(Remaining modules and problems continue below — original content preserved and reformatted into sections.)*

---

## Exercises

Follow the practical exercises and problems above. Each exercise section below contains starter tasks and suggested tests. Answers and explanations are included in collapsible details blocks where applicable in the original file.




🧠 Problem 1: The "Case-Sensitive" Account LockoutThe Scenario: A user registers an account with the email Student@School.edu. The next day, they try to log in using student@school.edu.The Code Trap: A standard database query like db.findOne({ email: req.body.email }) will fail in many databases (like PostgreSQL or strict MongoDB configurations) because strings are case-sensitive. The user is locked out of their own account.The Student Challenge:Fix the registration and login routes so that emails are completely case-insensitive.The "Think About It" Question: Should you modify the data before saving it to the database, or should you use complex regex search queries during login? Which is faster and more secure?🧩 Problem 2: The Silent Route Crash (Error Handling)The Scenario: A student writes a beautiful JWT validation middleware. It works perfectly when a valid token is provided. However, when an attacker sends a random, malformed string in the headers, the entire Node.js backend server crashes.The Code Trap: jwt.verify(token, secret) throws a synchronous error if the token is invalid or malformed. If it is not wrapped inside a proper code block, it bubbles up and terminates the Node.js process.The Student Challenge:Write an Express middleware that gracefully handles missing tokens, expired tokens, and completely malformed/gibberish tokens without ever crashing the server.The "Think About It" Question: What specific HTTP status code should you return for a missing token versus an expired token? Why does it matter to the frontend developer?🛑 Problem 3: The "Timing Attack" Information LeakThe Scenario: Your /login route looks up a user. If the user doesn't exist, it immediately returns "User not found". If the user does exist, it runs bcrypt.compare(), which takes about 200–500 milliseconds to compute.The Code Trap: An attacker can write a script to measure exactly how many milliseconds the server takes to respond. If a response takes 2ms, they know the email doesn't exist. If it takes 300ms, they know the email is real. They can use this to harvest a list of valid user accounts.The Student Challenge:Rewrite the login logic so that whether an email exists or not, the server takes roughly the same amount of time to respond.The "Think About It" Question: How can you make bcrypt run a dummy check even if the user is missing from your database?🕳️ Problem 4: The Array Bypass VulnerabilityThe Scenario: A student builds a login endpoint that expects req.body.password to be a string.The Code Trap: What happens if a malicious user uses Postman to send a JSON payload where the password is an array or an object instead of a string? For example: {"email": "admin@test.com", "password": ["password123", "wrongpass"]} or {"password": {"$gt": ""}}.The Student Challenge:Secure the route by adding strict input type validation. The application must reject the request immediately if req.body.email or req.body.password are anything other than flat strings.The "Think About It" Question: Why is relying on the database or bcrypt to catch bad data types a dangerous architectural habit?🚪 Problem 5: The "Back Button" Session IllusionThe Scenario: A user clicks "Logout" on a web app. The server successfully deletes their session or clears their cookie. However, when the user clicks the browser’s "Back" arrow button, they can still see their private dashboard page and private data.The Code Trap: The server isn't actually logged in💻 Problem 1: Safe Password Storage (Beginner)Goal: Implement secure account creation. Never save passwords as plaintext.Task: Create a /register POST route using Express. Accept email and password from req.body. Use the bcrypt library to hash the password asynchronously before saving it to your mock array or database.Testing Challenge: Prove your application is safe by printing the database array. Verify that the user's actual password is invisible and only the long, random hash is displayed.🎟️ Problem 2: Basic JWT Issuance & Login (Beginner)Goal: Generate stateless identity tokens.Task: Create a /login POST route. Take the email and password inputs. Use bcrypt.compare() to verify credentials against the hash saved in Problem 1. If valid, use the jsonwebtoken package to sign and return a JSON Web Token containing the user's ID.Testing Challenge: Use Postman to submit a bad password to confirm it returns an HTTP 401 Unauthorized status. Submit the right password and copy the resulting string token.🛡️ Problem 3: The Route Guard Middleware (Intermediate)Goal: Protect sensitive endpoints from anonymous traffic.Task: Write a custom Express middleware named authenticateToken(req, res, next). Read the token from the request's incoming Authorization header (Bearer <TOKEN>). Use jwt.verify() to check the token. If valid, append the decoded payload data to req.user and invoke next().Testing Challenge: Apply this middleware to a new route /dashboard. Attempt to fetch /dashboard with no header to verify a rejection, then inject your token from Problem 2 to successfully view the data.⏳ Problem 4: Refresh Tokens & Rotation (Advanced)Goal: Prevent permanent access risks by utilizing short-lived session lifetimes.Task: Modify your login logic to return two distinct tokens:An accessToken valid for exactly 15 minutes.A refreshToken valid for 7 days, securely stored inside an httpOnly cookie.Task Cont.: Build a /refresh POST route. This route reads the cookie, confirms the refresh token is valid against a database whitelist, and dispenses a brand new access token.Testing Challenge: Create a simulation where a user logs out. Build logic to wipe the refresh token from your database whitelist so that the cookie can no longer be exchanged for new access tokens.🛑 Problem 5: Multi-Factor Authentication (Advanced)Goal: Layer a secondary validation challenge on top of passwords.Task: Create a /2fa/setup route that utilizes the otplib or speakeasy library to establish a shared TOTP secret key for a user profile. Use a library to generate a QR code URI.Task Cont.: Build a /2fa/verify endpoint that accepts a 6-digit code from the user's authenticator app and unlocks their session only if the code matches.🔍 Self-Review Code Correction QuizCan you find the critical security flaw in this Node.js login handler?javascript// app.js snippet
app.post('/login', async (req, res) => {
  const user = await db.findUser(req.body.email);
  
  if (user && req.body.password === user.password) {
    const token = jwt.sign({ id: user.id }, "MY_SECRET_123");
    return res.json({ token });
  }
  res.status(401).send('Invalid');
});
Use code with caution.What is wrong?Plaintext Password Check: It uses strict string equality (===) rather than hashing verification, rendering stored passwords vulnerable to leaks.Hardcoded Secret Key: The token signature key "MY_SECRET_123" is committed in the raw code. It must be moved to an external environment variable via process.env.JWT_SECRET to prevent production exposure. anymore, but the browser cached the HTML/JSON response of the protected page.The Student Challenge:Write an Express middleware or configure headers on your protected routes that completely disables browser caching for sensitive endpoints (Cache-Control, Pragma, Expires).The "Think About It" Question: Is this a security flaw on the server side, client side, or both? How do you explain the difference between a cleared session and a cached screen?🕒 Problem 6: The "Forever" TokenThe Scenario: A student successfully implements JWT authentication. A user logs in, gets a token, and leaves. Three months later, that same token still grants access to the system.The Code Trap: The student forgot to pass an expiration option when signing the token: jwt.sign({ id: user.id }, secret). Without an explicit lifespan, tokens last forever by default. If a device is stolen, the attacker has permanent access.The Student Challenge:Configure the JWT payload to expire exactly 15 minutes after issuance. Write a second, separate route that simulates checking whether a token is still valid or has expired.The "Think About It" Question: If a token expires in 15 minutes, how can we keep a legitimate user logged in while they are actively using our app without forcing them to type their password every 15 minutes?



💻 Problem 1: Safe Password Storage (Beginner)Goal: Implement secure account creation. Never save passwords as plaintext.Task: Create a /register POST route using Express. Accept email and password from req.body. Use the bcrypt library to hash the password asynchronously before saving it to your mock array or database.Testing Challenge: Prove your application is safe by printing the database array. Verify that the user's actual password is invisible and only the long, random hash is displayed.🎟️ Problem 2: Basic JWT Issuance & Login (Beginner)Goal: Generate stateless identity tokens.Task: Create a /login POST route. Take the email and password inputs. Use bcrypt.compare() to verify credentials against the hash saved in Problem 1. If valid, use the jsonwebtoken package to sign and return a JSON Web Token containing the user's ID.Testing Challenge: Use Postman to submit a bad password to confirm it returns an HTTP 401 Unauthorized status. Submit the right password and copy the resulting string token.🛡️ Problem 3: The Route Guard Middleware (Intermediate)Goal: Protect sensitive endpoints from anonymous traffic.Task: Write a custom Express middleware named authenticateToken(req, res, next). Read the token from the request's incoming Authorization header (Bearer <TOKEN>). Use jwt.verify() to check the token. If valid, append the decoded payload data to req.user and invoke next().Testing Challenge: Apply this middleware to a new route /dashboard. Attempt to fetch /dashboard with no header to verify a rejection, then inject your token from Problem 2 to successfully view the data.⏳ Problem 4: Refresh Tokens & Rotation (Advanced)Goal: Prevent permanent access risks by utilizing short-lived session lifetimes.Task: Modify your login logic to return two distinct tokens:An accessToken valid for exactly 15 minutes.A refreshToken valid for 7 days, securely stored inside an httpOnly cookie.Task Cont.: Build a /refresh POST route. This route reads the cookie, confirms the refresh token is valid against a database whitelist, and dispenses a brand new access token.Testing Challenge: Create a simulation where a user logs out. Build logic to wipe the refresh token from your database whitelist so that the cookie can no longer be exchanged for new access tokens.🛑 Problem 5: Multi-Factor Authentication (Advanced)Goal: Layer a secondary validation challenge on top of passwords.Task: Create a /2fa/setup route that utilizes the otplib or speakeasy library to establish a shared TOTP secret key for a user profile. Use a library to generate a QR code URI.Task Cont.: Build a /2fa/verify endpoint that accepts a 6-digit code from the user's authenticator app and unlocks their session only if the code matches.🔍 Self-Review Code Correction QuizCan you find the critical security flaw in this Node.js login handler?javascript// app.js snippet
app.post('/login', async (req, res) => {
  const user = await db.findUser(req.body.email);
  
  if (user && req.body.password === user.password) {
    const token = jwt.sign({ id: user.id }, "MY_SECRET_123");
    return res.json({ token });
  }
  res.status(401).send('Invalid');
});
Use code with caution.What is wrong?Plaintext Password Check: It uses strict string equality (===) rather than hashing verification, rendering stored passwords vulnerable to leaks.Hardcoded Secret Key: The token signature key "MY_SECRET_123" is committed in the raw code. It must be moved to an external environment variable via process.env.JWT_SECRET to prevent production exposure.





