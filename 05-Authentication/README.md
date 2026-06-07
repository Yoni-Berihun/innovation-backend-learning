# 🟦 Module 05 — Authentication

> 👋 **Welcome to Authentication!**
> This module teaches practical authentication patterns for Node.js and Express: sessions, JWTs, refresh tokens, OAuth, MFA, and hardening techniques.

---

## 📖 Table of Contents

1. [Understanding JWTs (JSON Web Tokens)](#understanding-jwts-json-web-tokens)
2. [Setting Up the Project](#setting-up-the-project)
3. [How to Implement JWT Authentication](#how-to-implement-jwt-authentication)
4. [How to Verify JWTs and Protect Routes](#how-to-verify-jwts-and-protect-routes)
5. [Refresh Tokens and Rotation](#refresh-tokens-and-rotation)
6. [Conclusion](#conclusion)

Authentication in Node.js with Express 

![alt text](.images/image-5.png)

Every app that handles user accounts needs a way to confirm who’s who. That’s what authentication is for, making sure the person using an app is the person they claim to be. But doing this securely is harder than it sounds.

Traditional methods often rely on server sessions and cookies. Those work, but they don’t always scale well, especially when you’re building APIs or mobile apps that talk to multiple services. This is why JWTs, or JSON Web Tokens, are useful. They’re small, self-contained tokens that can carry user data safely between a client and a server.

JWTs make it easy to verify users without constantly checking a database – but they also expire fast to reduce risk. To keep users logged in without forcing them to sign in again every few minutes, we use something called a refresh token. It’s a separate, long-lived token that can request new access tokens when the old ones expire.

In this guide, we’ll walk through how to build a secure authentication system using JWTs and refresh tokens. You’ll learn how to generate tokens, validate them, handle expiry, and keep everything safe from common security threats.


Understanding JWTs (JSON Web Tokens)
A JWT, short for JSON Web Token, is a compact way to share information between a client and a server. It’s often used to prove that a user is who they say they are. The token is created on the server after a user logs in and is then sent back to the client. The client then includes this token with each request, so the server knows who is making the call.

A JWT has three parts: a header, a payload, and a signature.

The header usually tells the system which algorithm was used to sign the token.

The payload contains the data, such as the user’s ID or role.

The signature is the part that keeps everything secure. It’s created by hashing the header and payload with a secret key.

Once created, a JWT looks like a long string of random characters separated by dots. When the client sends it back to the server, the server verifies the signature using the same secret key. If it matches, the request is trusted.

One of the main benefits of JWTs is that they are stateless. The server doesn’t need to store session data. Everything needed to verify the user is already inside the token. This makes them fast and easy to use in modern APIs and microservices.

JWTs do have a downside: they cannot be revoked easily once issued. If a token is stolen, the attacker can use it until it expires. This is why short token lifetimes matter. It’s also why refresh tokens exist.

In the next section, we’ll finish the basic JWT setup. After that, we’ll add refresh tokens in “Refresh Tokens and Rotation.” That part shows how to handle expiry without making users log in again.

Setting Up the Project
Before writing any code, let’s set up a simple backend where we can build and test our authentication system. For this guide, we’ll use Node.js with Express, since it’s lightweight and easy to follow. You can use any stack later once you understand the flow.

Prerequisites
Make sure you have:

Node.js and npm installed

A text editor (VS Code works great)

Basic knowledge of JavaScript and APIs

1. Initialize the Project
Create a new folder and open it in your terminal.

mkdir jwt-auth-demo
cd jwt-auth-demo
npm init -y
This creates a package.json file that will track your dependencies.

2. Install Dependencies
You’ll need a few packages to get started:

express: the web framework

jsonwebtoken: to create and verify tokens

bcryptjs: to hash passwords

dotenv: to manage environment variables

Install them all at once like this:

npm install express jsonwebtoken bcryptjs dotenv
If you want auto-reloading while developing, install nodemon as a dev dependency:

npm install --save-dev nodemon
3. Project Structure
Here’s a clean structure to keep things organized:

jwt-auth-demo/
│
├── server.js
├── .env
├── package.json
│
├── config/
│   └── db.js
│
├── middleware/
│   └── auth.js
│
├── routes/
│   └── auth.js
│
└── models/
    └── user.js
4. Basic Express Setup
In server.js, start with a minimal Express server.

require('dotenv').config();
const express = require('express');
const app = express();

app.use(express.json());

app.get('/', (req, res) => {
  res.send('JWT Auth API running');
});

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
You can now run it using:

node server.js
or, if you’re using nodemon:

npx nodemon server.js
If everything is set up correctly, visiting http://localhost:5000 should display “JWT Auth API running”:
![alt text](.images/image-1.png)


How to Implement JWT Authentication
Now that your server is up, let’s add real authentication. We’ll start with user registration, password hashing, and login. Each user will get a token after logging in, which they can use to access protected routes.

1. Set Up the User Model
We’ll store users in a simple database. For this demo, let’s use MongoDB with Mongoose, since it’s quick to set up and easy to scale later.

Install the required packages:

npm install mongoose
Then create models/user.js:

const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  username: { type: String, required: true, unique: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true }
});

module.exports = mongoose.model('User', userSchema);
We store users with a unique email and a hashed password. The database never sees the raw password. Hashing makes stolen data harder to use.

2. Connect to MongoDB
Inside config/db.js:

const mongoose = require('mongoose');

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log('MongoDB connected');
  } catch (err) {
    console.error(err.message);
    process.exit(1);
  }
};

module.exports = connectDB;
mongoose.connect reads the connection string from .env. If the connection fails, we exit the process so we don’t continue in a broken state.

Update your server.js to include the connection:

const connectDB = require('./config/db');
connectDB();
And don’t forget to add your MongoDB URI in the .env file:

MONGO_URI=mongodb+srv://yourusername:yourpassword@cluster.mongodb.net/auth
JWT_SECRET=your_jwt_secret_key
3. Create Registration and Login Routes
In routes/auth.js:

const express = require('express');
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');
const User = require('../models/user');

const router = express.Router();

// Register a new user
router.post('/register', async (req, res) => {
  try {
    const { username, email, password } = req.body;

    const existingUser = await User.findOne({ email });
    if (existingUser) return res.status(400).json({ message: 'User already exists' });

    const hashedPassword = await bcrypt.hash(password, 10);

    const newUser = new User({ username, email, password: hashedPassword });
    await newUser.save();

    res.status(201).json({ message: 'User created successfully' });
  } catch (err) {
    res.status(500).json({ message: 'Server error' });
  }
});

// Login and issue JWT
router.post('/login', async (req, res) => {
  try {
    const { email, password } = req.body;

    const user = await User.findOne({ email });
    if (!user) return res.status(400).json({ message: 'Invalid credentials' });

    const isMatch = await bcrypt.compare(password, user.password);
    if (!isMatch) return res.status(400).json({ message: 'Invalid credentials' });

    const payload = { id: user._id, email: user.email };

    const token = jwt.sign(payload, process.env.JWT_SECRET, { expiresIn: '15m' });

    res.json({ token });
  } catch (err) {
    res.status(500).json({ message: 'Server error' });
  }
});

module.exports = router;
Add it to your server in server.js:

const authRoutes = require('./routes/auth');
app.use('/api/auth', authRoutes);
4. Test It Out
You can now test these routes using Postman or Insomnia.

Send a POST request to /api/auth/register with a JSON body:

{
  "username": "demoUser",
  "email": "demo@email.com",
  "password": "mypassword"
}

![alt text](.images/image-2.png)

the register route checks for an existing user by email. It hashes the password with a cost factor of 10 and then returns a 201 on success. We don’t log the password or include it in the response.

Then log in at /api/auth/login to receive a JWT.

![alt text](.images/image-3.png)The login route finds the user by email and compares the password with bcrypt.compare. If it matches, we sign a token with a small payload: the user ID and email. The JWT_SECRET signs the token so the server can verify it later. The expiresIn: '15m' setting keeps the token short-lived to limit risk. The response only includes the token. User data can be fetched from a protected route.

Once you get the token, copy it, you’ll use it to access protected routes later.

How to Verify JWTs and Protect Routes
Now that login returns a token, we should verify it on each request that needs auth. We will write a small middleware that checks the Authorization header, validates the token, and adds the user info to the request.

1. Create the Auth Middleware
Create middleware/auth.js:

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
What it does:

Reads the Authorization header.

Checks for the Bearer <token> format.

Verifies the token with the secret.

Attaches a simple user object to req for later use.

2. Create the Protected Route
Create a small profile route that returns the current user. Add routes/profile.js:

const express = require('express');
const auth = require('../middleware/auth');
const User = require('../models/user');

const router = express.Router();

router.get('/me', auth, async (req, res) => {
  try {
    const user = await User.findById(req.user.id).select('-password');
    if (!user) {
      return res.status(404).json({ message: 'User not found' });
    }
    res.json({ user });
  } catch (err) {
    res.status(500).json({ message: 'Server error' });
  }
});

module.exports = router;
Wire it in server.js:

const profileRoutes = require('./routes/profile');
app.use('/api/profile', profileRoutes);
Now a GET /api/profile/me call will only work with a valid token.

3. Handle Token Expiry Clearly
Short access tokens reduce damage if they leak. We set expiresIn: '15m' during login. When a token expires, the middleware returns a 401 with Access token expired.

We won’t refresh the token here because refresh requires its own endpoint, storage, and rotation rules. You’ll add that in “Refresh Tokens and Rotation.” For now, the 401 proves that the expiry is enforced.

4. Testing the Flow
In this section, we’ll test that the server blocks requests without a valid token and allows requests with a valid token.

Log in at /api/auth/login and copy the token. Then call /api/profile/me with:

Authorization: Bearer <paste_token_here>
You should see the current user without the password field.![alt text](.images/image-4.png)Then remove the header or change the token and call again. You should get a 401.

Next, wait for the token to expire or change expiresIn to a very short value for a quick test. Call again and confirm you get Access token expired.

Tips for debugging
401 with “Missing or invalid Authorization header” means the header format is wrong. Use Authorization: Bearer <token>.

401 with “Invalid token” means the token string is wrong, signed with the wrong secret, or corrupted.

401 with “Access token expired” means the expiry check works. You will fix the client experience with the refresh endpoint later.

If all calls fail, confirm your JWT_SECRET is set in .env and that the server was restarted after changes.

5. Optional Cookie Support
You can store tokens in HTTP-only cookies. The browser sends them automatically. Scripts cannot read HTTP-only cookies, which reduces the risk from XSS.

Install and enable cookies:

npm install cookie-parser
// server.js
const cookieParser = require('cookie-parser');
app.use(cookieParser());
Read the access token from a cookie as a fallback:

// middleware/auth.js
const jwt = require('jsonwebtoken');

function auth(req, res, next) {
  const header = req.headers.authorization || '';
  const [scheme, tokenFromHeader] = header.split(' ');
  const tokenFromCookie = req.cookies?.access_token;

  const token = scheme === 'Bearer' && tokenFromHeader ? tokenFromHeader : tokenFromCookie;

  if (!token) return res.status(401).json({ message: 'No token provided' });

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = { id: decoded.id, email: decoded.email };
    next();
  } catch (err) {
    const msg = err.name === 'TokenExpiredError' ? 'Access token expired' : 'Invalid token';
    return res.status(401).json({ message: msg });
  }
}

module.exports = auth;
How this works:

The access token can live in a cookie named access_token.

Mark the cookie as httpOnly and secure in production.

Set sameSite: 'strict' to reduce CSRF risk.

For APIs used by browsers, cookies simplify sending tokens. For SPAs that call many domains, an Authorization header may be simpler.

In the next section, we’ll use the same cookie approach for the refresh token. That section explains why refresh belongs in a cookie and how rotation blocks replay.

Refresh Tokens and Rotation
Access tokens are short-lived and used on every request. They prove the user identity quickly. Refresh tokens live longer and are used only to get new access tokens when the old ones expire. This split keeps day-to-day requests fast and limits the damage if a token leaks.

We will store the refresh token in an HTTP-only cookie. This reduces exposure to scripts and keeps the flow smooth.

1. Install and Setup
We already have cookie-parser. We won’t add anything new for now, but we will use Node’s built-in crypto module to hash the refresh token before storing it. As a reminder, hashing means the raw token is never saved. If the database leaks, attackers cannot use the hashes to log in.

Create models/refreshToken.js:

const mongoose = require('mongoose');

const refreshTokenSchema = new mongoose.Schema({
  user: { type: mongoose.Schema.Types.ObjectId, ref: 'User', index: true },
  tokenHash: { type: String, required: true, unique: true },
  jti: { type: String, required: true, index: true },
  expiresAt: { type: Date, required: true, index: true },
  revokedAt: { type: Date, default: null },
  replacedBy: { type: String, default: null }, // new jti when rotated
  createdAt: { type: Date, default: Date.now },
  ip: String,
  userAgent: String
});

module.exports = mongoose.model('RefreshToken', refreshTokenSchema);
2. Token Helpers
Create utils/tokens.js for clean, reusable logic.

const jwt = require('jsonwebtoken');
const crypto = require('crypto');
const RefreshToken = require('../models/refreshToken');

const ACCESS_TTL = '15m';
const REFRESH_TTL_SEC = 60 * 60 * 24 * 7; // 7 days

function hashToken(token) {
  return crypto.createHash('sha256').update(token).digest('hex');
}

function createJti() {
  return crypto.randomBytes(16).toString('hex');
}

function signAccessToken(user) {
  const payload = { id: user._id.toString(), email: user.email };
  return jwt.sign(payload, process.env.JWT_SECRET, { expiresIn: ACCESS_TTL });
}

function signRefreshToken(user, jti) {
  const payload = { id: user._id.toString(), jti };
  const token = jwt.sign(payload, process.env.REFRESH_TOKEN_SECRET, { expiresIn: REFRESH_TTL_SEC });
  return token;
}

async function persistRefreshToken({ user, refreshToken, jti, ip, userAgent }) {
  const tokenHash = hashToken(refreshToken);
  const expiresAt = new Date(Date.now() + REFRESH_TTL_SEC * 1000);
  await RefreshToken.create({ user: user._id, tokenHash, jti, expiresAt, ip, userAgent });
}

function setRefreshCookie(res, refreshToken) {
  const isProd = process.env.NODE_ENV === 'production';
  res.cookie('refresh_token', refreshToken, {
    httpOnly: true,
    secure: isProd,
    sameSite: 'strict',
    path: '/api/auth/refresh',
    maxAge: REFRESH_TTL_SEC * 1000
  });
}

async function rotateRefreshToken(oldDoc, user, req, res) {
  // revoke old
  oldDoc.revokedAt = new Date();
  const newJti = createJti();
  oldDoc.replacedBy = newJti;
  await oldDoc.save();

  // issue new
  const newAccess = signAccessToken(user);
  const newRefresh = signRefreshToken(user, newJti);
  await persistRefreshToken({
    user,
    refreshToken: newRefresh,
    jti: newJti,
    ip: req.ip,
    userAgent: req.headers['user-agent'] || ''
  });
  setRefreshCookie(res, newRefresh);
  return { accessToken: newAccess };
}

module.exports = {
  hashToken,
  createJti,
  signAccessToken,
  signRefreshToken,
  persistRefreshToken,
  setRefreshCookie,
  rotateRefreshToken
};
In this code,

signAccessToken creates a short token with the user ID and email.

signRefreshToken creates a long-lived token with a jti value. The jti lets us rotate and track tokens.

persistRefreshToken hashes the refresh token and stores metadata like expiry and device info.

setRefreshCookie writes the HTTP-only cookie so the browser sends it to the refresh endpoint automatically.

rotateRefreshToken revokes the old token, issues a new pair, and saves the new record. Rotation blocks replay if an old refresh token is stolen.

3. Issue Refresh Token on Login
Update your routes/auth.js login handler to create and store a refresh token, then set the cookie.

const express = require('express');
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');
const User = require('../models/user');
const RefreshToken = require('../models/refreshToken');
const {
  createJti,
  signAccessToken,
  signRefreshToken,
  persistRefreshToken,
  setRefreshCookie
} = require('../utils/tokens');

const router = express.Router();

router.post('/login', async (req, res) => {
  try {
    const { email, password } = req.body;

    const user = await User.findOne({ email });
    if (!user) return res.status(400).json({ message: 'Invalid credentials' });

    const isMatch = await bcrypt.compare(password, user.password);
    if (!isMatch) return res.status(400).json({ message: 'Invalid credentials' });

    const accessToken = signAccessToken(user);

    const jti = createJti();
    const refreshToken = signRefreshToken(user, jti);

    await persistRefreshToken({
      user,
      refreshToken,
      jti,
      ip: req.ip,
      userAgent: req.headers['user-agent'] || ''
    });

    setRefreshCookie(res, refreshToken);

    res.json({ accessToken });
  } catch (err) {
    res.status(500).json({ message: 'Server error' });
  }
});

module.exports = router;
On login, we issue both tokens. The access token goes to the JSON response. The refresh token goes to an HTTP-only cookie scoped to /api/auth/refresh. This keeps the refresh token away from frontend code while still letting the browser send it to the refresh endpoint.

4. The Refresh Endpoint
Create an endpoint that reads the refresh cookie, verifies it, checks the database entry, and rotates it. If all checks pass, it returns a new access token and sets a new refresh cookie.

Add to routes/auth.js:

const { hashToken, rotateRefreshToken } = require('../utils/tokens');

router.post('/refresh', async (req, res) => {
  try {
    const token = req.cookies?.refresh_token;
    if (!token) return res.status(401).json({ message: 'No refresh token' });

    let decoded;
    try {
      decoded = jwt.verify(token, process.env.REFRESH_TOKEN_SECRET);
    } catch (err) {
      return res.status(401).json({ message: 'Invalid or expired refresh token' });
    }

    const tokenHash = hashToken(token);
    const doc = await RefreshToken.findOne({ tokenHash, jti: decoded.jti }).populate('user');

    if (!doc) {
      return res.status(401).json({ message: 'Refresh token not recognized' });
    }
    if (doc.revokedAt) {
      return res.status(401).json({ message: 'Refresh token revoked' });
    }
    if (doc.expiresAt < new Date()) {
      return res.status(401).json({ message: 'Refresh token expired' });
    }

    const result = await rotateRefreshToken(doc, doc.user, req, res);
    return res.json({ accessToken: result.accessToken });
  } catch (err) {
    res.status(500).json({ message: 'Server error' });
  }
});
The refresh endpoint verifies the cookie, checks the database record, confirms it is not expired or revoked, then rotates it. Rotation sets revokedAt on the old record and creates a new one with a fresh jti. The response returns a new access token and sets a new refresh cookie.

5. Logout and Revoke
On logout, revoke the current refresh token and clear the cookie.

router.post('/logout', async (req, res) => {
  try {
    const token = req.cookies?.refresh_token;
    if (token) {
      const tokenHash = hashToken(token);
      const doc = await RefreshToken.findOne({ tokenHash });
      if (doc && !doc.revokedAt) {
        doc.revokedAt = new Date();
        await doc.save();
      }
    }
    res.clearCookie('refresh_token', { path: '/api/auth/refresh' });
    res.json({ message: 'Logged out' });
  } catch (err) {
    res.status(500).json({ message: 'Server error' });
  }
});
Logout revokes the matching refresh token if present and clears the cookie. This ends the session cleanly on the server side and the client side.

6. Client Flow
Here is how the browser app should behave:

Keep the access token in memory. Do not put it in localStorage.

Call protected APIs with the Authorization header or let cookies handle it if you chose the cookie approach for access.

If a call fails with Access token expired, call /api/auth/refresh. The browser sends the refresh cookie automatically.

Replace the in-memory access token with the new one.

Retry the original request.

On logout, call /api/auth/logout and clear any local state.

7. Security Notes
There are some key steps you can take to make sure everything is secure:

Separate secrets
Use a different secret for access and refresh tokens. If the access secret leaks, refresh tokens still use a different key. Set JWT_SECRET and REFRESH_TOKEN_SECRET in .env.

HTTPS only
Serve production traffic over HTTPS. Cookies marked secure: true only travel over HTTPS. This protects tokens in transit.

Rotate on every refresh
Issue a new refresh token and revoke the old one each time you refresh. Rotation makes a stolen old token useless after the next refresh.

Hash refresh tokens in the database
Store a SHA-256 hash, not the raw token. This way a database leak does not give attackers the actual token string.

Scope and flags for cookies
Use httpOnly: true, secure: true in production, sameSite: 'strict', and a narrow path such as /api/auth/refresh. These flags reduce XSS and CSRF risk and limit where the cookie is sent.

Short access TTL and moderate refresh TTL
Keep access tokens short, such as 15 minutes. Use a refresh lifetime like 7 days. This keeps risk low without annoying users.

Device awareness
Store ip and userAgent. If patterns change in a suspicious way, you can revoke or challenge the session.

Auditing and limits
Log refresh events and consider rate limits on the refresh endpoint. This helps detect abuse.’

Add to .env:

REFRESH_TOKEN_SECRET=your_refresh_secret_key
Conclusion
You now have a working authentication system that uses JWTs and refresh tokens to keep users logged in safely. The access token handles quick verification. The refresh token quietly renews access when it expires. Together, they strike a balance between security and convenience.

You built user registration, login, protected routes, and a full refresh flow. You also learned how to rotate refresh tokens, store them securely, and handle logout cleanly. Each step adds another layer of safety that keeps your app and users protected.

From here, you can expand this setup to match your real project. You can add role-based permissions, track user sessions by device, or move the logic into a dedicated authentication service. What matters most is understanding the flow and keeping tokens short-lived and well-guarded
Stateless vs. Stateful Authentication
Stateless Authentication: The server does not maintain any session data. Each request is independent and must contain all the necessary information for authentication, typically through tokens (e.g., JWT).
Stateful Authentication: The server maintains session data for authenticated users, often stored in a database or memory. The client holds a session ID to access the stored session data.
Common Node.js Express Authentication Methods
1. Basic Authentication
Description: Users provide their username and password for each request.
Use Case: Simple and quick to implement, suitable for basic applications or internal tools.
Example Libraries: No specific library required, implemented using base64 encoding and decoding.
2. Token-Based Authentication (JWT)
Description: Users authenticate by receiving a JSON Web Token (JWT) after logging in, which they include in the header of subsequent requests.
Use Case: Stateless authentication, commonly used in modern web and mobile applications.
Example Libraries: jsonwebtoken, express-jwt
3. Session-Based Authentication
Description: User credentials are stored in a session on the server after login. The session ID is stored in a cookie on the client side.
Use Case: Traditional web applications where server-side sessions are manageable.
Example Libraries: express-session, connect-mongo
4. OAuth2
Description: Delegated authorization framework that allows third-party services to exchange tokens on behalf of the user.
Use Case: Integration with third-party services like Google, Facebook, and GitHub for authentication.
Example Libraries: passport, passport-google-oauth20, passport-facebook, passport-github
5. Social Login
Description: Authentication through social media accounts like Google, Facebook, or Twitter.
Use Case: Allows users to log in using their social media accounts, simplifying the login process.
Example Libraries: passport-google-oauth20, passport-facebook, passport-twitter
6. Multi-Factor Authentication (MFA)
Description: Adds an extra layer of security by requiring multiple forms of verification (e.g., password + OTP).
Use Case: High-security applications where additional authentication layers are necessary.
Example Libraries: speakeasy (for OTP generation), node-2fa
7. API Key Authentication
Description: Users include an API key in the request header for each API call.
Use Case: Commonly used for service-to-service communication or for public APIs.
Example Libraries: No specific library required, implemented by checking the API key in the request header.
8. LDAP Authentication
Description: Authenticates users against a directory service like Microsoft Active Directory.
Use Case: Enterprise applications where centralized authentication is required.
Example Libraries: passport-ldapauth, ldapjs
9. SAML Authentication
Description: Security Assertion Markup Language (SAML) is an XML-based protocol for exchanging authentication and authorization data between parties.
Use Case: Enterprise single sign-on (SSO) solutions.
Example Libraries: passport-saml
How to Choose the Right Node.js Express Authentication Methods?
Choosing the right authentication method for your Node.js Express application depends on several factors, including the security requirements, user experience, and specific use cases of your application. Here’s a guide on when to use each authentication method:

1. Basic Authentication
When to Use:

Internal Tools or Prototypes: Simple projects where security is not a major concern.
Quick and Easy Setup: Scenarios that require minimal setup and complexity.
Pros:

Simple to implement.
No need for additional libraries.
Cons:

Not secure for sensitive data (credentials are base64 encoded, not encrypted).
Requires HTTPS to be somewhat secure.
2. Token-Based Authentication (JWT)
When to Use:

Single Page Applications (SPAs): Modern web applications with front-end frameworks like React, Angular, or Vue.js.
Mobile Applications: APIs for mobile apps where stateless authentication is beneficial.
Microservices Architecture: Distributed systems where each service can independently verify the token.
Pros:

Stateless, no server-side session storage required.
Can be easily used across different domains.
Cons:

Token storage on the client side can be challenging (localStorage, cookies, etc.).
Revoking tokens can be complex.
3. Session-Based Authentication
When to Use:

Traditional Web Applications: Websites where server-side rendering is used.
Applications with Persistent Sessions: Where the user experience benefits from maintaining a session state.
Pros:

Centralized session management.
Easier to implement and manage with frameworks like Express.
Cons:

Requires server-side storage (in-memory, database, etc.).
Less scalable due to server-side state.
4. OAuth2
When to Use:

Third-Party Integrations: Applications that need to access user data from services like Google, Facebook, GitHub, etc.
Delegated Authorization: When you need users to grant access to their resources on another service.
Pros:

Secure and widely adopted.
Allows third-party access without sharing passwords.
Cons:

Complex setup and implementation.
Dependency on third-party providers.
5. Social Login
When to Use:

Consumer-Facing Applications: Apps where simplifying the registration and login process enhances user experience.
User Convenience: When you want to reduce friction for users by leveraging their existing social media accounts.
Pros:

Simplifies login for users.
Reduces the burden of password management.
Cons:

Dependency on social media providers.
Potential privacy concerns from users.
6. Multi-Factor Authentication (MFA)
When to Use:

High-Security Applications: Banking, financial services, healthcare, and other applications where security is critical.
Compliance Requirements: Industries with regulatory requirements for enhanced security.
Pros:

Significantly increases security.
Can be combined with other methods for robust authentication.
Cons:

More complex to implement and manage.
Can impact user experience due to additional steps.
7. API Key Authentication
When to Use:

Service-to-Service Communication: Internal APIs where different services need to authenticate each other.
Public APIs: When providing access to third-party developers.
Pros:

Simple to implement and use.
Easily revocable and manageable.
Cons:

Less secure (API keys can be leaked).
No user-specific context, just service-level authentication.
8. LDAP Authentication
When to Use:

Enterprise Applications: Large organizations with centralized user directories like Active Directory.
Internal Tools: Where employees’ credentials need to be verified against a corporate directory.
Pros:

Centralized user management.
Integrates well with existing corporate infrastructure.
Cons:

Requires LDAP server setup and management.
Can be complex to implement and debug.
9. SAML Authentication
When to Use:

Enterprise Single Sign-On (SSO): When integrating with enterprise SSO solutions.
Applications Requiring Federated Identity: Where user authentication needs to be federated across multiple domains.
Pros:

Secure and standardized.
Facilitates SSO across multiple applications.
Cons:

Complex setup and configuration.
Typically requires more infrastructure.
A Brief Summary of Choosing Authentication Methods
Choosing the right authentication method for your Node.js Express application involves understanding the different options available and evaluating them against your application's specific requirements.

Basic Authentication: Quick and simple for non-critical applications.

Token-Based Authentication (JWT): Ideal for SPAs, mobile apps, and microservices.

Session-Based Authentication: Suitable for traditional web applications.

OAuth2: Best for third-party integrations and delegated access.

Social Login: Great for consumer-facing applications to improve user experience.

Multi-Factor Authentication (MFA): Essential for high-security applications.

API Key Authentication: Useful for service-to-service communication and public APIs.

LDAP Authentication: Fit for enterprise applications with centralized user management.

SAML Authentication: Used for enterprise SSO and federated identity systems.

Choosing the right method depends on your application’s specific needs, security requirements, and user experience considerations.

Node.js Express Authentication Examples
Authentication is a critical part of any web application, ensuring that users can securely access resources. Let's explore various examples of how to implement authentication in a Node.js Express application. We'll cover some of the most common methods: JWT (JSON Web Tokens), session-based authentication, OAuth2, and API keys.

1. JWT (JSON Web Tokens) Authentication
JWT is a stateless authentication method that allows you to securely transmit information between parties as a JSON object. This information can be verified and trusted because it is digitally signed.

Implementation Steps:

Step 1: Set Up Your Project

First, create a new project and install the necessary dependencies:

mkdir jwt-auth-example
cd jwt-auth-example
npm init -y
npm install express jsonwebtoken body-parser bcryptjs
Step 2: Create the Express Server

Create an app.js file and set up a basic Express server:

const express = require('express');
const jwt = require('jsonwebtoken');
const bodyParser = require('body-parser');
const bcrypt = require('bcryptjs');

const app = express();
app.use(bodyParser.json());

const SECRET_KEY = 'your_jwt_secret';

// Mock User Data
const users = [{ id: 1, username: 'user1', password: bcrypt.hashSync('password1', 8) }];

app.post('/login', (req, res) => {
  const { username, password } = req.body;
  const user = users.find(u => u.username === username);
  if (user && bcrypt.compareSync(password, user.password)) {
    const token = jwt.sign({ id: user.id, username: user.username }, SECRET_KEY, { expiresIn: '1h' });
    res.json({ token });
  } else {
    res.status(401).send('Invalid credentials');
  }
});

const authenticateJWT = (req, res, next) => {
  const token = req.headers.authorization;
  if (token) {
    jwt.verify(token, SECRET_KEY, (err, user) => {
      if (err) {
        return res.sendStatus(403);
      }
      req.user = user;
      next();
    });
  } else {
    res.sendStatus(401);
  }
};

app.get('/protected', authenticateJWT, (req, res) => {
  res.send(`Hello ${req.user.username}, you have accessed a protected route!`);
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
2. Session-Based Authentication
Session-based authentication relies on storing session data on the server side. This method is stateful and is commonly used in traditional web applications.

Implementation Steps:

Step 1: Set Up Your Project

Create a new project and install the necessary dependencies:

mkdir session-auth-example
cd session-auth-example
npm init -y
npm install express express-session body-parser bcryptjs
Step 2: Create the Express Server

Create an app.js file and set up a basic Express server:

const express = require('express');
const session = require('express-session');
const bodyParser = require('body-parser');
const bcrypt = require('bcryptjs');

const app = express();
app.use(bodyParser.json());
app.use(session({ secret: 'your_session_secret', resave: false, saveUninitialized: true }));

const users = [{ id: 1, username: 'user1', password: bcrypt.hashSync('password1', 8) }];

app.post('/login', (req, res) => {
  const { username, password } = req.body;
  const user = users.find(u => u.username === username);
  if (user && bcrypt.compareSync(password, user.password)) {
    req.session.userId = user.id;
    res.send('Logged in');
  } else {
    res.status(401).send('Invalid credentials');
  }
});

const authenticateSession = (req, res, next) => {
  if (req.session.userId) {
    next();
  } else {
    res.sendStatus(401);
  }
};

app.get('/protected', authenticateSession, (req, res) => {
  const user = users.find(u => u.id === req.session.userId);
  res.send(`Hello ${user.username}, you have accessed a protected route!`);
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
3. OAuth2 Authentication
OAuth2 is a more complex authentication method that allows third-party applications to access user resources without exposing user credentials. It’s commonly used for social login and integrating with other services.

Implementation Steps:

Implementing OAuth2 usually involves using a library or framework that handles the OAuth2 flow. For simplicity, we’ll use the passport library with a strategy like passport-google-oauth20.

Step 1: Set Up Your Project

Create a new project and install the necessary dependencies:

mkdir oauth2-auth-example
cd oauth2-auth-example
npm init -y
npm install express passport passport-google-oauth20 express-session
Step 2: Create the Express Server

Create an app.js file and set up a basic Express server:

const express = require('express');
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;
const session = require('express-session');

const app = express();

app.use(session({ secret: 'your_session_secret', resave: false, saveUninitialized: true }));
app.use(passport.initialize());
app.use(passport.session());

passport.use(new GoogleStrategy({
  clientID: 'YOUR_GOOGLE_CLIENT_ID',
  clientSecret: 'YOUR_GOOGLE_CLIENT_SECRET',
  callbackURL: 'http://localhost:3000/auth/google/callback'
}, (accessToken, refreshToken, profile, done) => {
  // In a real application, you would save the profile info to your database
  return done(null, profile);
}));

passport.serializeUser((user, done) => {
  done(null, user);
});

passport.deserializeUser((obj, done) => {
  done(null, obj);
});

app.get('/auth/google', passport.authenticate('google', { scope: ['https://www.googleapis.com/auth/plus.login'] }));

app.get('/auth/google/callback', passport.authenticate('google', { failureRedirect: '/' }), (req, res) => {
  res.redirect('/protected');
});

const ensureAuthenticated = (req, res, next) => {
  if (req.isAuthenticated()) {
    return next();
  }
  res.redirect('/');
};

app.get('/protected', ensureAuthenticated, (req, res) => {
  res.send(`Hello ${req.user.displayName}, you have accessed a protected route!`);
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
4. API Key Authentication
API key authentication is simple and often used for server-to-server communication. It involves passing a key with each request to verify the client.

Implementation Steps

Step 1: Set Up Your Project

Create a new project and install the necessary dependencies:

mkdir api-key-auth-example
cd api-key-auth-example
npm init -y
npm install express
Step 2: Create the Express Server

Create an app.js file and set up a basic Express server:

const express = require('express');
const app = express();

const API_KEY = 'your_api_key';

const authenticateApiKey = (req, res, next) => {
  const apiKey = req.headers['x-api-key'];
  if (apiKey && apiKey === API_KEY) {
    next();
  } else {
    res.sendStatus(401);
  }
};

app.get('/protected', authenticateApiKey, (req, res) => {
  res.send('Access granted to protected route');
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
From stateless JWT authentication to traditional session-based authentication and OAuth2 for third-party integrations, you have a variety of methods to choose from based on your application’s requirements. Understanding and correctly implementing these methods will help you build secure and scalable applications.

10 Best Practices for Nodejs Express Authentication
Implementing authentication in a Node.js Express application requires careful consideration to ensure security, scalability, and ease of use. Here are some best practices to follow when handling authentication in your Node.js Express applications:

1. Use Strong Password Hashing
Hash Passwords: Always hash passwords before storing them in the database. Use a robust hashing algorithm like bcrypt.
Salt Passwords: Add a unique salt to each password to prevent rainbow table attacks.
const bcrypt = require('bcryptjs');

const hashPassword = async (password) => {
    const salt = await bcrypt.genSalt(10);
    return await bcrypt.hash(password, salt);
};
2. Secure JWT Tokens
Keep Secret Keys Secure: Store secret keys in environment variables and not in your source code.
Set Expiry Time: Always set an expiration time for JWTs to limit their validity period.
Use Strong Algorithms: Use a strong signing algorithm (e.g., HS256).
const jwt = require('jsonwebtoken');
const SECRET_KEY = process.env.SECRET_KEY;

const token = jwt.sign({ userId: user.id }, SECRET_KEY, { expiresIn: '1h' });
3. Use HTTPS
Encrypt Traffic: Always use HTTPS to encrypt data transmitted between the client and server, protecting against man-in-the-middle attacks.
4. Validate Input
Sanitize and Validate: Use libraries like Joi or express-validator to sanitize and validate user inputs to prevent injection attacks.
const { body, validationResult } = require('express-validator');

app.post('/register', [
    body('email').isEmail(),
    body('password').isLength({ min: 6 })
], (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
        return res.status(400).json({ errors: errors.array() });
    }
    // Proceed with registration
});
5. Implement Rate Limiting
Prevent Brute Force Attacks: Use rate limiting to limit the number of requests a client can make to your authentication endpoints.
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100 // limit each IP to 100 requests per windowMs
});

app.use('/login', limiter);
6. Store Tokens Securely
Use Secure Cookies: Store JWTs in secure, HTTP-only cookies to prevent access from JavaScript (protect against XSS attacks).
Refresh Tokens: Implement refresh tokens to allow users to obtain new access tokens without re-authenticating.
7. Implement Proper Session Management
Invalidate Tokens: Implement token invalidation on logout to prevent reuse of tokens.
Session Expiry: Ensure sessions expire after a certain period of inactivity.
8. Use Middleware for Protected Routes
Centralize Authentication Logic: Use middleware to handle authentication logic for protected routes.
const authenticateJWT = (req, res, next) => {
    const token = req.headers.authorization;

    if (!token) {
        return res.sendStatus(401);
    }

    jwt.verify(token, SECRET_KEY, (err, user) => {
        if (err) {
            return res.sendStatus(403);
        }

        req.user = user;
        next();
    });
};

app.get('/protected', authenticateJWT, (req, res) => {
    res.send('This is a protected route');
});
9. Monitor and Log Authentication Events
Log Suspicious Activity: Monitor and log authentication-related events to detect and respond to suspicious activities.
Audit Logs: Maintain audit logs for user login attempts, account changes, and other critical actions.
10. Regularly Update Dependencies
Keep Libraries Up to Date: Regularly update your dependencies to benefit from security patches and improvements.
Audit Dependencies: Use tools like npm audit to identify and fix vulnerabilities in your dependencies.
Leveraging Apidog to Test APIs with Nodejs Express Authentication Methods
Apidog is a comprehensive API development platform that streamlines the entire development process. It features robust built-in authentication options, allowing developers to test API endpoints with various methods including API Key, Bearer Token, JWT, Basic Auth, Digest Auth, OAuth 1.0, OAuth 2.0, Hawk Authentication, NTLM, and Akamai EdgeGrid. This enables API developers to thoroughly validate the authentication strategies implemented in their APIs.

![alt text](.images/image.png)Conclusion
Implementing authentication in a Node.js Express application is crucial for ensuring the security and integrity of your web application. By understanding the various authentication methods, from Basic Auth and JWT to OAuth2 and LDAP, and following best practices like using strong password hashing, securing JWT tokens, and validating input, you can create robust and secure authentication systems. Tools like Apidog further enhance your ability to test and validate these authentication methods, ensuring they work as intended. By carefully choosing and implementing the right authentication strategy for your application’s needs, you can provide a secure and seamless user experience, protecting sensitive data and resources effectively.