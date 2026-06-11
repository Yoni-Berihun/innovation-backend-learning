

```markdown
# 🟢 Beginner Backend Projects - 15 Hands-On Exercises

A carefully curated collection of **15 beginner-friendly backend projects** to master Node.js and Express fundamentals. Each project includes detailed descriptions, learning objectives, and direct links to working examples and tutorials.

---

## 📋 Table of Contents

- [Quick Start Guide](#quick-start-guide)
- [Projects 1-5: Core Node.js Fundamentals](#projects-1-5-core-nodejs-fundamentals)
- [Projects 6-10: Express.js & REST APIs](#projects-6-10-expressjs--rest-apis)
- [Projects 11-15: Databases & Authentication](#projects-11-15-databases--authentication)
- [Project Reference Index](#project-reference-index)
- [Completion Tracker](#completion-tracker)

---

## 🚀 Quick Start Guide

```bash
# For each project, follow this pattern:
mkdir project-name
cd project-name
npm init -y
npm install [dependencies]
# Then follow the tutorial links below
```

---

## Projects 1-5: Core Node.js Fundamentals

---

### 1. 🤝 Hello World HTTP Server

**Difficulty:** ⭐ | **Time:** 30 mins | **Dependencies:** none

#### 📝 Description
Build your first web server using Node.js built-in `http` module (no frameworks!). This project teaches you how servers work under the hood.

#### 🎯 Learning Objectives
- Understand Node.js `http` module
- Work with request/response objects
- Set HTTP status codes and headers
- Basic routing implementation

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Node.js HTTP Module Tutorial - YouTube](https://www.youtube.com/watch?v=VShtPwqKvI0) |
| 💻 **Working Code** | [github.com/nodejs/examples/tree/master/http](https://github.com/nodejs/examples/tree/master/http) |
| 📖 **Documentation** | [Node.js Official HTTP Docs](https://nodejs.org/api/http.html) |
| ✍️ **Step-by-Step** | [freeCodeCamp HTTP Server Guide](https://www.freecodecamp.org/news/how-to-create-a-web-server-in-node-js/) |

#### ✅ Expected Output
```bash
# Run: node server.js
Server running at http://localhost:3000/

# Visit localhost:3000
Hello World!

# Visit localhost:3000/about
This is the about page
```

---

### 2. 📁 File System Explorer

**Difficulty:** ⭐ | **Time:** 1 hour | **Dependencies:** none

#### 📝 Description
Create a command-line tool that reads, writes, creates, and deletes files using Node.js `fs` module. Build a simple file explorer that lists directory contents.

#### 🎯 Learning Objectives
- Master synchronous vs asynchronous file operations
- Work with `fs.readdir()`, `fs.readFile()`, `fs.writeFile()`
- Handle file paths using `path` module
- Error handling with try-catch

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Node.js File System Tutorial - Programming with Mosh](https://www.youtube.com/watch?v=U57dyk4fVcw) |
| 💻 **Working Code** | [github.com/joeferner/node-fs-extra/examples](https://github.com/jprichardson/node-fs-extra/tree/master/examples) |
| 📖 **Documentation** | [Node.js fs Module Docs](https://nodejs.org/api/fs.html) |
| ✍️ **Step-by-Step** | [DigitalOcean fs Guide](https://www.digitalocean.com/community/tutorials/how-to-work-with-files-in-node-js) |

#### 💡 Sample Commands to Build
```bash
node explorer.js list ./downloads
node explorer.js read myfile.txt
node explorer.js write newfile.txt "Hello World"
node explorer.js delete oldfile.txt
```

---

### 3. 🌡️ Simple CLI Weather Tool

**Difficulty:** ⭐ | **Time:** 1.5 hours | **Dependencies:** axios, dotenv

#### 📝 Description
Build a command-line weather application that fetches real weather data from OpenWeatherMap API. Accept city name as input and display temperature, humidity, and conditions.

#### 🎯 Learning Objectives
- Make HTTP requests using Axios
- Work with external REST APIs
- Handle API keys with environment variables
- Parse and display JSON responses

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Build a CLI Weather App - freeCodeCamp](https://www.youtube.com/watch?v=JLvxVW05O1o) |
| 💻 **Working Code** | [github.com/bmorelli25/weather-cli](https://github.com/bmorelli25/weather-cli) |
| 📖 **Documentation** | [OpenWeatherMap API Docs](https://openweathermap.org/current) |
| ✍️ **Step-by-Step** | [Twilio Weather App Guide](https://www.twilio.com/blog/build-weather-api-node-js) |

#### 🔑 Get Your Free API Key
Sign up at [OpenWeatherMap](https://openweathermap.org/api) - Free tier: 1000 calls/day

---

### 4. 📝 Note Taking CLI App

**Difficulty:** ⭐⭐ | **Time:** 2 hours | **Dependencies:** chalk, yargs

#### 📝 Description
Create a command-line note-taking application with add, remove, list, and read functionality. Notes persist in a JSON file.

#### 🎯 Learning Objectives
- Parse command-line arguments using yargs/commander
- Add colorful console output with chalk
- Implement CRUD operations in CLI
- Work with JSON file storage

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Node.js CLI Note App - The Net Ninja](https://www.youtube.com/watch?v=wIuB2yN9C1o&list=PL4cUxeGkcC9gcy9lrvMJ75i9nWzb2PegF) |
| 💻 **Working Code** | [github.com/andrewjmead/node-course/tree/master/notes-app](https://github.com/andrewjmead/node-course/tree/master/notes-app) |
| 📖 **Documentation** | [Yargs Documentation](http://yargs.js.org/) |
| ✍️ **Step-by-Step** | [freeCodeCamp CLI App Guide](https://www.freecodecamp.org/news/how-to-build-a-command-line-application-with-node-js/) |

#### 💡 Commands to Implement
```bash
node app.js add --title="Buy milk" --body="Get 2% milk from store"
node app.js remove --title="Buy milk"
node app.js list
node app.js read --title="Buy milk"
```

---

### 5. 🎲 Password Generator

**Difficulty:** ⭐ | **Time:** 1 hour | **Dependencies:** none

#### 📝 Description
Build a command-line password generator that creates random, secure passwords with customizable length and character types (uppercase, lowercase, numbers, symbols).

#### 🎯 Learning Objectives
- Work with Node.js `crypto` module for secure randomness
- Accept command-line flags using `process.argv`
- String manipulation and character sets
- Create reusable modules with `module.exports`

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Password Generator - Web Dev Simplified](https://www.youtube.com/watch?v=3LuV5xxJ7nY) |
| 💻 **Working Code** | [github.com/leonardomso/password-generator](https://github.com/leonardomso/password-generator) |
| 📖 **Documentation** | [Node.js Crypto Module](https://nodejs.org/api/crypto.html) |
| ✍️ **Step-by-Step** | [Better Programming Guide](https://betterprogramming.pub/build-a-password-generator-with-node-js-8c2b2b0b4f0a) |

#### 💻 Usage Example
```bash
node generator.js --length=16 --numbers --symbols
# Output: K#9mP$2n@qR!5vX&
```

---

## Projects 6-10: Express.js & REST APIs

---

### 6. 🏪 Basic REST API (Products)

**Difficulty:** ⭐⭐ | **Time:** 2 hours | **Dependencies:** express

#### 📝 Description
Build your first REST API using Express.js to manage a product catalog. Implement all CRUD operations (GET, POST, PUT, DELETE) with in-memory storage.

#### 🎯 Learning Objectives
- Understand REST API principles
- Handle different HTTP methods
- Work with route parameters and query strings
- Use Postman for API testing

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [REST API Crash Course - Traversy Media](https://www.youtube.com/watch?v=Q4Vr959jF6Y) |
| 💻 **Working Code** | [github.com/expressjs/express/tree/master/examples](https://github.com/expressjs/express/tree/master/examples) |
| 📖 **Documentation** | [Express.js Official Guide](https://expressjs.com/en/starter/hello-world.html) |
| ✍️ **Step-by-Step** | [MDN REST API Tutorial](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs) |
| 🧪 **Testing Tool** | [Postman Download](https://www.postman.com/downloads/) |

#### 📍 API Endpoints to Build
```http
GET    /api/products         - List all products
GET    /api/products/:id     - Get single product
POST   /api/products         - Create new product
PUT    /api/products/:id     - Update product
DELETE /api/products/:id     - Delete product
```

---

### 7. ✏️ To-Do List API

**Difficulty:** ⭐⭐ | **Time:** 2 hours | **Dependencies:** express, uuid

#### 📝 Description
Create a task management API where each task has an ID, title, description, status (pending/completed), priority, and timestamps.

#### 🎯 Learning Objectives
- Generate unique IDs using UUID
- Add filtering and sorting capabilities
- Implement search functionality
- Validate request data

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Todo API Tutorial - Web Dev Simplified](https://www.youtube.com/watch?v=l8WPWK9mS5M) |
| 💻 **Working Code** | [github.com/amejiarosario/todoAPI](https://github.com/amejiarosario/todoAPI) |
| 📖 **Documentation** | [UUID npm Package](https://www.npmjs.com/package/uuid) |
| ✍️ **Step-by-Step** | [BezKoder Todo Guide](https://www.bezkoder.com/node-express-tutorial/) |

#### 🎯 Bonus Features
- Filter by status: `/api/tasks?status=pending`
- Search: `/api/tasks?search=keyword`
- Sort by date: `/api/tasks?sort=createdAt`

---

### 8. 🎯 URL Shortener

**Difficulty:** ⭐⭐ | **Time:** 2.5 hours | **Dependencies:** express, nanoid

#### 📝 Description
Build a URL shortener service similar to bit.ly. Convert long URLs to short codes and redirect users to original URLs.

#### 🎯 Learning Objectives
- Generate unique short codes
- Handle HTTP redirects (301/302)
- Store mappings in memory/JSON
- Track click statistics

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [URL Shortener - Traversy Media](https://www.youtube.com/watch?v=SLpUKAGnm-g) |
| 💻 **Working Code** | [github.com/bytebrat/url-shortener](https://github.com/bytebrat/url-shortener) |
| 📖 **Documentation** | [NanoID Documentation](https://github.com/ai/nanoid) |
| ✍️ **Step-by-Step** | [DigitalOcean URL Shortener](https://www.digitalocean.com/community/tutorials/how-to-build-a-url-shortener-with-node-js-express-and-mongodb) |

#### 📍 Example Flow
```http
POST /shorten
Body: { "url": "https://very-long-url.com/blog/article-123" }
Response: { "shortCode": "abc123", "shortUrl": "http://localhost:3000/abc123" }

GET /abc123
Redirects to: https://very-long-url.com/blog/article-123
```

---

### 9. 📧 Contact Form API

**Difficulty:** ⭐⭐ | **Time:** 2 hours | **Dependencies:** express, nodemailer

#### 📝 Description
Create an API endpoint for contact forms that validates input and sends emails. Perfect for portfolio websites.

#### 🎯 Learning Objectives
- Validate email addresses and form data
- Send emails using Nodemailer
- Create email templates
- Prevent spam with rate limiting

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Nodemailer Tutorial - Academind](https://www.youtube.com/watch?v=-RCnNyD0L-s) |
| 💻 **Working Code** | [github.com/nodemailer/nodemailer/examples](https://github.com/nodemailer/nodemailer/tree/master/examples) |
| 📖 **Documentation** | [Nodemailer Docs](https://nodemailer.com/about/) |
| ✍️ **Step-by-Step** | [freeCodeCamp Email Guide](https://www.freecodecamp.org/news/use-nodemailer-to-send-emails-from-your-node-js-server/) |

#### 📝 API Endpoint
```http
POST /api/contact
Body: {
  "name": "John Doe",
  "email": "john@example.com",
  "subject": "Question about product",
  "message": "I need help with..."
}
Response: { "success": true, "message": "Email sent!" }
```

---

### 10. 📊 Quote Generator API

**Difficulty:** ⭐ | **Time:** 1.5 hours | **Dependencies:** express

#### 📝 Description
Build an API that serves inspirational quotes. Include random quotes, quotes by category, and daily quote features.

#### 🎯 Learning Objectives
- Serve static JSON data
- Implement random selection logic
- Add query parameters for filtering
- Cache responses for performance

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Build a Quote API - Code With Harry](https://www.youtube.com/watch?v=Xbp5rJq3T3M) |
| 💻 **Working Code** | [github.com/lukePeavey/quotable](https://github.com/lukePeavey/quotable) |
| 📖 **Documentation** | [Express Routing Guide](https://expressjs.com/en/guide/routing.html) |
| ✍️ **Step-by-Step** | [RapidAPI Quote Guide](https://rapidapi.com/blog/build-quote-generator-api/) |

#### 📍 API Endpoints
```http
GET /api/quotes/random          - Get random quote
GET /api/quotes/category/motivation  - Quotes by category
GET /api/quotes/today           - Quote of the day
GET /api/quotes?author=Einstein - Filter by author
```

---

## Projects 11-15: Databases & Authentication

---

### 11. 📚 Blog API with MongoDB

**Difficulty:** ⭐⭐⭐ | **Time:** 3 hours | **Dependencies:** express, mongoose

#### 📝 Description
Build a blog platform API with posts, comments, and categories using MongoDB database for permanent storage.

#### 🎯 Learning Objectives
- Connect to MongoDB (Atlas or local)
- Design Mongoose schemas and models
- Perform database CRUD operations
- Implement population (referencing documents)

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [MongoDB with Node.js - The Net Ninja](https://www.youtube.com/watch?v=9OPP_1eAENg&list=PL4cUxeGkcC9h77dJ-QJlwGlZlTd4ecZOA) |
| 💻 **Working Code** | [github.com/adnanrahic/rest-api-nodejs-mongodb](https://github.com/adnanrahic/rest-api-nodejs-mongodb) |
| 📖 **Documentation** | [Mongoose Docs](https://mongoosejs.com/docs/guide.html) |
| ✍️ **Step-by-Step** | [MongoDB Official Tutorial](https://www.mongodb.com/developer/languages/javascript/nodejs-blog-api-tutorial/) |
| ☁️ **Free DB** | [MongoDB Atlas (Free Tier)](https://www.mongodb.com/cloud/atlas) |

#### 📊 Database Models
```javascript
Post: title, content, author, category, tags, createdAt
Comment: postId, author, content, createdAt
Category: name, description
```

---

### 12. 🔐 User Registration & Login API

**Difficulty:** ⭐⭐⭐ | **Time:** 3 hours | **Dependencies:** express, bcrypt, jsonwebtoken

#### 📝 Description
Create a complete authentication API with user registration, login, and protected routes using JWT tokens.

#### 🎯 Learning Objectives
- Hash passwords with bcrypt
- Generate and verify JWT tokens
- Create authentication middleware
- Protect routes from unauthorized access

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [JWT Authentication - Traversy Media](https://www.youtube.com/watch?v=7nafaH9SddU) |
| 💻 **Working Code** | [github.com/hagopj13/node-express-boilerplate](https://github.com/hagopj13/node-express-boilerplate) |
| 📖 **Documentation** | [JWT.io Docs](https://jwt.io/introduction) |
| ✍️ **Step-by-Step** | [BezKoder Auth Tutorial](https://www.bezkoder.com/node-js-jwt-authentication-mysql/) |

#### 🔑 API Endpoints
```http
POST /api/auth/register   - Create new account
POST /api/auth/login      - Login and get token
GET  /api/profile         - Get user profile (protected)
PUT  /api/profile         - Update profile (protected)
```

---

### 13. 🐘 User Management with PostgreSQL

**Difficulty:** ⭐⭐⭐ | **Time:** 3 hours | **Dependencies:** express, pg, sequelize

#### 📝 Description
Build a user management API using PostgreSQL database and Sequelize ORM. Learn relational database concepts.

#### 🎯 Learning Objectives
- Set up PostgreSQL database
- Use Sequelize ORM for models
- Write complex SQL queries
- Understand relationships (one-to-many)

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [PostgreSQL with Node.js - Programming with Mosh](https://www.youtube.com/watch?v=3xXc4j1nI3E) |
| 💻 **Working Code** | [github.com/sequelize/express-example](https://github.com/sequelize/express-example) |
| 📖 **Documentation** | [Sequelize Docs](https://sequelize.org/docs/v6/) |
| ✍️ **Step-by-Step** | [PostgreSQL Tutorial](https://www.postgresqltutorial.com/postgresql-nodejs/) |
| 🐘 **Installation** | [Download PostgreSQL](https://www.postgresql.org/download/) |

#### 📊 Database Schema
```sql
Users: id, username, email, password_hash, role, created_at
Products: id, name, price, stock, user_id (foreign key)
Orders: id, user_id, total_amount, status, created_at
```

---

### 14. 📸 Image Upload API

**Difficulty:** ⭐⭐ | **Time:** 2 hours | **Dependencies:** express, multer, sharp

#### 📝 Description
Create an API that accepts image uploads, compresses them, and serves them with different sizes (thumbnail, medium, original).

#### 🎯 Learning Objectives
- Handle multipart/form-data uploads with Multer
- Validate file types and sizes
- Resize and optimize images with Sharp
- Serve static files securely

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [File Upload with Multer - Web Dev Simplified](https://www.youtube.com/watch?v=srPXMt1Q0nY) |
| 💻 **Working Code** | [github.com/expressjs/multer/tree/master/examples](https://github.com/expressjs/multer/tree/master/examples) |
| 📖 **Documentation** | [Sharp Documentation](https://sharp.pixelplumbing.com/) |
| ✍️ **Step-by-Step** | [DigitalOcean Upload Guide](https://www.digitalocean.com/community/tutorials/express-file-upload) |

#### 📍 API Endpoints
```http
POST /api/upload          - Upload single image
POST /api/upload/multiple - Upload multiple images
GET  /api/images/:filename - Get original image
GET  /api/images/:filename?size=thumbnail - Get resized version
DELETE /api/images/:filename - Delete image
```

---

### 15. 🏆 Simple Forum API

**Difficulty:** ⭐⭐⭐ | **Time:** 4 hours | **Dependencies:** express, mongoose, jsonwebtoken

#### 📝 Description
Build a forum/discussion board API with users, topics, posts, and replies. Include voting functionality for posts.

#### 🎯 Learning Objectives
- Design complex database relationships
- Implement nested comment threads
- Add upvote/downvote system
- Create search functionality across posts

#### 🔗 Reference Links

| Type | Link |
|------|------|
| 📺 **Tutorial** | [Forum API - Ben Awad](https://www.youtube.com/watch?v=7kdR2pIwtfE) |
| 💻 **Working Code** | [github.com/sahat/hackathon-starter](https://github.com/sahat/hackathon-starter) |
| 📖 **Documentation** | [MongoDB Aggregation](https://www.mongodb.com/docs/manual/aggregation/) |
| ✍️ **Step-by-Step** | [Forum API Guide](https://dev.to/ericchapman/building-a-forum-api-with-nodejs-express-and-mongodb-part-1-1l2o) |

#### 📊 Features to Build
- User registration and profiles
- Create discussion topics
- Post replies to topics
- Upvote/downvote system
- Search by title or content
- Sort by recent or popular

---

## 📖 Project Reference Index

### Complete Working Examples (Multi-Project)

| Repository | Description | Link |
|------------|-------------|------|
| **Node.js Crash Course** | Complete beginner projects collection | [github.com/john-smilga/node-express-course](https://github.com/john-smilga/node-express-course) |
| **Hackathon Starter** | Boilerplate with auth, API, and examples | [github.com/sahat/hackathon-starter](https://github.com/sahat/hackathon-starter) |
| **RealWorld Examples** | Production-ready backend examples | [github.com/gothinkster/realworld](https://github.com/gothinkster/realworld) |
| **Node.js Best Practices** | Project structure examples | [github.com/goldbergyoni/nodebestpractices](https://github.com/goldbergyoni/nodebestpractices) |

### Free Learning Platforms

| Platform | Focus | Link |
|----------|-------|------|
| **freeCodeCamp** | Backend curriculum (300+ hours) | [freecodecamp.org/learn](https://www.freecodecamp.org/learn/back-end-development-and-apis/) |
| **The Odin Project** | Full-stack Node.js path | [theodinproject.com/paths/full-stack-javascript](https://www.theodinproject.com/paths/full-stack-javascript) |
| **MDN Web Docs** | Complete Express guide | [developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs) |
| **Codecademy** | Interactive Node.js course | [codecademy.com/learn/learn-node-js](https://www.codecademy.com/learn/learn-node-js) |

### YouTube Playlists (Free)

| Creator | Focus | Link |
|---------|-------|------|
| **Traversy Media** | Node.js crash courses | [youtube.com/playlist?list=PLillGF-RfqbYzXBEL9GvWb5y9x5EaA9RB](https://youtube.com/playlist?list=PLillGF-RfqbYzXBEL9GvWb5y9x5EaA9RB) |
| **The Net Ninja** | Complete Node.js tutorial | [youtube.com/playlist?list=PL4cUxeGkcC9gcy9lrvMJ75i9nWzb2PegF](https://youtube.com/playlist?list=PL4cUxeGkcC9gcy9lrvMJ75i9nWzb2PegF) |
| **Web Dev Simplified** | Advanced Node concepts | [youtube.com/@WebDevSimplified/playlists](https://youtube.com/@WebDevSimplified/playlists) |
| **Academind** | Express & MongoDB | [youtube.com/playlist?list=PL55RiY5tL51q4D-B63KBnygU6opNPFk_q](https://youtube.com/playlist?list=PL55RiY5tL51q4D-B63KBnygU6opNPFk_q) |

---

## ✅ Completion Tracker

Copy this checklist and track your progress:

```markdown
## Beginner Projects Progress

### Core Node.js (1-5)
- [ ] Project 1: Hello World HTTP Server
- [ ] Project 2: File System Explorer
- [ ] Project 3: CLI Weather Tool
- [ ] Project 4: Note Taking CLI App
- [ ] Project 5: Password Generator

### Express & REST APIs (6-10)
- [ ] Project 6: Basic REST API (Products)
- [ ] Project 7: To-Do List API
- [ ] Project 8: URL Shortener
- [ ] Project 9: Contact Form API
- [ ] Project 10: Quote Generator API

### Databases & Auth (11-15)
- [ ] Project 11: Blog API with MongoDB
- [ ] Project 12: User Registration & Login API
- [ ] Project 13: User Management with PostgreSQL
- [ ] Project 14: Image Upload API
- [ ] Project 15: Simple Forum API

### Total Progress: 0/15 completed
```

---

## 🎯 What's Next?

After completing these 15 beginner projects, you'll be ready for:

- **Intermediate Projects** (Coming soon) - 15 projects including:
  - Real-time chat with Socket.io
  - E-commerce with payments
  - Social media API
  - File management system
  - Webhook service

- **Advanced Projects** (Coming soon) - 15 projects including:
  - Microservices architecture
  - Video streaming platform
  - Recommendation engine
  - Multi-tenant SaaS

---

## 🤝 Need Help?

- **Discord Community:** [Join our backend server](https://discord.gg/backend-projects)
- **GitHub Discussions:** [Ask questions here](https://github.com/your-repo/discussions)
- **Stack Overflow:** Tag with `#nodejs-beginner-project`

---

**⭐ Star this repository** if these projects help you learn backend development!

**Happy Coding! 🚀**
```
