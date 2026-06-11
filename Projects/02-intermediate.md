
```markdown
# 🟡 Intermediate Backend Projects - 15 Hands-On Exercises

A comprehensive collection of **15 intermediate-level backend projects** covering authentication, real-time features, databases, caching, queues, and production-ready practices. Each project includes detailed descriptions, learning objectives, and direct links to working examples and tutorials.

---

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Projects 1-5: Authentication & Security](#projects-1-5-authentication--security)
- [Projects 6-10: Real-Time & WebSockets](#projects-6-10-real-time--websockets)
- [Projects 11-15: Performance & Architecture](#projects-11-15-performance--architecture)
- [Project Reference Index](#project-reference-index)
- [Completion Tracker](#completion-tracker)

---

## ✅ Prerequisites

Before starting intermediate projects, ensure you have:

- Completed at least 8-10 beginner projects
- Solid understanding of JavaScript ES6+ (async/await, promises, destructuring)
- Familiarity with Express.js and REST APIs
- Basic knowledge of databases (MongoDB or PostgreSQL)
- Understanding of authentication basics (JWT, bcrypt)

---

## Projects 1-5: Authentication & Security

---

### 1. 🔐 Complete JWT Authentication System

**Difficulty:** ⭐⭐⭐ | **Time:** 4 hours | **Dependencies:** express, bcrypt, jsonwebtoken, cookie-parser

#### 📝 Description
Build a production-ready authentication system with access tokens, refresh tokens, token rotation, and secure HTTP-only cookies. Include login, logout, token refresh, and protected routes.

#### 🎯 Learning Objectives
- Implement access & refresh token strategy
- Store refresh tokens in database (allow revocation)
- Use HTTP-only cookies for security
- Implement token rotation (new refresh token each request)
- Add logout and token invalidation

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [JWT Refresh Tokens - Ben Awad](https://www.youtube.com/watch?v=mbsmsi7l3r4) |
| 💻 **Working Code** | [github.com/vercel/next.js/tree/canary/examples/with-passport](https://github.com/vercel/next.js/tree/canary/examples/with-passport) |
| 📖 **Documentation** | [JWT Refresh Token Best Practices](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/) |
| ✍️ **Step-by-Step** | [BezKoder JWT Refresh Tutorial](https://www.bezkoder.com/jwt-refresh-token-node-js/) |

#### 📍 API Endpoints
```http
POST /api/auth/register     - Create account (hashed password)
POST /api/auth/login        - Get access + refresh tokens (HTTP-only cookie)
POST /api/auth/refresh      - Get new access token using refresh token
POST /api/auth/logout       - Clear refresh token cookie
GET  /api/auth/profile      - Protected route (requires access token)
```

---

### 2. 🔑 OAuth 2.0 Integration (Google & GitHub)

**Difficulty:** ⭐⭐⭐ | **Time:** 3 hours | **Dependencies:** passport, passport-google-oauth20, passport-github

#### 📝 Description
Add social login functionality using OAuth 2.0. Users can sign in with Google or GitHub accounts. Store OAuth profiles in database and handle account linking.

#### 🎯 Learning Objectives
- Understand OAuth 2.0 flow (redirects, authorization codes, tokens)
- Integrate Passport.js strategies
- Handle OAuth callbacks
- Link OAuth accounts to existing users
- Extract and store user profile data

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [OAuth with Passport - Traversy Media](https://www.youtube.com/watch?v=9YcWrI39Uz0) |
| 💻 **Working Code** | [github.com/passport/express-4.x-oauth2-example](https://github.com/passport/express-4.x-oauth2-example) |
| 📖 **Documentation** | [Passport.js Docs](https://www.passportjs.org/concepts/authentication/oauth/) |
| ✍️ **Step-by-Step** | [DigitalOcean OAuth Guide](https://www.digitalocean.com/community/tutorials/oauth-2-0-nodejs) |

#### 🔑 Setup Required
- Google Cloud Console: Create OAuth 2.0 credentials
- GitHub Developer Settings: Register OAuth App

---

### 3. 📧 Email Verification & Password Reset

**Difficulty:** ⭐⭐⭐ | **Time:** 3 hours | **Dependencies:** nodemailer, jsonwebtoken, redis (optional)

#### 📝 Description
Add email verification during registration and password reset functionality. Send verification links with expiring tokens. Users must verify email before accessing protected features.

#### 🎯 Learning Objectives
- Generate time-limited verification tokens
- Send HTML emails with links
- Handle token expiration and resend functionality
- Implement secure password reset flow
- Rate limit email requests to prevent abuse

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Email Verification - Web Dev Simplified](https://www.youtube.com/watch?v=Q7iD0W9qNws) |
| 💻 **Working Code** | [github.com/joshuaquinones/email-verification](https://github.com/joshuaquinones/email-verification) |
| 📖 **Documentation** | [Nodemailer Guide](https://nodemailer.com/usage/example/) |
| ✍️ **Step-by-Step** | [freeCodeCamp Email Verification](https://www.freecodecamp.org/news/how-to-set-up-email-verification-in-node-js/) |

#### 📍 Flow
```http
1. POST /api/auth/register → sends verification email
2. GET /api/auth/verify/:token → verifies email
3. POST /api/auth/forgot-password → sends reset email
4. POST /api/auth/reset-password/:token → resets password
```

---

### 4. 🛡️ Two-Factor Authentication (2FA)

**Difficulty:** ⭐⭐⭐⭐ | **Time:** 4 hours | **Dependencies:** speakeasy, qrcode

#### 📝 Description
Add time-based one-time password (TOTP) 2FA using Google Authenticator. Users can enable/disable 2FA, backup codes, and QR code scanning.

#### 🎯 Learning Objectives
- Generate TOTP secrets using speakeasy
- Create QR codes for authenticator apps
- Verify 6-digit OTP codes
- Generate backup codes for account recovery
- Implement 2FA enable/disable flow

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [2FA with Node.js - Ben Awad](https://www.youtube.com/watch?v=rM3dxBpL1YY) |
| 💻 **Working Code** | [github.com/speakeasyjs/speakeasy](https://github.com/speakeasyjs/speakeasy) |
| 📖 **Documentation** | [TOTP Specification](https://tools.ietf.org/html/rfc6238) |
| ✍️ **Step-by-Step** | [Twilio 2FA Guide](https://www.twilio.com/blog/two-factor-authentication-node-js) |

#### 📍 Implementation Steps
```javascript
1. Generate secret: speakeasy.generateSecret()
2. Create QR code: QRCode.toDataURL(otpauth_url)
3. Verify token: speakeasy.totp.verify({ secret, token })
4. Store backup codes (8-10 codes per user)
```

---

### 5. 🔒 Rate Limiting & DDoS Protection

**Difficulty:** ⭐⭐⭐ | **Time:** 3 hours | **Dependencies:** express-rate-limit, redis, slow-down

#### 📝 Description
Build a comprehensive rate limiting system with different tiers for authenticated vs unauthenticated users. Store limits in Redis for distributed systems.

#### 🎯 Learning Objectives
- Implement sliding window rate limiting
- Create user tiers (free: 100/day, premium: 10,000/day)
- Store counters in Redis for performance
- Add custom headers (X-RateLimit-*)
- Implement IP-based and user-based limits

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Rate Limiting - Fireship](https://www.youtube.com/watch?v=9WIvfxVLH8E) |
| 💻 **Working Code** | [github.com/express-rate-limit/express-rate-limit](https://github.com/express-rate-limit/express-rate-limit) |
| 📖 **Documentation** | [Rate Limiting Strategies](https://cloud.google.com/architecture/rate-limiting-strategies-techniques) |
| ✍️ **Step-by-Step** | [LogRocket Rate Limiting Guide](https://blog.logrocket.com/rate-limiting-node-js/) |

#### 📍 Rate Limit Configuration
```javascript
// Unauthenticated: 100 requests per hour
// Authenticated (free): 1000 requests per day
// Premium: 10,000 requests per day
// Login endpoint: 5 attempts per 15 minutes
// Sensitive endpoints: Stricter limits
```

---

## Projects 6-10: Real-Time & WebSockets

---

### 6. 💬 Real-Time Chat Application

**Difficulty:** ⭐⭐⭐ | **Time:** 4 hours | **Dependencies:** socket.io, express, redis (for scaling)

#### 📝 Description
Build a real-time chat server with multiple rooms, typing indicators, online/offline status, and message history. Support private messaging and file sharing.

#### 🎯 Learning Objectives
- Implement WebSocket connections with Socket.io
- Create and manage chat rooms
- Show typing indicators in real-time
- Track online users and status
- Store message history in database

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Socket.io Chat - Traversy Media](https://www.youtube.com/watch?v=jD7F3IWHjZA) |
| 💻 **Working Code** | [github.com/socketio/socket.io/tree/main/examples/chat](https://github.com/socketio/socket.io/tree/main/examples/chat) |
| 📖 **Documentation** | [Socket.io Official Docs](https://socket.io/docs/v4/) |
| ✍️ **Step-by-Step** | [freeCodeCamp Chat App](https://www.freecodecamp.org/news/how-to-build-a-chat-application-using-socket-io-and-node-js/) |

#### 📍 Features to Implement
```javascript
// Events to handle:
- connection / disconnect
- join-room / leave-room
- send-message / receive-message
- typing-start / typing-stop
- user-online / user-offline
```

---

### 7. 🎮 Real-Time Leaderboard with Redis

**Difficulty:** ⭐⭐⭐ | **Time:** 3 hours | **Dependencies:** redis, socket.io

#### 📝 Description
Create a real-time gaming leaderboard using Redis Sorted Sets. Players submit scores and rankings update instantly across all connected clients.

#### 🎯 Learning Objectives
- Use Redis Sorted Sets for leaderboards
- Implement score updates in O(log N) time
- Add time-based leaderboards (daily/weekly/monthly)
- Handle ties with timestamp ordering
- Real-time WebSocket updates

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Redis Leaderboards - Redis Official](https://www.youtube.com/watch?v=4MZRPMpxjEE) |
| 💻 **Working Code** | [github.com/redis/node-redis/tree/master/examples](https://github.com/redis/node-redis/tree/master/examples) |
| 📖 **Documentation** | [Redis Sorted Sets](https://redis.io/docs/data-types/sorted-sets/) |
| ✍️ **Step-by-Step** | [Leaderboard Tutorial](https://www.section.io/engineering-education/building-a-real-time-leaderboard-system-with-nodejs-and-redis/) |

#### 📍 Redis Commands to Use
```redis
ZADD leaderboard:daily 1000 "player1"   # Add score
ZREVRANGE leaderboard:daily 0 9        # Get top 10
ZRANK leaderboard:daily "player1"      # Get player rank
ZINCRBY leaderboard:daily 50 "player1" # Increment score
```

---

### 8. 📊 Live Notification System

**Difficulty:** ⭐⭐⭐ | **Time:** 4 hours | **Dependencies:** socket.io, bull, nodemailer

#### 📝 Description
Build a real-time notification system that supports in-app, email, and push notifications. Queue notifications for delivery and handle retries.

#### 🎯 Learning Objectives
- Create notification templates
- Implement real-time delivery via WebSockets
- Queue notifications with Bull
- Handle delivery status and retries
- Mark notifications as read/unread

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Notification System - Hussein Nasser](https://www.youtube.com/watch?v=Cc0tJNmDdqQ) |
| 💻 **Working Code** | [github.com/notifirehq/notifire/tree/main/examples](https://github.com/notifirehq/notifire/tree/main/examples) |
| 📖 **Documentation** | [Bull Queue Docs](https://docs.bullmq.io/) |
| ✍️ **Step-by-Step** | [Building Notification System](https://dev.to/davidcallanan/how-to-build-a-real-time-notification-system-using-node-js-express-and-socket-io-2jc4) |

#### 📍 Notification Types
```javascript
- In-app notifications (WebSocket)
- Email notifications (Nodemailer)
- Push notifications (future: Firebase)
- SMS notifications (future: Twilio)
```

---

### 9. 🎥 Live Streaming with WebRTC

**Difficulty:** ⭐⭐⭐⭐ | **Time:** 5 hours | **Dependencies:** socket.io, simple-peer

#### 📝 Description
Build a peer-to-peer video streaming application using WebRTC. Support one-to-one video calls with signaling server for connection negotiation.

#### 🎯 Learning Objectives
- Understand WebRTC protocol and ICE candidates
- Implement signaling server with Socket.io
- Handle peer connection establishment
- Stream video/audio between peers
- Add mute/unmute and video on/off controls

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [WebRTC Tutorial - Fireship](https://www.youtube.com/watch?v=WmR9IMUD_CY) |
| 💻 **Working Code** | [github.com/feross/simple-peer/tree/master/examples](https://github.com/feross/simple-peer/tree/master/examples) |
| 📖 **Documentation** | [WebRTC Official Docs](https://webrtc.org/getting-started/overview) |
| ✍️ **Step-by-Step** | [MDN WebRTC Guide](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Signaling_and_video_calling) |

---

### 10. 🔄 Collaborative Document Editor

**Difficulty:** ⭐⭐⭐⭐ | **Time:** 5 hours | **Dependencies:** socket.io, mongoose, diff-match-patch

#### 📝 Description
Build a real-time collaborative document editor similar to Google Docs. Multiple users can edit simultaneously with operational transformation (OT) or Conflict-Free Replicated Data Types (CRDTs).

#### 🎯 Learning Objectives
- Implement operational transformation for conflict resolution
- Track document versions and changes
- Show active cursors of other users
- Handle connection drops and reconnection
- Save document versions for history

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [CRDT Explained - Tom Scott](https://www.youtube.com/watch?v=DEcwa68f-jY) |
| 💻 **Working Code** | [github.com/ether/etherpad-lite](https://github.com/ether/etherpad-lite) |
| 📖 **Documentation** | [Operational Transformation](https://en.wikipedia.org/wiki/Operational_transformation) |
| ✍️ **Step-by-Step** | [Building Google Docs Clone](https://www.freecodecamp.org/news/building-a-real-time-collaborative-editor-with-node-js-and-react/) |

---

## Projects 11-15: Performance & Architecture

---

### 11. 🚀 Redis Caching Layer

**Difficulty:** ⭐⭐⭐ | **Time:** 3 hours | **Dependencies:** redis, node-cache

#### 📝 Description
Add Redis caching to an existing API to dramatically improve performance. Implement cache invalidation strategies, cache-aside pattern, and TTL management.

#### 🎯 Learning Objectives
- Implement cache-aside pattern
- Set appropriate TTL for different data types
- Handle cache invalidation on updates
- Cache expensive database queries
- Implement write-through and write-behind caching

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Redis Caching - Web Dev Simplified](https://www.youtube.com/watch?v=oaJq1m4z8pM) |
| 💻 **Working Code** | [github.com/redis/node-redis-caching-example](https://github.com/redis/node-redis-caching-example) |
| 📖 **Documentation** | [Caching Patterns](https://redis.com/solutions/use-cases/caching/) |
| ✍️ **Step-by-Step** | [DigitalOcean Redis Caching](https://www.digitalocean.com/community/tutorials/how-to-implement-caching-in-node-js-using-redis) |

#### 📍 Caching Strategy
```javascript
// Cache TTL examples
User profiles: 1 hour
Product catalog: 30 minutes
Session data: 24 hours
API responses: 5 minutes
Leaderboard: 10 seconds (real-time)
```

---

### 12. 📦 Background Job Queue (BullMQ)

**Difficulty:** ⭐⭐⭐⭐ | **Time:** 4 hours | **Dependencies:** bullmq, redis

#### 📝 Description
Build a robust background job processing system for tasks like email sending, image resizing, PDF generation, and report exports. Add job priorities, retries, and progress tracking.

#### 🎯 Learning Objectives
- Implement job queues with BullMQ
- Add job priorities (high, medium, low)
- Configure retry strategies with exponential backoff
- Track job progress and status
- Add worker concurrency and rate limiting

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [BullMQ Tutorial - Taskforce.sh](https://www.youtube.com/watch?v=WZn2G3Df9SI) |
| 💻 **Working Code** | [github.com/taskforcesh/bullmq/tree/master/examples](https://github.com/taskforcesh/bullmq/tree/master/examples) |
| 📖 **Documentation** | [BullMQ Official Docs](https://docs.bullmq.io/) |
| ✍️ **Step-by-Step** | [Queue System Guide](https://blog.logrocket.com/background-jobs-node-js-bullmq/) |

#### 📍 Job Examples
```javascript
- Email sending (retry 3 times, 5 min delay)
- Image resizing (concurrency: 5 workers)
- PDF report generation (priority: based on user tier)
- Data export (chunked processing)
```

---

### 13. 🏗️ API Gateway with Gateway

**Difficulty:** ⭐⭐⭐⭐ | **Time:** 5 hours | **Dependencies:** express-gateway, http-proxy-middleware

#### 📝 Description
Build an API Gateway that routes requests to multiple microservices. Add authentication, rate limiting, request logging, and response aggregation.

#### 🎯 Learning Objectives
- Implement reverse proxy with routing rules
- Add JWT authentication at gateway level
- Aggregate responses from multiple services
- Implement circuit breaker pattern
- Add request/response transformation

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [API Gateway - Hussein Nasser](https://www.youtube.com/watch?v=6ULyxuHKxg8) |
| 💻 **Working Code** | [github.com/express-gateway/express-gateway/tree/master/examples](https://github.com/express-gateway/express-gateway/tree/master/examples) |
| 📖 **Documentation** | [API Gateway Pattern](https://microservices.io/patterns/apigateway.html) |
| ✍️ **Step-by-Step** | [Build API Gateway Guide](https://blog.logrocket.com/building-an-api-gateway-using-node-js/) |

#### 📍 Routing Rules
```javascript
/users/*    → User Service (port 3001)
/products/* → Product Service (port 3002)
/orders/*   → Order Service (port 3003)
/payments/* → Payment Service (port 3004)
```

---

### 14. 📊 Request Logger & Analytics

**Difficulty:** ⭐⭐⭐ | **Time:** 3 hours | **Dependencies:** winston, morgan, elasticsearch

#### 📝 Description
Build a comprehensive logging and analytics system. Log all API requests with response times, status codes, and user agents. Store in Elasticsearch for querying.

#### 🎯 Learning Objectives
- Implement structured logging with Winston
- Create different log levels (error, warn, info, debug)
- Log request/response cycles
- Calculate response time percentiles (p50, p95, p99)
- Build a simple analytics dashboard

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Winston Logging - Academind](https://www.youtube.com/watch?v=9kynYHBqyB8) |
| 💻 **Working Code** | [github.com/winstonjs/winston/tree/master/examples](https://github.com/winstonjs/winston/tree/master/examples) |
| 📖 **Documentation** | [Winston Docs](https://github.com/winstonjs/winston) |
| ✍️ **Step-by-Step** | [Logging Best Practices](https://betterstack.com/community/guides/logging/nodejs-logging-best-practices/) |

#### 📊 Metrics to Track
```javascript
- Total requests per endpoint
- Average response time
- Error rate (4xx, 5xx)
- Top users by request count
- Slowest endpoints (p99)
```

---

### 15. 🔁 Webhook Delivery System

**Difficulty:** ⭐⭐⭐⭐ | **Time:** 5 hours | **Dependencies:** bullmq, axios, crypto

#### 📝 Description
Build a production webhook delivery system that sends events to external services. Include retries, exponential backoff, idempotency keys, signature verification, and delivery logs.

#### 🎯 Learning Objectives
- Implement reliable webhook delivery with retries
- Add HMAC-SHA256 signature verification
- Handle idempotency (prevent duplicate processing)
- Build webhook dashboard for debugging
- Add endpoint health checking

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Webhook System - Webhook Relay](https://www.youtube.com/watch?v=cq6l3u1_GTA) |
| 💻 **Working Code** | [github.com/svix/svix-webhooks/tree/main/examples](https://github.com/svix/svix-webhooks/tree/main/examples) |
| 📖 **Documentation** | [Webhook Best Practices](https://webhookrelay.com/blog/2020/04/20/building-webhook-delivery-system/) |
| ✍️ **Step-by-Step** | [Building Reliable Webhooks](https://dev.to/microsoft/building-reliable-webhook-delivery-in-node-js-3fno) |

#### 📍 Webhook Features
```javascript
- Retry policy: 5 retries (1s, 5s, 30s, 2m, 10m)
- Idempotency keys (prevent duplicates)
- Signature header: X-Webhook-Signature
- Delivery logs and debug information
- Endpoint health monitoring
```

---

## 📖 Project Reference Index

### Complete Working Examples (Multi-Project)

| Repository | Description | Link |
|------------|-------------|------|
| **Node.js API Boilerplate** | Production-ready auth & structure | [github.com/hagopj13/node-express-boilerplate](https://github.com/hagopj13/node-express-boilerplate) |
| **RealWorld Examples** | Full-stack medium clone | [github.com/gothinkster/node-express-realworld-example-app](https://github.com/gothinkster/node-express-realworld-example-app) |
| **Socket.io Chat** | Complete chat application | [github.com/socketio/socket.io/tree/main/examples/chat](https://github.com/socketio/socket.io/tree/main/examples/chat) |
| **BullMQ Examples** | Job queue implementations | [github.com/taskforcesh/bullmq/tree/master/examples](https://github.com/taskforcesh/bullmq/tree/master/examples) |

### Free Learning Resources

| Platform | Focus | Link |
|----------|-------|------|
| **Node.js Design Patterns** | Advanced architecture | [github.com/Fedosejev/node-js-design-patterns](https://github.com/Fedosejev/node-js-design-patterns) |
| **System Design Primer** | Large-scale design | [github.com/donnemartin/system-design-primer](https://github.com/donnemartin/system-design-primer) |
| **Redis University** | Free Redis courses | [university.redis.com](https://university.redis.com) |
| **Socket.io Academy** | Real-time courses | [socket.io/academy](https://socket.io/academy) |

### YouTube Channels for Intermediate Topics

| Creator | Focus | Link |
|---------|-------|------|
| **Hussein Nasser** | Backend architecture | [youtube.com/@HusseinNasser-js](https://youtube.com/@HusseinNasser-js) |
| **Ben Awad** | Full-stack + Auth | [youtube.com/@bawd](https://youtube.com/@bawd) |
| **Fireship** | Modern tech concepts | [youtube.com/@Fireship](https://youtube.com/@Fireship) |
| **Theo - t3.gg** | Production patterns | [youtube.com/@t3dotgg](https://youtube.com/@t3dotgg) |

### Required Dependencies Summary

```json
{
  "authentication": ["bcrypt", "jsonwebtoken", "passport", "speakeasy"],
  "real-time": ["socket.io", "socket.io-client"],
  "caching-queues": ["redis", "bullmq", "node-cache"],
  "logging": ["winston", "morgan", "pino"],
  "database": ["mongoose", "pg", "sequelize", "prisma"],
  "security": ["helmet", "cors", "express-rate-limit", "compression"]
}
```

---

## ✅ Completion Tracker

```markdown
## Intermediate Projects Progress

### Authentication & Security (1-5)
- [ ] Project 1: Complete JWT Authentication System
- [ ] Project 2: OAuth 2.0 Integration (Google & GitHub)
- [ ] Project 3: Email Verification & Password Reset
- [ ] Project 4: Two-Factor Authentication (2FA)
- [ ] Project 5: Rate Limiting & DDoS Protection

### Real-Time & WebSockets (6-10)
- [ ] Project 6: Real-Time Chat Application
- [ ] Project 7: Real-Time Leaderboard with Redis
- [ ] Project 8: Live Notification System
- [ ] Project 9: Live Streaming with WebRTC
- [ ] Project 10: Collaborative Document Editor

### Performance & Architecture (11-15)
- [ ] Project 11: Redis Caching Layer
- [ ] Project 12: Background Job Queue (BullMQ)
- [ ] Project 13: API Gateway with Gateway
- [ ] Project 14: Request Logger & Analytics
- [ ] Project 15: Webhook Delivery System

### Total Progress: 0/15 completed
```

---

## 🎯 What's Next?

After completing these 15 intermediate projects, you'll be ready for:

- **Advanced Projects** (Coming soon) - 15 projects including:
  - Microservices architecture with Docker/K8s
  - GraphQL federation
  - Real-time streaming with Kafka
  - Machine learning API integration
  - Multi-region deployment
  - Serverless functions
  - Distributed tracing with Jaeger
  - Chaos engineering

---

## 🤝 Need Help?

- **Discord Community:** [Join our intermediate backend server](https://discord.gg/intermediate-backend)
- **GitHub Discussions:** [Ask architecture questions](https://github.com/your-repo/discussions/categories/intermediate)
- **Stack Overflow:** Tag with `#node.js-intermediate`

---

**⭐ Star this repository** if these intermediate projects help you level up!

**Happy Coding! 🚀**
```
