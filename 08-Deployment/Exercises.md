

## Environment Configuration, Production Readiness, Hosting, CI/CD

A beginner-friendly guide with **20 practice problems** covering essential deployment concepts.

---

```markdown
# 🚀 Deployment & DevOps - Practice Problems (Beginner Level)

## Table of Contents

- [Environment Configuration (Problems 1-5)](#environment-configuration-problems-1-5)
- [Production Readiness (Problems 6-10)](#production-readiness-problems-6-10)
- [Hosting Platforms (Problems 11-15)](#hosting-platforms-problems-11-15)
- [CI/CD Basics (Problems 16-20)](#cicd-basics-problems-16-20)

---

## Environment Configuration (Problems 1-5)

---

### Problem 1: Setting Up Environment Variables

#### 📝 Problem Statement
You have a Node.js app that needs different settings for development and production:
- Database URL (different for local vs production)
- API keys (should be secret, not in code)
- Port number (default 3000, can be changed)
- Debug mode (on for dev, off for production)

Create a system to manage these settings without hardcoding them in your source code.

#### 🔍 Detailed Explanation

**What are Environment Variables?**
Environment variables are values that live outside your code. They change depending on where your app runs.

**Why use them?**
- **Security** - Passwords, API keys never in code
- **Flexibility** - Same code works everywhere
- **Safety** - No risk of committing secrets to GitHub

**Common Environment Variables:**
```bash
PORT=3000
DATABASE_URL=postgresql://localhost/mydb
API_KEY=abc123secret
NODE_ENV=production
```

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Step 1: Install dotenv package
// ============================================
// Run in terminal: npm install dotenv

// ============================================
// Step 2: Create .env file (NEVER commit this!)
// ============================================
// File: .env (create in your project root)
/*
PORT=3000
DATABASE_URL=postgresql://localhost:5432/myapp
API_KEY=your-secret-key-here
NODE_ENV=development
DEBUG_MODE=true
*/

// ============================================
// Step 3: Create .env.example (commit this to git)
// ============================================
// File: .env.example (template for other developers)
/*
PORT=3000
DATABASE_URL=postgresql://localhost:5432/myapp
API_KEY=your-api-key-here
NODE_ENV=development
DEBUG_MODE=true
*/

// ============================================
// Step 4: Load environment variables in your app
// ============================================
// File: config.js
const dotenv = require('dotenv');

// Load environment variables from .env file
dotenv.config();

// Export configuration as an object
module.exports = {
    port: process.env.PORT || 3000,
    databaseUrl: process.env.DATABASE_URL,
    apiKey: process.env.API_KEY,
    nodeEnv: process.env.NODE_ENV || 'development',
    debugMode: process.env.DEBUG_MODE === 'true'
};

// ============================================
// Step 5: Use configuration in your app
// ============================================
// File: server.js
const config = require('./config');
const express = require('express');

const app = express();

console.log(`Running in ${config.nodeEnv} mode`);
console.log(`Server will start on port ${config.port}`);

app.listen(config.port, () => {
    console.log(`Server running on http://localhost:${config.port}`);
});

// ============================================
// Step 6: Different .env files for different environments
// ============================================
// File: .env.development
/*
PORT=3000
DATABASE_URL=postgresql://localhost:5432/myapp_dev
NODE_ENV=development
DEBUG_MODE=true
*/

// File: .env.production
/*
PORT=8080
DATABASE_URL=postgresql://prod-server:5432/myapp_prod
NODE_ENV=production
DEBUG_MODE=false
*/

// Load different file based on NODE_ENV
const envFile = `.env.${process.env.NODE_ENV || 'development'}`;
require('dotenv').config({ path: envFile });
```

**Important Security Rule:**
Add `.env` to your `.gitignore` file:
```
# .gitignore
.env
.env.*
!.env.example
```

</details>

#### 💡 Key Takeaways
- Never commit `.env` files to git
- Create `.env.example` as a template
- Use `process.env.VARIABLE_NAME` to access values
- Provide default values for non-critical settings

---

### Problem 2: Environment-Specific Configuration

#### 📝 Problem Statement
Your app needs different settings for:
- **Development** - Local database, debug logs, fake email sending
- **Staging** - Test database, real email but to test addresses
- **Production** - Real database, real email, error tracking

Create configuration files for each environment.

#### 🔍 Detailed Explanation

**Why Different Configurations?**
- **Development** - Fast iteration, detailed errors, fake services
- **Staging** - Test with real services but safe data
- **Production** - Optimized for performance and security

**Common Differences:**
| Setting | Development | Staging | Production |
|---------|-------------|---------|------------|
| Database | Local | Test server | Main server |
| Logging | Debug level | Info level | Error level |
| Email | Console only | Send to test emails | Send real emails |
| Errors | Full stack traces | User-friendly messages | User-friendly messages |

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Environment-specific configuration
// ============================================

// File: config/index.js
const environments = {
    development: {
        // Server settings
        port: 3000,
        host: 'localhost',
        
        // Database settings
        database: {
            host: 'localhost',
            port: 5432,
            name: 'myapp_dev',
            user: 'dev_user',
            password: 'dev_password'
        },
        
        // Email settings (fake - just log to console)
        email: {
            provider: 'console',
            from: 'dev@myapp.com'
        },
        
        // Logging
        logLevel: 'debug',
        
        // Features
        debugMode: true,
        
        // External APIs (use test keys)
        apiKeys: {
            stripe: 'sk_test_...',
            sendgrid: 'SG.test...'
        }
    },
    
    staging: {
        // Server settings
        port: 3000,
        host: '0.0.0.0',
        
        // Database settings (cloud test database)
        database: {
            host: 'test-db.aws.com',
            port: 5432,
            name: 'myapp_staging',
            user: 'staging_user',
            password: process.env.STAGING_DB_PASSWORD  // From environment
        },
        
        // Email settings (real but to test emails only)
        email: {
            provider: 'sendgrid',
            from: 'staging@myapp.com',
            testMode: true  // Only send to whitelisted emails
        },
        
        // Logging
        logLevel: 'info',
        
        // Features
        debugMode: false,
        
        // External APIs (test keys)
        apiKeys: {
            stripe: 'sk_test_...',
            sendgrid: 'SG.test...'
        }
    },
    
    production: {
        // Server settings
        port: process.env.PORT || 8080,
        host: '0.0.0.0',
        
        // Database settings (production cloud database)
        database: {
            host: process.env.DB_HOST,
            port: 5432,
            name: process.env.DB_NAME,
            user: process.env.DB_USER,
            password: process.env.DB_PASSWORD
        },
        
        // Email settings (real)
        email: {
            provider: 'sendgrid',
            from: 'hello@myapp.com',
            testMode: false
        },
        
        // Logging
        logLevel: 'error',
        
        // Features
        debugMode: false,
        
        // External APIs (live keys from environment)
        apiKeys: {
            stripe: process.env.STRIPE_LIVE_KEY,
            sendgrid: process.env.SENDGRID_LIVE_KEY
        }
    }
};

// Determine current environment
const nodeEnv = process.env.NODE_ENV || 'development';

// Load configuration for current environment
const config = environments[nodeEnv];

// Add helper method to check environment
config.isDevelopment = nodeEnv === 'development';
config.isStaging = nodeEnv === 'staging';
config.isProduction = nodeEnv === 'production';

module.exports = config;
```

**Using the configuration:**
```javascript
// server.js
const config = require('./config');

console.log(`Starting in ${config.nodeEnv} mode`);

if (config.isDevelopment) {
    console.log('Debug mode ON - detailed logs enabled');
}

const server = app.listen(config.port, () => {
    console.log(`Server running on http://${config.host}:${config.port}`);
});
```

**Running different environments:**
```bash
# Development (default)
npm start

# Staging
NODE_ENV=staging npm start

# Production
NODE_ENV=production npm start
```

</details>

#### 💡 Key Takeaways
- Use `NODE_ENV` to determine current environment
- Never hardcode production credentials - use environment variables
- Create separate config files or objects for each environment
- Test staging environment before deploying to production

---

### Problem 3: Validating Required Environment Variables

#### 📝 Problem Statement
Your production app requires certain environment variables to work:
- `DATABASE_URL` - Must be a valid URL
- `JWT_SECRET` - Must be at least 32 characters
- `API_KEY` - Required for external service

If any required variable is missing, the app should:
1. Log a clear error message
2. Exit with error code 1 (fail)
3. List ALL missing variables (not just first one)

#### 🔍 Detailed Explanation

**Why Validate?**
- **Fail Fast** - Catch problems before they cause errors
- **Clear Messages** - Developer knows what's missing
- **No Secrets in Logs** - Show variable names, not values

**What to Validate:**
- Required variables exist
- Values have correct format (email, URL, etc.)
- Minimum length for secrets
- Numbers are within range

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Environment Variable Validator
// ============================================

// File: config/validator.js

// Define required variables and their rules
const validationRules = [
    {
        name: 'DATABASE_URL',
        required: true,
        validate: (value) => {
            if (!value.startsWith('postgresql://')) {
                return 'Must start with postgresql://';
            }
            if (!value.includes('@')) {
                return 'Must contain host (use @ symbol)';
            }
            return null;
        }
    },
    {
        name: 'JWT_SECRET',
        required: true,
        minLength: 32,
        validate: (value) => {
            if (value.length < 32) {
                return `Must be at least 32 characters (current: ${value.length})`;
            }
            if (value === 'secret' || value === 'password') {
                return 'Please use a strong, unique secret';
            }
            return null;
        }
    },
    {
        name: 'API_KEY',
        required: true,
        validate: (value) => {
            if (!value.match(/^sk_(live|test)_[a-zA-Z0-9]+$/)) {
                return 'Invalid API key format';
            }
            return null;
        }
    },
    {
        name: 'PORT',
        required: false,
        default: 3000,
        validate: (value) => {
            const port = parseInt(value, 10);
            if (isNaN(port)) {
                return 'Must be a number';
            }
            if (port < 1024 && process.env.NODE_ENV === 'production') {
                return 'Ports below 1024 require root privileges - use port 3000 or higher';
            }
            if (port > 65535) {
                return 'Port must be between 1 and 65535';
            }
            return null;
        }
    },
    {
        name: 'REDIS_URL',
        required: false,  // Optional feature
        validate: (value) => {
            if (value && !value.startsWith('redis://')) {
                return 'Must start with redis://';
            }
            return null;
        }
    }
];

// Validate all environment variables
function validateEnvironment() {
    const errors = [];
    const warnings = [];
    const config = {};

    for (const rule of validationRules) {
        const value = process.env[rule.name];
        
        // Check if required variable exists
        if (rule.required && !value) {
            errors.push(`❌ Missing required variable: ${rule.name}`);
            continue;
        }
        
        // If not required and not provided, use default
        if (!value && rule.default !== undefined) {
            config[rule.name] = rule.default;
            continue;
        }
        
        // Skip validation if no value and not required
        if (!value) {
            continue;
        }
        
        // Run custom validation
        if (rule.validate) {
            const validationError = rule.validate(value);
            if (validationError) {
                errors.push(`❌ ${rule.name}: ${validationError}`);
                continue;
            }
        }
        
        // Check minimum length
        if (rule.minLength && value.length < rule.minLength) {
            errors.push(`❌ ${rule.name}: Must be at least ${rule.minLength} characters (current: ${value.length})`);
            continue;
        }
        
        // Check if value matches allowed values
        if (rule.allowed && !rule.allowed.includes(value)) {
            errors.push(`❌ ${rule.name}: Must be one of: ${rule.allowed.join(', ')}`);
            continue;
        }
        
        // For production, add warnings for potentially weak values
        if (process.env.NODE_ENV === 'production') {
            if (value === 'changeme' || value === 'password123') {
                warnings.push(`⚠️ ${rule.name}: Using weak/default value in production!`);
            }
        }
        
        // Store validated value
        config[rule.name] = value;
    }
    
    // Report results
    if (errors.length > 0) {
        console.error('\n🚫 Environment Validation Failed:\n');
        errors.forEach(error => console.error(`  ${error}`));
        
        if (process.env.NODE_ENV === 'production') {
            console.error('\n💡 Tip: Check your production environment variables');
            console.error('   Run: heroku config (for Heroku) or echo $VARIABLE_NAME\n');
        }
        
        // Exit with error code
        process.exit(1);
    }
    
    if (warnings.length > 0) {
        console.warn('\n⚠️ Environment Warnings:\n');
        warnings.forEach(warning => console.warn(`  ${warning}`));
    }
    
    console.log('\n✅ Environment validation passed\n');
    return config;
}

// Run validation
const validatedConfig = validateEnvironment();

module.exports = validatedConfig;
```

**Usage in main app:**
```javascript
// index.js - First thing your app does
require('dotenv').config();

// Validate environment before anything else
const config = require('./config/validator');

// Only continue if validation passed
const app = require('./app');
app.listen(config.PORT || 3000);
```

</details>

#### 💡 Key Takeaways
- Validate environment variables at startup
- Exit with error if required variables missing
- Show helpful error messages
- Don't log actual secret values, only variable names

---

### Problem 4: Using .env Files with Different Environments

#### 📝 Problem Statement
You need to manage multiple environments:
- Local development (your computer)
- Testing (CI/CD pipeline)
- Staging (test server)
- Production (live server)

Create a system that automatically loads the correct .env file based on `NODE_ENV`.

#### 🔍 Detailed Explanation

**File Structure:**
```
project/
├── .env.development      # Local development
├── .env.test            # Testing environment
├── .env.staging         # Staging server
├── .env.production      # Production server
├── .env.example         # Template for new developers
└── .gitignore          # Excludes .env files (except example)
```

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Multi-Environment .env Loader
// ============================================

// File: config/env-loader.js
const path = require('path');
const fs = require('fs');

// Determine current environment
const NODE_ENV = process.env.NODE_ENV || 'development';

// Define which .env file to load for each environment
const envFiles = {
    development: '.env.development',
    test: '.env.test',
    staging: '.env.staging',
    production: '.env.production'
};

// Get the correct file for this environment
const envFile = envFiles[NODE_ENV];

if (!envFile) {
    console.error(`Unknown environment: ${NODE_ENV}`);
    console.log('Valid environments: development, test, staging, production');
    process.exit(1);
}

// Path to the .env file
const envPath = path.join(process.cwd(), envFile);

// Check if file exists
if (!fs.existsSync(envPath)) {
    console.warn(`⚠️ Warning: ${envFile} not found`);
    console.log(`   Creating from .env.example...`);
    
    // Create from example if available
    const examplePath = path.join(process.cwd(), '.env.example');
    if (fs.existsSync(examplePath)) {
        fs.copyFileSync(examplePath, envPath);
        console.log(`✅ Created ${envFile} from example`);
    } else {
        console.error(`❌ No .env.example found!`);
        process.exit(1);
    }
}

// Load environment variables
require('dotenv').config({ path: envPath });

// Print loaded configuration (without secrets)
console.log(`\n📁 Loading environment: ${NODE_ENV}`);
console.log(`📄 Using config file: ${envFile}\n`);

// Export helper methods
module.exports = {
    isDev: NODE_ENV === 'development',
    isTest: NODE_ENV === 'test',
    isStaging: NODE_ENV === 'staging',
    isProd: NODE_ENV === 'production',
    env: NODE_ENV
};
```

**Example .env files:**

```bash
# .env.development (Local development)
PORT=3000
DATABASE_URL=postgresql://localhost:5432/myapp_dev
DEBUG=true
EMAIL_SEND=false     # Don't send real emails
LOG_LEVEL=debug

# .env.test (Testing)
PORT=3001
DATABASE_URL=postgresql://localhost:5432/myapp_test
DEBUG=true
EMAIL_SEND=false
LOG_LEVEL=silent     # No logs during tests

# .env.staging (Staging server)
PORT=3000
DATABASE_URL=postgresql://staging-db:5432/myapp_staging
DEBUG=false
EMAIL_SEND=true      # Send real emails but to test accounts
LOG_LEVEL=info

# .env.production (Production server)
PORT=8080
DATABASE_URL=postgresql://prod-db:5432/myapp_prod
DEBUG=false
EMAIL_SEND=true
LOG_LEVEL=error
```

**Usage in package.json scripts:**
```json
{
    "scripts": {
        "start": "node index.js",
        "dev": "NODE_ENV=development nodemon index.js",
        "test": "NODE_ENV=test jest",
        "staging": "NODE_ENV=staging node index.js",
        "prod": "NODE_ENV=production node index.js"
    }
}
```

**Usage in code:**
```javascript
// index.js
require('./config/env-loader');
const config = require('./config/config');

if (config.isDev) {
    console.log('🐛 Running in development mode - detailed logging on');
}

if (config.isProd) {
    console.log('🚀 Running in production mode - optimized for performance');
}

const app = require('./app');
const PORT = process.env.PORT || 3000;
app.listen(PORT);
```

</details>

#### 💡 Key Takeaways
- Keep separate .env files for each environment
- Use .env.example as template for new developers
- Never commit actual .env files to git
- Add .env files to .gitignore

---

### Problem 5: Configuration for Different Deployment Platforms

#### 📝 Problem Statement
Your app will be deployed to different platforms:
- **Heroku** - Uses `process.env.PORT` (assigned automatically)
- **Render** - Uses `PORT` environment variable
- **Vercel** - Uses `VERCEL_ENV` for environment detection
- **Local** - Uses your .env file

Create configuration that works on all platforms.

#### 🔍 Detailed Explanation

**Platform Differences:**
| Platform | PORT Variable | Environment Detection | Database URLs |
|----------|---------------|----------------------|---------------|
| Heroku | `process.env.PORT` | `process.env.NODE_ENV` | Addons provide URL |
| Render | `process.env.PORT` | `process.env.RENDER` | Dashboard configuration |
| Vercel | `process.env.PORT` | `process.env.VERCEL_ENV` | Environment variables |
| Local | Your .env file | Your NODE_ENV | Your local database |

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Platform-Agnostic Configuration
// ============================================

// File: config/platform.js

// Detect which platform we're running on
const platform = {
    // Check for Heroku
    isHeroku: !!process.env.HEROKU_APP_NAME || !!process.env.HEROKU_SLUG_ID,
    
    // Check for Render
    isRender: !!process.env.RENDER,
    
    // Check for Vercel
    isVercel: !!process.env.VERCEL,
    
    // Check for AWS Elastic Beanstalk
    isAWS: !!process.env.AWS_EXECUTION_ENV,
    
    // Check for Docker
    isDocker: fs.existsSync('/.dockerenv'),
    
    // Default to local
    isLocal: !process.env.HEROKU && !process.env.RENDER && !process.env.VERCEL
};

// Get port number (handles different platforms)
function getPort() {
    // Heroku and many platforms set PORT env variable
    if (process.env.PORT) {
        return parseInt(process.env.PORT, 10);
    }
    
    // Default for local development
    return 3000;
}

// Get environment name
function getEnvironment() {
    // Vercel uses VERCEL_ENV
    if (process.env.VERCEL_ENV) {
        return process.env.VERCEL_ENV; // 'production', 'preview', 'development'
    }
    
    // Heroku and others use NODE_ENV
    if (process.env.NODE_ENV) {
        return process.env.NODE_ENV;
    }
    
    // Default to development
    return 'development';
}

// Get database URL (handles different platforms)
function getDatabaseUrl() {
    // Check if platform provides database URL
    if (process.env.DATABASE_URL) {
        return process.env.DATABASE_URL;
    }
    
    // Heroku Postgres addon provides DATABASE_URL
    if (process.env.HEROKU_POSTGRESQL_CRIMSON_URL) {
        return process.env.HEROKU_POSTGRESQL_CRIMSON_URL;
    }
    
    // Local fallback
    return 'postgresql://localhost:5432/myapp_dev';
}

// File: config/index.js
const platform = require('./platform');

// Load appropriate .env file
if (platform.isLocal) {
    require('dotenv').config();
}

const config = {
    // Server configuration
    port: getPort(),
    env: getEnvironment(),
    
    // Database
    databaseUrl: getDatabaseUrl(),
    
    // Platform info
    platform: {
        name: platform.isHeroku ? 'Heroku' :
              platform.isRender ? 'Render' :
              platform.isVercel ? 'Vercel' :
              platform.isAWS ? 'AWS' :
              platform.isDocker ? 'Docker' : 'Local',
        
        isProduction: getEnvironment() === 'production'
    },
    
    // Feature flags based on environment
    features: {
        sendEmails: getEnvironment() === 'production',
        detailedLogs: getEnvironment() !== 'production',
        cacheResponses: getEnvironment() === 'production'
    }
};

console.log(`🚀 Running on: ${config.platform.name}`);
console.log(`🌍 Environment: ${config.env}`);
console.log(`🔌 Port: ${config.port}`);

module.exports = config;
```

**Deployment-specific configuration files:**

```javascript
// File: config/heroku.js (Heroku-specific overrides)
module.exports = {
    // Heroku gives you a PORT variable
    port: process.env.PORT,
    
    // Heroku uses dynos (server instances)
    workers: {
        web: 1,   // Web dynos for HTTP
        worker: 1  // Worker dynos for background jobs
    },
    
    // Heroku has 30 second timeout
    timeout: 30000,
    
    // Heroku recommends logging to stdout
    logging: {
        type: 'stdout',
        format: 'line'  // Heroku expects line-based logs
    }
};

// File: config/vercel.js (Vercel-specific)
module.exports = {
    // Vercel uses serverless functions
    isServerless: true,
    
    // Maximum execution time (10 seconds on Hobby plan)
    maxDuration: 10,
    
    // Vercel caches responses
    caching: {
        enabled: true,
        ttl: 60  // 60 seconds
    }
};
```

**Platform-agnostic server code:**
```javascript
// server.js
const config = require('./config');
const express = require('express');

const app = express();

// Health check endpoint (required by many platforms)
app.get('/health', (req, res) => {
    res.json({
        status: 'OK',
        platform: config.platform.name,
        environment: config.env,
        timestamp: new Date().toISOString()
    });
});

// Start server (different for serverless vs traditional)
if (config.platform.isVercel) {
    // Export for Vercel (serverless)
    module.exports = app;
} else {
    // Traditional server
    app.listen(config.port, () => {
        console.log(`Server running on port ${config.port}`);
    });
}
```

</details>

#### 💡 Key Takeaways
- Different platforms have different conventions
- Use platform detection to adapt configuration
- Always check for environment variables first
- Test on each platform before deploying

---

## Production Readiness (Problems 6-10)

---

### Problem 6: Production Logging vs Development Logging

#### 📝 Problem Statement
Your app needs different logging behavior:
- **Development:** Colorful, detailed, human-readable logs
- **Production:** JSON format, machine-readable, less detail

Implement a logging system that works differently based on environment.

#### 🔍 Detailed Explanation

**Why Different Logging?**
- **Development** - You want to see everything, easy to read
- **Production** - Logs go to services like Logtail, need structured data

**Log Levels (from most to least detail):**
1. `debug` - Very detailed (only development)
2. `info` - Normal events (user logged in)
3. `warn` - Something wrong but app continues
4. `error` - Something failed

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Environment-Aware Logger
// ============================================

// File: logger.js
const config = require('./config');

// Simple console logger with colors
class Logger {
    constructor(environment) {
        this.isDev = environment === 'development';
        this.isProd = environment === 'production';
    }
    
    // Format timestamp
    getTimestamp() {
        return new Date().toISOString();
    }
    
    // Development: colorful, human-readable
    formatDev(level, message, meta = {}) {
        const colors = {
            debug: '\x1b[36m',  // Cyan
            info: '\x1b[32m',   // Green
            warn: '\x1b[33m',   // Yellow
            error: '\x1b[31m'   // Red
        };
        
        const reset = '\x1b[0m';
        const color = colors[level] || colors.info;
        
        let output = `${color}[${level.toUpperCase()}]${reset} `;
        output += `${this.getTimestamp()} - `;
        output += message;
        
        if (Object.keys(meta).length > 0) {
            output += `\n  ${JSON.stringify(meta, null, 2)}`;
        }
        
        return output;
    }
    
    // Production: JSON format for log services
    formatProd(level, message, meta = {}) {
        const logEntry = {
            timestamp: this.getTimestamp(),
            level: level,
            message: message,
            environment: config.env,
            ...meta
        };
        
        return JSON.stringify(logEntry);
    }
    
    // Main log method
    log(level, message, meta = {}) {
        if (level === 'debug' && this.isProd) {
            // Don't log debug in production
            return;
        }
        
        const formatted = this.isDev 
            ? this.formatDev(level, message, meta)
            : this.formatProd(level, message, meta);
        
        // Choose console method based on level
        if (level === 'error') {
            console.error(formatted);
        } else if (level === 'warn') {
            console.warn(formatted);
        } else {
            console.log(formatted);
        }
    }
    
    // Convenience methods
    debug(message, meta) { this.log('debug', message, meta); }
    info(message, meta) { this.log('info', message, meta); }
    warn(message, meta) { this.log('warn', message, meta); }
    error(message, meta) { this.log('error', message, meta); }
}

// Create logger instance
const logger = new Logger(config.env);

module.exports = logger;
```

**Usage in your app:**
```javascript
const logger = require('./logger');

// Different log levels
logger.debug('Loading user data...');  // Only shows in development
logger.info('User 123 logged in');
logger.warn('Rate limit approaching (80/100 requests)');
logger.error('Failed to connect to database', { error: err.message });

// With additional metadata
logger.info('Order created', {
    orderId: 'ORD-123',
    userId: 456,
    total: 99.95,
    items: 3
});
```

**Output in Development:**
```
[INFO] 2024-01-15T10:30:00.000Z - User 123 logged in
[WARN] 2024-01-15T10:30:01.000Z - Rate limit approaching (80/100 requests)
[ERROR] 2024-01-15T10:30:02.000Z - Failed to connect to database
  {"error": "Connection refused"}
```

**Output in Production:**
```json
{"timestamp":"2024-01-15T10:30:00.000Z","level":"info","message":"User 123 logged in","environment":"production"}
{"timestamp":"2024-01-15T10:30:01.000Z","level":"warn","message":"Rate limit approaching (80/100 requests)","environment":"production"}
{"timestamp":"2024-01-15T10:30:02.000Z","level":"error","message":"Failed to connect to database","environment":"production","error":"Connection refused"}
```

</details>

#### 💡 Key Takeaways
- Use `debug` level only in development
- Production logs should be JSON for machine parsing
- Include timestamps and environment info in logs
- Never log sensitive data (passwords, tokens, credit cards)

---

### Problem 7: Error Handling in Production

#### 📝 Problem Statement
Your API needs to handle errors differently:
- **Development:** Show full error details (stack traces)
- **Production:** Show user-friendly messages, log details internally

Create error handling middleware that adapts to the environment.

#### 🔍 Detailed Explanation

**Why Hide Errors in Production?**
- **Security** - Stack traces reveal code structure
- **User Experience** - Technical errors confuse users
- **Professional** - Shows polished error messages

**Error Types to Handle:**
- Validation errors (user input wrong)
- Not found (404)
- Authentication errors (401)
- Authorization errors (403)
- Server errors (500)

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Environment-Aware Error Handler
// ============================================

// File: middleware/errorHandler.js
const logger = require('../logger');

// Custom error classes
class AppError extends Error {
    constructor(message, statusCode, isOperational = true) {
        super(message);
        this.statusCode = statusCode;
        this.isOperational = isOperational;
        this.status = `${statusCode}`.startsWith('4') ? 'fail' : 'error';
        Error.captureStackTrace(this, this.constructor);
    }
}

class ValidationError extends AppError {
    constructor(message, errors) {
        super(message, 400);
        this.errors = errors;
    }
}

class NotFoundError extends AppError {
    constructor(resource = 'Resource') {
        super(`${resource} not found`, 404);
    }
}

class UnauthorizedError extends AppError {
    constructor(message = 'Unauthorized') {
        super(message, 401);
    }
}

class ForbiddenError extends AppError {
    constructor(message = 'Forbidden') {
        super(message, 403);
    }
}

// Error handler middleware
function errorHandler(err, req, res, next) {
    const isDevelopment = process.env.NODE_ENV === 'development';
    const isProduction = process.env.NODE_ENV === 'production';
    
    // Default values
    let statusCode = err.statusCode || 500;
    let message = err.message || 'Internal Server Error';
    let response = {};
    
    // Log the error
    if (statusCode >= 500) {
        logger.error(`[${req.method}] ${req.url} - ${err.message}`, {
            stack: err.stack,
            body: req.body,
            params: req.params,
            query: req.query,
            userId: req.user?.id
        });
    } else {
        logger.warn(`[${req.method}] ${req.url} - ${err.message}`);
    }
    
    // Handle different error types
    if (err.name === 'ValidationError') {
        statusCode = 400;
        response = {
            status: 'fail',
            message: 'Validation failed',
            errors: err.errors
        };
    }
    else if (err.code === '23505') { // PostgreSQL duplicate key
        statusCode = 409;
        message = 'Duplicate entry exists';
    }
    else if (err.code === '23503') { // PostgreSQL foreign key violation
        statusCode = 400;
        message = 'Related record not found';
    }
    else if (err.name === 'JsonWebTokenError') {
        statusCode = 401;
        message = 'Invalid token';
    }
    else if (err.name === 'TokenExpiredError') {
        statusCode = 401;
        message = 'Token expired';
    }
    else {
        // Use the error's own status and message
        statusCode = err.statusCode || 500;
        message = err.message || 'Something went wrong';
        
        response = {
            status: err.status || 'error',
            message: message
        };
    }
    
    // Add development-only details
    if (isDevelopment) {
        response.stack = err.stack;
        response.details = err.details;
    }
    
    // Add safe production error (don't leak details)
    if (isProduction && statusCode === 500) {
        response.message = 'Internal server error';
    }
    
    res.status(statusCode).json(response);
}

// 404 handler for unmatched routes
function notFoundHandler(req, res, next) {
    const error = new NotFoundError(`Cannot ${req.method} ${req.url}`);
    next(error);
}

// Async wrapper to catch errors in async routes
function catchAsync(fn) {
    return (req, res, next) => {
        fn(req, res, next).catch(next);
    };
}

module.exports = {
    AppError,
    ValidationError,
    NotFoundError,
    UnauthorizedError,
    ForbiddenError,
    errorHandler,
    notFoundHandler,
    catchAsync
};
```

**Using in your routes:**
```javascript
const express = require('express');
const { catchAsync, ValidationError, NotFoundError } = require('./middleware/errorHandler');

const router = express.Router();

// Using catchAsync to handle async errors
router.get('/users/:id', catchAsync(async (req, res) => {
    const user = await db.findUser(req.params.id);
    
    if (!user) {
        throw new NotFoundError('User');
    }
    
    res.json(user);
}));

// Validation example
router.post('/users', catchAsync(async (req, res) => {
    const { email, password } = req.body;
    
    if (!email) {
        throw new ValidationError('Validation failed', {
            email: 'Email is required'
        });
    }
    
    if (password.length < 8) {
        throw new ValidationError('Validation failed', {
            password: 'Password must be at least 8 characters'
        });
    }
    
    const user = await db.createUser(req.body);
    res.status(201).json(user);
}));

// Apply error handlers at the end
app.use(notFoundHandler);
app.use(errorHandler);
```

</details>

#### 💡 Key Takeaways
- Never show stack traces in production
- Use custom error classes for different error types
- Log all errors (with details) to your logging service
- Always handle 404 (not found) errors
- Use `catchAsync` wrapper for async routes

---

### Problem 8: Performance Optimization for Production

#### 📝 Problem Statement
Your production app needs optimizations:
- Compress responses (gzip)
- Enable caching
- Remove console.log statements
- Set security headers
- Use production database connection pool

Implement production-only optimizations.

#### 🔍 Detailed Explanation

**Production Optimizations:**
| Optimization | Benefit | Implementation |
|--------------|---------|----------------|
| Gzip compression | Smaller responses, faster loading | `compression` middleware |
| Caching headers | Fewer requests to server | `helmet` + cache headers |
| Security headers | Protect against attacks | `helmet` middleware |
| Connection pooling | Reuse database connections | Configure pool size |
| Remove logs | Better performance | Disable debug logs |

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Production Optimizations
// ============================================

// File: app.js
const express = require('express');
const compression = require('compression');
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');

const app = express();
const isProduction = process.env.NODE_ENV === 'production';

// ============================================
// 1. Security Headers (helmet)
// ============================================
// Helmet sets various security headers
app.use(helmet());

// In development, you might want to relax CSP
if (!isProduction) {
    app.use(helmet({
        contentSecurityPolicy: false // Disable CSP in dev
    }));
}

// ============================================
// 2. Compression (gzip)
// ============================================
// Compress responses - only needed in production
if (isProduction) {
    app.use(compression({
        level: 6,  // Compression level (1-9, 6 is good balance)
        threshold: 1024,  // Only compress responses > 1KB
        filter: (req, res) => {
            // Don't compress small responses
            if (req.headers['x-no-compression']) {
                return false;
            }
            // Use default compression filter
            return compression.filter(req, res);
        }
    }));
}

// ============================================
// 3. Rate Limiting
// ============================================
const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: isProduction ? 100 : 10000, // Stricter in production
    message: 'Too many requests from this IP, please try again later.',
    standardHeaders: true,
    legacyHeaders: false,
});

app.use('/api/', limiter);

// Stricter limit for auth endpoints
const authLimiter = rateLimit({
    windowMs: 15 * 60 * 1000,
    max: isProduction ? 5 : 100, // Only 5 login attempts per 15 min in production
    skipSuccessfulRequests: true,
});

app.use('/api/auth/', authLimiter);

// ============================================
// 4. Remove console.log in production
// ============================================
if (isProduction) {
    // Override console.log to do nothing
    console.log = function() {};
    console.info = function() {};
    
    // Keep console.error and console.warn
    // They're important for debugging issues
}

// Or use a logger that respects environment
const logger = {
    log: (...args) => {
        if (!isProduction) console.log(...args);
    },
    error: (...args) => console.error(...args),
    warn: (...args) => console.warn(...args),
    info: (...args) => {
        if (!isProduction) console.info(...args);
    }
};

// ============================================
// 5. Database Connection Pool
// ============================================
// config/database.js
const { Pool } = require('pg');

const poolConfig = {
    connectionString: process.env.DATABASE_URL,
};

if (isProduction) {
    // Production pool settings
    poolConfig.max = 20,        // Maximum number of clients
    poolConfig.idleTimeoutMillis = 30000,  // Close idle clients after 30s
    poolConfig.connectionTimeoutMillis = 2000,  // Fail fast if can't connect
} else {
    // Development pool settings (lighter)
    poolConfig.max = 5,
    poolConfig.idleTimeoutMillis = 10000,
}

const pool = new Pool(poolConfig);

// ============================================
// 6. Cache Headers
// ============================================
// Cache static assets aggressively
app.use('/static', express.static('public', {
    maxAge: isProduction ? '30d' : '0',
    etag: true,
    lastModified: true
}));

// Cache API responses
function cacheControl(duration) {
    return (req, res, next) => {
        if (isProduction) {
            res.set('Cache-Control', `public, max-age=${duration}`);
        } else {
            res.set('Cache-Control', 'no-cache');
        }
        next();
    };
}

app.get('/api/products', cacheControl(300), async (req, res) => {
    // Products data (cached for 5 minutes)
    const products = await db.getProducts();
    res.json(products);
});

// ============================================
// 7. Trust Proxy (for Heroku, Render, etc.)
// ============================================
if (isProduction) {
    // Trust first proxy (important for HTTPS detection)
    app.set('trust proxy', 1);
}

// ============================================
// 8. Production-Specific Middleware
// ============================================
// Don't log detailed request info in production
app.use((req, res, next) => {
    if (!isProduction) {
        console.log(`${req.method} ${req.url}`);
    }
    next();
});

// Health check endpoint (for load balancers)
app.get('/health', (req, res) => {
    res.status(200).json({
        status: 'healthy',
        timestamp: new Date().toISOString(),
        uptime: process.uptime()
    });
});

// ============================================
// 9. Graceful Shutdown
// ============================================
function gracefulShutdown() {
    console.log('Received shutdown signal, closing connections...');
    
    // Close database pool
    pool.end(() => {
        console.log('Database connections closed');
    });
    
    // Close server
    server.close(() => {
        console.log('HTTP server closed');
        process.exit(0);
    });
    
    // Force close after 10 seconds
    setTimeout(() => {
        console.error('Could not close connections in time, forcing shutdown');
        process.exit(1);
    }, 10000);
}

if (isProduction) {
    process.on('SIGTERM', gracefulShutdown);
    process.on('SIGINT', gracefulShutdown);
}

module.exports = app;
```

</details>

#### 💡 Key Takeaways
- Enable gzip compression for JSON responses
- Set proper cache headers for static files
- Use rate limiting to prevent abuse
- Remove `console.log` in production
- Configure connection pool sizes appropriately
- Always add security headers with Helmet

---

### Problem 9: Health Checks and Monitoring

#### 📝 Problem Statement
Your deployed app needs health check endpoints that:
- Tell if the app is running (simple ping)
- Check if database is connected
- Report memory usage
- Provide uptime information
- Return different statuses based on severity

#### 🔍 Detailed Explanation

**Why Health Checks?**
- Load balancers use them to know if your app is alive
- Monitoring services alert you when something is wrong
- Kubernetes uses them to restart unhealthy containers

**Health Check Types:**
| Type | Endpoint | Purpose |
|------|----------|---------|
| Liveness | `/health/live` | Is the app running? |
| Readiness | `/health/ready` | Is the app ready to receive traffic? |
| Startup | `/health/startup` | Is the app still starting up? |
| Deep check | `/health/deep` | Check all dependencies |

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Health Check Endpoints
// ============================================

// File: routes/health.js
const express = require('express');
const router = express.Router();

// Get memory usage
function getMemoryUsage() {
    const used = process.memoryUsage();
    return {
        rss: Math.round(used.rss / 1024 / 1024), // MB
        heapTotal: Math.round(used.heapTotal / 1024 / 1024),
        heapUsed: Math.round(used.heapUsed / 1024 / 1024),
        external: Math.round(used.external / 1024 / 1024)
    };
}

// Simple ping endpoint (liveness)
router.get('/live', (req, res) => {
    res.status(200).json({
        status: 'alive',
        timestamp: new Date().toISOString()
    });
});

// Readiness check (is app ready for traffic)
router.get('/ready', async (req, res) => {
    const checks = {
        app: true,
        database: false,
        redis: false
    };
    
    // Check database connection
    try {
        const db = require('../config/database');
        await db.query('SELECT 1');
        checks.database = true;
    } catch (error) {
        console.error('Database health check failed:', error.message);
    }
    
    // Check Redis (if used)
    try {
        const redis = require('../config/redis');
        await redis.ping();
        checks.redis = true;
    } catch (error) {
        // Redis might be optional, don't fail if not configured
        if (process.env.REDIS_URL) {
            console.error('Redis health check failed:', error.message);
        } else {
            checks.redis = 'not configured';
        }
    }
    
    // Determine overall status
    const allRequired = checks.database; // Database is required
    const statusCode = allRequired ? 200 : 503; // 503 = Service Unavailable
    
    res.status(statusCode).json({
        status: allRequired ? 'ready' : 'not_ready',
        timestamp: new Date().toISOString(),
        checks: checks
    });
});

// Detailed health check (for monitoring)
router.get('/deep', async (req, res) => {
    const startTime = Date.now();
    
    const health = {
        status: 'healthy',
        timestamp: new Date().toISOString(),
        uptime: process.uptime(),
        memory: getMemoryUsage(),
        version: process.env.npm_package_version || '1.0.0',
        environment: process.env.NODE_ENV,
        checks: {
            database: { status: 'unknown', latency: null },
            redis: { status: 'unknown', latency: null },
            disk: { status: 'unknown' }
        }
    };
    
    // Check database with timing
    const dbStart = Date.now();
    try {
        const db = require('../config/database');
        await db.query('SELECT 1');
        health.checks.database.status = 'healthy';
        health.checks.database.latency = Date.now() - dbStart;
    } catch (error) {
        health.checks.database.status = 'unhealthy';
        health.checks.database.error = error.message;
        health.status = 'unhealthy';
    }
    
    // Check Redis
    const redisStart = Date.now();
    try {
        const redis = require('../config/redis');
        await redis.ping();
        health.checks.redis.status = 'healthy';
        health.checks.redis.latency = Date.now() - redisStart;
    } catch (error) {
        if (process.env.REDIS_URL) {
            health.checks.redis.status = 'unhealthy';
            health.checks.redis.error = error.message;
            health.status = 'degraded';
        } else {
            health.checks.redis.status = 'not_configured';
        }
    }
    
    // Response time
    health.responseTime = Date.now() - startTime;
    
    // Warn if response time is high
    if (health.responseTime > 1000) {
        health.status = 'slow';
    }
    
    // Return 200 even if degraded (but not unhealthy)
    const statusCode = health.status === 'unhealthy' ? 503 : 200;
    res.status(statusCode).json(health);
});

// Metrics endpoint (for Prometheus)
router.get('/metrics', (req, res) => {
    const metrics = {
        uptime_seconds: process.uptime(),
        memory_usage_bytes: process.memoryUsage().heapUsed,
        active_connections: 0, // You'd track this
        requests_total: 0, // You'd track this
    };
    
    res.set('Content-Type', 'text/plain');
    
    let output = '';
    for (const [key, value] of Object.entries(metrics)) {
        output += `${key} ${value}\n`;
    }
    
    res.send(output);
});

// Start up check (for slow-starting apps)
let isReady = false;
let startupTime = Date.now();

// Simulate startup tasks
setTimeout(() => {
    isReady = true;
    console.log('App is now ready to receive traffic');
}, 5000); // 5 second startup time

router.get('/startup', (req, res) => {
    const elapsed = (Date.now() - startupTime) / 1000;
    
    if (!isReady) {
        return res.status(503).json({
            status: 'starting',
            message: `Starting up (${Math.round(elapsed)}s elapsed)`,
            estimatedTime: 5
        });
    }
    
    res.status(200).json({
        status: 'started',
        startupTime: `${elapsed}s`
    });
});

module.exports = router;
```

**Using health checks in your main app:**
```javascript
// server.js
const express = require('express');
const healthRoutes = require('./routes/health');

const app = express();

// Health check endpoints (no authentication needed)
app.use('/health', healthRoutes);

// Your other routes go here
app.use('/api', apiRoutes);

// Start server
const server = app.listen(process.env.PORT || 3000, () => {
    console.log('Server started');
});

// Track active connections for metrics
let activeConnections = 0;

server.on('connection', (socket) => {
    activeConnections++;
    socket.on('close', () => activeConnections--);
});
```

**Testing health checks:**
```bash
# Simple ping
curl http://localhost:3000/health/live

# Readiness check
curl http://localhost:3000/health/ready

# Detailed check
curl http://localhost:3000/health/deep

# Metrics
curl http://localhost:3000/health/metrics
```

</details>

#### 💡 Key Takeaways
- Always implement `/health/live` for basic checks
- Implement `/health/ready` for dependency checks
- Use different endpoints for different purposes
- Include timestamps and version info
- Return appropriate HTTP status codes (200 = OK, 503 = Not ready)

---

### Problem 10: Environment-Specific Dependencies

#### 📝 Problem Statement
Your app uses different packages in development vs production:
- **Development only:** nodemon, jest, supertest
- **Production only:** No dev dependencies
- **All environments:** express, dotenv, pg

Install and manage dependencies correctly for each environment.

#### 🔍 Detailed Explanation

**Dependency Types:**
| Type | Install Command | Used In | Examples |
|------|----------------|---------|----------|
| Regular | `npm install` | Production + Dev | express, pg |
| Dev | `npm install --save-dev` | Development only | nodemon, jest |
| Optional | `npm install --no-save` | Feature flags | |

**Why Separate?**
- Smaller production deployments (faster installs)
- Security (dev tools not exposed)
- Cost (less disk space, bandwidth)

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Managing Different Dependencies
// ============================================

// package.json
{
    "name": "myapp",
    "version": "1.0.0",
    "scripts": {
        "start": "node server.js",
        "dev": "nodemon server.js",
        "test": "jest",
        "build": "npm ci --only=production",
        "postinstall": "echo 'Installation complete'"
    },
    "dependencies": {
        // These go to production
        "express": "^4.18.0",
        "dotenv": "^16.0.0",
        "pg": "^8.10.0",
        "bcrypt": "^5.1.0",
        "jsonwebtoken": "^9.0.0"
    },
    "devDependencies": {
        // These are development only
        "nodemon": "^2.0.0",
        "jest": "^29.0.0",
        "supertest": "^6.0.0",
        "eslint": "^8.0.0"
    }
}

// ============================================
// Detecting environment in code
// ============================================
// config/index.js
const isProduction = process.env.NODE_ENV === 'production';
const isDevelopment = process.env.NODE_ENV === 'development';
const isTest = process.env.NODE_ENV === 'test';

// Conditionally require dev-only modules
let devTools = null;
if (isDevelopment) {
    // Only load in development
    devTools = {
        morgan: require('morgan'),
        cors: require('cors')
    };
}

// ============================================
// Different middleware based on environment
// ============================================
// server.js
const express = require('express');
const app = express();

// Development-only middleware
if (process.env.NODE_ENV === 'development') {
    const morgan = require('morgan');
    app.use(morgan('dev')); // Detailed request logging
    
    const cors = require('cors');
    app.use(cors()); // Allow any origin in dev
}

// Production-only middleware
if (process.env.NODE_ENV === 'production') {
    const compression = require('compression');
    app.use(compression()); // Compress responses
    
    const helmet = require('helmet');
    app.use(helmet()); // Security headers
}

// ============================================
// Environment-specific database seeding
// ============================================
// scripts/seed.js
async function seedDatabase() {
    const isDevelopment = process.env.NODE_ENV === 'development';
    
    if (isDevelopment) {
        console.log('🌱 Seeding development database with sample data...');
        // Add 100 sample users, products, etc.
        await addSampleData();
    } else if (process.env.NODE_ENV === 'test') {
        console.log('🧪 Seeding test database...');
        await addMinimalTestData();
    } else {
        console.log('⚠️ Not in development - skipping seed');
    }
}

// ============================================
// Optimized production install
// ============================================
// .dockerignore
node_modules
npm-debug.log
.env
.git
.gitignore
README.md
test/
tests/
__tests__/
coverage/
.nyc_output/

// Dockerfile (production optimized)
FROM node:18-alpine

WORKDIR /app

# Copy package files only
COPY package*.json ./

# Install ONLY production dependencies
RUN npm ci --only=production

# Copy application code
COPY . .

# Remove development files
RUN rm -rf tests/ __tests__/ .git/

EXPOSE 3000
CMD ["node", "server.js"]

// ============================================
// npm scripts for different environments
// ============================================
// package.json scripts
{
    "scripts": {
        // Development
        "dev": "NODE_ENV=development nodemon server.js",
        "dev:debug": "NODE_ENV=development nodemon --inspect server.js",
        
        // Testing
        "test": "NODE_ENV=test jest",
        "test:watch": "NODE_ENV=test jest --watch",
        "test:coverage": "NODE_ENV=test jest --coverage",
        
        // Production
        "start": "NODE_ENV=production node server.js",
        "start:cluster": "NODE_ENV=production node cluster.js",
        
        // Utilities
        "clean": "rm -rf node_modules package-lock.json",
        "rebuild": "npm run clean && npm install",
        "build": "npm ci --only=production"
    }
}

// ============================================
// Environment-specific configuration loading
// ============================================
// config/index.js
const config = {
    development: {
        database: {
            logging: true,
            synchronize: true  // Auto-create tables
        },
        debug: true,
        emailService: 'console'
    },
    
    test: {
        database: {
            logging: false,
            synchronize: true,
            database: 'myapp_test'
        },
        debug: false,
        emailService: 'memory'
    },
    
    production: {
        database: {
            logging: false,
            synchronize: false  // Use migrations in production
        },
        debug: false,
        emailService: 'sendgrid'
    }
};

const env = process.env.NODE_ENV || 'development';
module.exports = config[env];
```

**Installation commands for different scenarios:**

```bash
# Development (install everything)
npm install

# Production only (no dev dependencies)
npm install --only=production

# Production with CI (faster, uses package-lock.json)
npm ci --only=production

# Install a new production dependency
npm install express --save

# Install a new development dependency
npm install nodemon --save-dev

# Check what will be installed in production
npm list --only=production --depth=0

# Find which packages are unused (production)
npm prune --production --dry-run
```

</details>

#### 💡 Key Takeaways
- Use `--save-dev` for development-only packages
- Use `--only=production` when deploying
- Never run `npm install` in production (use `npm ci`)
- Keep production dependencies minimal
- Test with `NODE_ENV=production` locally before deploying

---

## Hosting Platforms (Problems 11-15)

---

### Problem 11: Deploying to Render.com

#### 📝 Problem Statement
Deploy a Node.js Express app to Render.com (free tier) with:
- Automatic deploys from GitHub
- Environment variables configuration
- Database connection (Render PostgreSQL)
- Custom domain (optional)

Provide step-by-step deployment instructions.

#### 🔍 Detailed Explanation

**Why Render?**
- Free tier available
- Automatic HTTPS
- Automatic deploys from GitHub
- Built-in PostgreSQL
- Easy to use

<details>
<summary>🔽 Click to View Solution</summary>

```yaml
# ============================================
# Solution: Deploy to Render.com
# ============================================

# Step-by-step guide:

# 1. Prepare your app for Render
# File: render.yaml (Blueprint - optional but recommended)
services:
  - type: web
    name: myapp-api
    runtime: node
    plan: free
    buildCommand: npm install
    startCommand: npm start
    envVars:
      - key: NODE_ENV
        value: production
      - key: DATABASE_URL
        fromDatabase:
          name: myapp-db
          property: connectionString
      - key: JWT_SECRET
        generateValue: true  # Render generates a random secret
      - key: API_KEY
        sync: false  # Manual entry required

databases:
  - name: myapp-db
    plan: free
```

```javascript
// 2. Update your server.js for Render
// server.js
const express = require('express');
const app = express();
const path = require('path');

// Get port from environment (Render sets this)
const PORT = process.env.PORT || 3000;

// Health check endpoint (required for Render)
app.get('/health', (req, res) => {
    res.status(200).json({ status: 'healthy' });
});

// Your API routes
app.get('/api', (req, res) => {
    res.json({ message: 'Hello from Render!' });
});

// For static files (if any)
app.use(express.static(path.join(__dirname, 'public')));

// Start server
app.listen(PORT, '0.0.0.0', () => {
    console.log(`Server running on port ${PORT}`);
});
```

```json
// 3. package.json scripts
{
    "scripts": {
        "start": "node server.js",
        "dev": "nodemon server.js"
    }
}
```

```javascript
// 4. Database configuration for Render PostgreSQL
// config/database.js
const { Pool } = require('pg');

// Render provides DATABASE_URL automatically
const pool = new Pool({
    connectionString: process.env.DATABASE_URL,
    ssl: {
        rejectUnauthorized: false  // Required for Render
    }
});

module.exports = pool;
```

**Deployment Steps:**

1. **Push code to GitHub**
```bash
git init
git add .
git commit -m "Ready for Render deployment"
git remote add origin https://github.com/yourusername/myapp.git
git push -u origin main
```

2. **Create account on Render.com**
   - Sign up with GitHub

3. **Create Web Service**
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Select the repository
   - Choose "Node.js" as environment
   - Build Command: `npm install`
   - Start Command: `npm start`

4. **Add Environment Variables**
   - Go to your service → Environment
   - Add:
     ```
     NODE_ENV=production
     JWT_SECRET=your-strong-secret-here
     ```

5. **Add Database (Optional)**
   - Click "New +" → "PostgreSQL"
   - Choose free plan
   - Copy the Internal Database URL
   - Add to your web service environment as `DATABASE_URL`

6. **Deploy**
   - Click "Manual Deploy" → "Deploy latest commit"
   - Wait for build to complete

7. **Verify Deployment**
   - Your app will be at: `https://myapp.onrender.com`
   - Test health check: `https://myapp.onrender.com/health`

**Automatic Deploys:**
Every time you push to GitHub, Render automatically redeploys!

**Troubleshooting:**
```bash
# Check logs in Render dashboard
# Go to your service → Logs

# Common issues:
# 1. Port binding - Must use process.env.PORT
# 2. SSL for database - Add ssl.rejectUnauthorized = false
# 3. Build fails - Check package.json scripts
```

</details>

#### 💡 Key Takeaways
- Render provides free hosting with automatic SSL
- Always use `process.env.PORT` for port binding
- Add health check endpoint for monitoring
- Render PostgreSQL requires SSL configuration
- Automatic deploys from GitHub saves time

---

### Problem 12: Deploying to Heroku

#### 📝 Problem Statement
Deploy a Node.js app to Heroku using:
- Heroku CLI
- Git deployment
- Environment variables
- Add-ons (database)

#### 🔍 Detailed Explanation

**Why Heroku?**
- One of the oldest, most reliable platforms
- Great CLI tools
- Extensive add-on marketplace
- Free tier available (with limitations)

**Heroku vs Render:**
| Feature | Heroku | Render |
|---------|--------|--------|
| Free tier | Yes (sleeps after 30 min) | Yes (always on) |
| CLI | Excellent | Basic |
| Database | Separate free tier | Free 256MB |
| Custom domain | Yes | Yes |

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Deploy to Heroku
// ============================================

// 1. Prepare your app for Heroku
// server.js
const express = require('express');
const app = express();

// Heroku sets PORT automatically
const PORT = process.env.PORT || 3000;

// Heroku requires binding to all interfaces
app.listen(PORT, '0.0.0.0', () => {
    console.log(`Server running on port ${PORT}`);
});

// Add Procfile (no file extension)
// File: Procfile (in root directory)
web: node server.js
```

```json
// 2. package.json - specify Node version
{
    "name": "myapp",
    "version": "1.0.0",
    "engines": {
        "node": "18.x",
        "npm": "9.x"
    },
    "scripts": {
        "start": "node server.js",
        "heroku-postbuild": "npm run build"  // Optional
    }
}
```

```javascript
// 3. Database configuration for Heroku Postgres
// config/database.js
const { Pool } = require('pg');

// Heroku provides DATABASE_URL
const pool = new Pool({
    connectionString: process.env.DATABASE_URL,
    ssl: {
        rejectUnauthorized: false  // Required for Heroku
    }
});

module.exports = pool;
```

**Deployment Steps:**

1. **Install Heroku CLI**
```bash
# macOS
brew tap heroku/brew && brew install heroku

# Windows
# Download from https://devcenter.heroku.com/articles/heroku-cli

# Login
heroku login
```

2. **Create Heroku App**
```bash
# Create new app
heroku create myapp-name

# Or use existing app name
heroku create myapp-unique-name
```

3. **Deploy with Git**
```bash
# Add Heroku remote
git remote add heroku https://git.heroku.com/myapp-name.git

# Deploy
git push heroku main
```

4. **Add Database**
```bash
# Add PostgreSQL (free tier)
heroku addons:create heroku-postgresql:hobby-dev

# Get database info
heroku pg:info

# Run migrations
heroku run npm run migrate
```

5. **Set Environment Variables**
```bash
# Set variables
heroku config:set NODE_ENV=production
heroku config:set JWT_SECRET=your-secret-key
heroku config:set API_KEY=abc123

# View all config vars
heroku config

# Get database URL
heroku config:get DATABASE_URL
```

6. **Scale Dynos**
```bash
# Scale to 1 web dyno (free tier)
heroku ps:scale web=1

# Check running dynos
heroku ps
```

7. **Open App**
```bash
# Open in browser
heroku open

# View logs
heroku logs --tail

# Run one-off commands
heroku run node scripts/seed.js
```

**Heroku-specific configurations:**

```javascript
// 4. Detect Heroku environment
// config/index.js
const isHeroku = !!process.env.HEROKU_APP_NAME;

if (isHeroku) {
    console.log('Running on Heroku');
    // Heroku-specific settings
    config.trustProxy = true;  // Heroku uses proxy
}

// 5. Graceful shutdown for Heroku
process.on('SIGTERM', () => {
    console.log('SIGTERM received, closing server...');
    server.close(() => {
        console.log('Server closed');
        process.exit(0);
    });
});
```

**Heroku Free Tier Limitations:**
- App sleeps after 30 minutes of inactivity
- 550-1000 free dyno hours per month
- 10,000 row limit for database
- No custom domain (with free SSL)

**Troubleshooting Heroku Deploy:**

```bash
# Check build logs
heroku builds -a myapp-name

# View runtime logs
heroku logs --tail -a myapp-name

# SSH into dyno
heroku run bash -a myapp-name

# Check if app is running
heroku ps -a myapp-name

# Restart app
heroku restart -a myapp-name

# Rollback to previous version
heroku releases -a myapp-name
heroku rollback v3 -a myapp-name
```

</details>

#### 💡 Key Takeaways
- Heroku requires a `Procfile` to start your app
- Use `heroku config:set` for environment variables
- Database URL is automatically provided
- App sleeps on free tier (cold starts)
- Use `heroku logs --tail` for debugging

---

### Problem 13: Deploying to Vercel (Serverless)

#### 📝 Problem Statement
Deploy a Node.js API to Vercel as serverless functions:
- Convert Express app to serverless
- Configure `vercel.json`
- Set up environment variables
- Handle routing

#### 🔍 Detailed Explanation

**What is Serverless?**
- No server to manage
- Pay per request (not for idle time)
- Auto-scaling
- Cold starts (first request may be slow)

**Vercel vs Traditional Hosting:**
| Aspect | Traditional | Vercel Serverless |
|--------|-------------|-------------------|
| Server management | Yes | No |
| Scaling | Manual | Automatic |
| Cold starts | No | Yes (first request) |
| Response time | Consistent | Variable |

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Deploy to Vercel (Serverless)
// ============================================

// 1. Convert Express app for serverless
// api/index.js (Vercel expects this path)
const express = require('express');
const serverless = require('serverless-http');

const app = express();

// Your routes
app.get('/api/hello', (req, res) => {
    res.json({ message: 'Hello from Vercel!' });
});

app.get('/api/users/:id', (req, res) => {
    res.json({ userId: req.params.id });
});

// Health check
app.get('/api/health', (req, res) => {
    res.json({ status: 'ok', timestamp: new Date().toISOString() });
});

// Export for serverless
module.exports.handler = serverless(app);
```

```json
// 2. vercel.json configuration
{
    "version": 2,
    "builds": [
        {
            "src": "api/index.js",
            "use": "@vercel/node"
        }
    ],
    "routes": [
        {
            "src": "/api/(.*)",
            "dest": "/api/index.js"
        },
        {
            "src": "/health",
            "dest": "/api/index.js"
        }
    ],
    "env": {
        "NODE_ENV": "production"
    }
}
```

```json
// 3. package.json
{
    "name": "my-vercel-api",
    "version": "1.0.0",
    "scripts": {
        "vercel-build": "npm install",
        "dev": "vercel dev"
    },
    "dependencies": {
        "express": "^4.18.0",
        "serverless-http": "^3.0.0"
    }
}
```

```javascript
// 4. Handling database in serverless
// config/database.js
const { Pool } = require('pg');

// In serverless, connection pool needs careful management
let pool = null;

function getDatabase() {
    if (!pool) {
        pool = new Pool({
            connectionString: process.env.DATABASE_URL,
            ssl: { rejectUnauthorized: false },
            max: 1,  // Only one connection per function instance
            idleTimeoutMillis: 0  // Don't keep connections idle
        });
    }
    return pool;
}

module.exports = { getDatabase };
```

```javascript
// 5. Example route with database
// api/users.js
const { getDatabase } = require('../config/database');

module.exports = async (req, res) => {
    const db = getDatabase();
    
    try {
        const result = await db.query('SELECT * FROM users LIMIT 10');
        res.json(result.rows);
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: 'Database error' });
    }
};
```

**Deployment Steps:**

1. **Install Vercel CLI**
```bash
npm install -g vercel
```

2. **Login to Vercel**
```bash
vercel login
```

3. **Deploy**
```bash
# Deploy to production
vercel --prod

# Deploy to preview (staging)
vercel

# With environment variables
vercel --env NODE_ENV=production
```

4. **Configure Project**
```bash
# Link existing project
vercel link

# List deployments
vercel list

# Set environment variables (in dashboard)
# Project Settings → Environment Variables
```

5. **Environment Variables in Vercel Dashboard:**
   - Go to your project
   - Settings → Environment Variables
   - Add:
     - `DATABASE_URL`
     - `JWT_SECRET`
     - `API_KEY`

**Serverless-Specific Considerations:**

```javascript
// 6. Handling cold starts (first request is slow)
// Warm-up function (optional)
module.exports = async (req, res) => {
    // Cache database connection
    if (!global.dbConnection) {
        console.log('Cold start - connecting to database...');
        global.dbConnection = await connectToDatabase();
    }
    
    // Your API logic
    res.json({ data: 'response' });
};

// 7. Response time limit (Vercel Hobby: 10 seconds)
app.get('/api/report', async (req, res) => {
    // For long-running tasks, use background jobs
    // Don't let the request wait
    startBackgroundJob();
    res.json({ message: 'Processing started' });
});

// 8. File size limits (Vercel: 4.5MB for serverless)
// Don't include large files in your deployment
```

**Vercel-specific features:**

```json
// 9. vercel.json with advanced config
{
    "functions": {
        "api/*.js": {
            "maxDuration": 10,  // Max seconds (Hobby: 10)
            "memory": 1024       // Memory in MB
        }
    },
    "headers": [
        {
            "source": "/api/(.*)",
            "headers": [
                { "key": "Access-Control-Allow-Origin", "value": "*" },
                { "key": "Cache-Control", "value": "max-age=0" }
            ]
        }
    ]
}
```

**Testing locally:**
```bash
# Install Vercel CLI
npm i -g vercel

# Run locally (emulates serverless)
vercel dev

# Visit http://localhost:3000/api/hello
```

**Limitations to Know:**
- Function timeout: 10 seconds (Hobby) / 60 seconds (Pro)
- Response size: 4.5MB
- Cold starts: 50-500ms penalty
- WebSocket support limited

</details>

#### 💡 Key Takeaways
- Vercel is great for APIs with low traffic
- Cold starts mean first request is slow
- Keep functions small and fast
- Use environment variables for secrets
- Test locally with `vercel dev`

---

### Problem 14: Deploying with PM2 (Process Manager)

#### 📝 Problem Statement
Deploy a Node.js app on a VPS (DigitalOcean, AWS EC2) using PM2 to:
- Keep the app running after logout
- Auto-restart on crash
- Manage multiple instances (clustering)
- View logs and metrics

#### 🔍 Detailed Explanation

**Why PM2?**
- Node.js process manager
- Keeps app alive forever
- Built-in load balancer
- Zero-downtime reloads
- Log management

**When to Use PM2:**
- You have a VPS (DigitalOcean, Linode, AWS)
- You need full control over the server
- You want to run multiple Node.js apps on one server

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Deploy with PM2
// ============================================

// 1. Install PM2 globally on your server
// SSH into your VPS and run:
// npm install -g pm2

// 2. Create ecosystem.config.js (PM2 config)
// ecosystem.config.js
module.exports = {
    apps: [{
        name: 'myapp',
        script: './server.js',
        
        // Environment variables
        env: {
            NODE_ENV: 'development',
            PORT: 3000
        },
        env_production: {
            NODE_ENV: 'production',
            PORT: 8080
        },
        
        // Process management
        instances: 'max',  // Use all CPU cores (cluster mode)
        exec_mode: 'cluster',  // Load balance between instances
        autorestart: true,
        watch: false,  // Don't watch files in production
        max_memory_restart: '1G',  // Restart if memory > 1GB
        
        // Logging
        error_file: './logs/err.log',
        out_file: './logs/out.log',
        log_file: './logs/combined.log',
        time: true,  // Add timestamps to logs
        log_date_format: 'YYYY-MM-DD HH:mm:ss Z',
        
        // Restart strategy
        min_uptime: '10s',  // Consider app started after 10s
        max_restarts: 10,   // Max restarts in 30s window
        restart_delay: 4000,  // Wait 4s between restarts
        
        // Kill timeout
        kill_timeout: 5000,  // Wait 5s for graceful shutdown
        
        // Listen for events
        listen_timeout: 10000,  // Time to wait for app to listen
        
        // Merge logs from all instances
        merge_logs: true,
        
        // Instance variables
        instance_var: 'INSTANCE_ID'
    }],
    
    // Deployment configuration (optional)
    deploy: {
        production: {
            user: 'root',
            host: 'your-server-ip',
            ref: 'origin/main',
            repo: 'git@github.com:yourusername/myapp.git',
            path: '/var/www/myapp',
            'post-deploy': 'npm install && pm2 reload ecosystem.config.js --env production'
        }
    }
};
```

```javascript
// 3. Prepare your server.js for graceful shutdown
// server.js
const express = require('express');
const app = express();
const server = require('http').createServer(app);

const PORT = process.env.PORT || 3000;

// Handle graceful shutdown
process.on('SIGINT', gracefulShutdown);
process.on('SIGTERM', gracefulShutdown);

function gracefulShutdown() {
    console.log('Received shutdown signal, closing connections...');
    
    server.close(() => {
        console.log('All connections closed, exiting...');
        process.exit(0);
    });
    
    // Force close after 10 seconds
    setTimeout(() => {
        console.error('Could not close connections in time, forcing shutdown');
        process.exit(1);
    }, 10000);
}

server.listen(PORT, () => {
    console.log(`Server listening on port ${PORT}`);
    console.log(`Process ID: ${process.pid}`);
    console.log(`Instance: ${process.env.INSTANCE_ID || 'primary'}`);
});
```

**Deployment Steps:**

1. **Set up your VPS**
```bash
# Update server
sudo apt update && sudo apt upgrade -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Install PM2
sudo npm install -g pm2

# Install Git
sudo apt install git -y
```

2. **Deploy your app**
```bash
# Clone your repository
git clone https://github.com/yourusername/myapp.git
cd myapp

# Install dependencies
npm install

# Set environment variables (create .env file)
echo "DATABASE_URL=postgresql://..." >> .env
echo "JWT_SECRET=your-secret" >> .env

# Start with PM2
pm2 start ecosystem.config.js --env production

# Save PM2 process list (auto-start on reboot)
pm2 save
pm2 startup  # Follow instructions
```

**Useful PM2 Commands:**

```bash
# Process Management
pm2 list                    # List all processes
pm2 logs myapp             # View logs
pm2 logs myapp --lines 100 # Last 100 lines
pm2 monit                  # Real-time monitoring
pm2 status                 # Process status

# Restart and Reload
pm2 restart myapp          # Restart (brief downtime)
pm2 reload myapp           # Zero-downtime reload
pm2 stop myapp             # Stop process
pm2 delete myapp           # Delete from PM2

# Scaling
pm2 scale myapp 4          # Scale to 4 instances

# Monitoring
pm2 show myapp             # Detailed info
pm2 describe myapp         # Same as show
pm2 info myapp             # Process info

# Logs
pm2 flush                  # Clear all logs
pm2 reloadLogs            # Rotate logs

# Startup
pm2 save                   # Save current process list
pm2 startup                # Generate startup script
pm2 unstartup              # Remove startup script
```

**Setting Up Nginx Reverse Proxy:**

```nginx
# /etc/nginx/sites-available/myapp
server {
    listen 80;
    server_name your-domain.com;
    
    location / {
        proxy_pass http://localhost:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

# Enable site
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

**Monitoring with PM2 Plus (optional):**

```bash
# Install PM2 Plus (monitoring dashboard)
pm2 link <secret> <public>

# Or use built-in monitoring
pm2 monit
```

**Zero-Downtime Deployment Script:**

```bash
#!/bin/bash
# deploy.sh

echo "Deploying new version..."

# Pull latest code
git pull origin main

# Install dependencies
npm install --production

# Reload with PM2 (zero downtime)
pm2 reload ecosystem.config.js --env production

# Check status
pm2 status

echo "Deployment complete!"
```

</details>

#### 💡 Key Takeaways
- PM2 keeps Node.js apps running forever
- Use cluster mode for multi-core CPUs
- Always handle SIGTERM for graceful shutdown
- Set up PM2 to auto-start on server reboot
- Use Nginx as reverse proxy for production

---

### Problem 15: Environment Variables in Production

#### 📝 Problem Statement
You need to manage secrets and configuration across different production environments:
- Staging (testing)
- Production (live)
- Different team members need access
- Secrets must never be in git

Compare different approaches for managing environment variables in production.

#### 🔍 Detailed Explanation

**Managing Secrets:**
| Method | Security | Ease of Use | Best For |
|--------|----------|-------------|----------|
| Platform Dashboard | High | Easy | Heroku, Render |
| .env files | Medium | Hard | Local only |
| Secret Manager | Very High | Medium | AWS, GCP |
| CI/CD Variables | High | Easy | GitHub Actions |

<details>
<summary>🔽 Click to View Solution</summary>

```javascript
// ============================================
// Solution: Production Environment Management
// ============================================

// 1. Platform-specific environment variables
// Heroku
// heroku config:set DATABASE_URL=postgresql://...
// heroku config:set JWT_SECRET=strong-secret-here

// Render
// Dashboard → Environment → Add Environment Variable

// Vercel
// Project Settings → Environment Variables

// ============================================
// 2. Using .env files with different environments
// ============================================

// .env.staging
DATABASE_URL=postgresql://staging-db:5432/myapp
JWT_SECRET=staging-secret-123
API_URL=https://staging-api.com

// .env.production
DATABASE_URL=postgresql://prod-db:5432/myapp
JWT_SECRET=prod-secret-456
API_URL=https://api.myapp.com

// Load based on environment
// config.js
const env = process.env.NODE_ENV || 'development';
require('dotenv').config({ path: `.env.${env}` });

// ============================================
// 3. AWS Secrets Manager (for AWS deployments)
// ============================================

// Install: npm install @aws-sdk/client-secrets-manager

const { SecretsManagerClient, GetSecretValueCommand } = require('@aws-sdk/client-secrets-manager');

async function getSecret(secretName) {
    const client = new SecretsManagerClient({ region: 'us-east-1' });
    
    try {
        const response = await client.send(
            new GetSecretValueCommand({ SecretId: secretName })
        );
        
        return JSON.parse(response.SecretString);
    } catch (error) {
        console.error('Error getting secret:', error);
        throw error;
    }
}

// Usage
async function loadConfig() {
    const secrets = await getSecret('myapp/production');
    process.env.DATABASE_URL = secrets.DATABASE_URL;
    process.env.JWT_SECRET = secrets.JWT_SECRET;
}

// ============================================
// 4. GitHub Actions secrets (for CI/CD)
// ============================================

// .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Deploy to Render
        env:
          DATABASE_URL: ${{ secrets.DATABASE_URL }}
          JWT_SECRET: ${{ secrets.JWT_SECRET }}
          API_KEY: ${{ secrets.API_KEY }}
        run: |
          curl -X POST https://api.render.com/deploy/...
```

**Best Practices for Production Secrets:**

```javascript
// 5. Never hardcode secrets
// ❌ BAD
const apiKey = "sk_live_abc123xyz";

// ✅ GOOD
const apiKey = process.env.API_KEY;

// 6. Validate secrets on startup
function validateSecrets() {
    const required = ['DATABASE_URL', 'JWT_SECRET', 'API_KEY'];
    const missing = required.filter(key => !process.env[key]);
    
    if (missing.length > 0) {
        console.error('Missing required secrets:', missing.join(', '));
        process.exit(1);
    }
}

// 7. Rotate secrets regularly
// Set up a cron job or reminder to update:
// - JWT_SECRET: Every 90 days
// - Database passwords: Every 180 days
// - API Keys: Per vendor policy

// 8. Different secrets for different environments
const config = {
    staging: {
        jwtSecret: process.env.STAGING_JWT_SECRET,
        databaseUrl: process.env.STAGING_DATABASE_URL
    },
    production: {
        jwtSecret: process.env.PROD_JWT_SECRET,
        databaseUrl: process.env.PROD_DATABASE_URL
    }
};

// 9. Never log secrets
// ❌ BAD
console.log('Database URL:', process.env.DATABASE_URL);

// ✅ GOOD
console.log('Database connected');

// 10. Use secret rotation tools
// HashiCorp Vault, AWS Secrets Manager, GCP Secret Manager
```

**Team Access Management:**

```bash
# For team access to platform secrets:

# Heroku: Add team members
heroku access:add team-member@company.com -a myapp

# Render: Invite team members in dashboard

# Vercel: Add team members in dashboard

# For shared .env files (development only):
# Use .env.example in git
# Each developer creates their own .env
```

**Emergency Secret Rotation Script:**

```bash
#!/bin/bash
# rotate-secrets.sh

echo "Rotating secrets..."

# Generate new JWT secret
NEW_JWT_SECRET=$(openssl rand -base64 32)

# Update in production
heroku config:set JWT_SECRET=$NEW_JWT_SECRET -a myapp-prod

# Update in staging
heroku config:set JWT_SECRET=$NEW_JWT_SECRET -a myapp-staging

# Notify team
echo "JWT secret rotated at $(date)" | mail -s "Secret Rotation" team@company.com

echo "Secret rotation complete"
```

</details>

#### 💡 Key Takeaways
- Never commit secrets to git
- Use platform-specific secret management
- Different secrets for different environments
- Validate required secrets on startup
- Rotate secrets regularly

---

## CI/CD Basics (Problems 16-20)

---

### Problem 16: GitHub Actions - Basic CI Pipeline

#### 📝 Problem Statement
Set up a GitHub Actions workflow that:
- Runs on every push to main branch
- Runs on every pull request
- Installs dependencies
- Runs tests
- Fails if tests fail

#### 🔍 Detailed Explanation

**What is CI/CD?**
- **CI (Continuous Integration)** - Automatically test code when pushed
- **CD (Continuous Deployment)** - Automatically deploy after tests pass

**Why GitHub Actions?**
- Free for public repos
- 2000 free minutes/month for private
- Integrated with GitHub
- Easy to configure

<details>
<summary>🔽 Click to View Solution</summary>

```yaml
# ============================================
# Solution: GitHub Actions CI Pipeline
# ============================================

# File: .github/workflows/ci.yml
name: CI

# When to run
on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

# What to do
jobs:
  test:
    runs-on: ubuntu-latest
    
    # Strategy for different Node versions
    strategy:
      matrix:
        node-version: [16.x, 18.x]
    
    steps:
      # 1. Checkout code
      - name: Checkout code
        uses: actions/checkout@v3
      
      # 2. Setup Node.js
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
      
      # 3. Install dependencies
      - name: Install dependencies
        run: npm ci
      
      # 4. Run linter
      - name: Run linter
        run: npm run lint
      
      # 5. Run tests
      - name: Run tests
        run: npm test
      
      # 6. Run security audit
      - name: Security audit
        run: npm audit --production
      
      # 7. Upload test coverage (optional)
      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          token: ${{ secrets.CODECOV_TOKEN }}

  # Separate job for building
  build:
    runs-on: ubuntu-latest
    needs: test  # Wait for tests to pass
    
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18.x
      
      - name: Build
        run: |
          npm ci
          npm run build
      
      - name: Upload build artifact
        uses: actions/upload-artifact@v3
        with:
          name: build
          path: dist/
```

**package.json scripts for CI:**
```json
{
    "scripts": {
        "test": "jest --coverage",
        "test:ci": "jest --ci --coverage --maxWorkers=2",
        "lint": "eslint .",
        "build": "webpack --mode=production"
    }
}
```

**Testing database in CI:**

```yaml
# With PostgreSQL service
jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:14
        env:
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: testdb
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --timeout 5s
          --retries 5
        ports:
          - 5432:5432
    
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      
      - name: Run tests with database
        run: npm test
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/testdb
```

**Viewing CI results:**
- Go to your GitHub repository
- Click "Actions" tab
- See all workflow runs
- Click into a run to see details

</details>

#### 💡 Key Takeaways
- GitHub Actions runs tests automatically on every push
- Matrix strategy tests multiple Node versions
- Services like PostgreSQL can be added
- Always run security audit in CI
- Test coverage can be uploaded to services like Codecov

---

### Problem 17: GitHub Actions - Deploy to Render

#### 📝 Problem Statement
Create a GitHub Actions workflow that:
- Runs tests
- If tests pass, deploys to Render
- Only deploys on push to main branch
- Uses Render API for deployment

#### 🔍 Detailed Explanation

**Continuous Deployment Flow:**
1. Push code to GitHub
2. GitHub Actions runs tests
3. If tests pass, automatically deploy
4. New version is live

<details>
<summary>🔽 Click to View Solution</summary>

```yaml
# ============================================
# Solution: Deploy to Render from GitHub Actions
# ============================================

# File: .github/workflows/deploy-render.yml
name: Deploy to Render

on:
  push:
    branches: [ main ]
  workflow_dispatch:  # Allow manual trigger

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18.x
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm test
      
      - name: Run linting
        run: npm run lint
  
  deploy-staging:
    needs: test
    runs-on: ubuntu-latest
    environment: staging
    if: github.ref == 'refs/heads/main'
    
    steps:
      - name: Deploy to Render Staging
        run: |
          curl -X POST https://api.render.com/deploy/${{ secrets.RENDER_STAGING_SERVICE_ID }} \
          -H "Authorization: Bearer ${{ secrets.RENDER_API_KEY }}"
  
  deploy-production:
    needs: deploy-staging
    runs-on: ubuntu-latest
    environment: production
    if: github.ref == 'refs/heads/main'
    
    steps:
      - name: Wait for staging deployment
        run: sleep 30  # Give staging time to deploy
      
      - name: Deploy to Render Production
        run: |
          curl -X POST https://api.render.com/deploy/${{ secrets.RENDER_PROD_SERVICE_ID }} \
          -H "Authorization: Bearer ${{ secrets.RENDER_API_KEY }}"
      
      - name: Verify deployment
        run: |
          sleep 30
          curl -f https://myapp.onrender.com/health || exit 1
```

**Setting up secrets in GitHub:**
1. Go to repository → Settings → Secrets and variables → Actions
2. Add secrets:
   - `RENDER_API_KEY` - Get from Render dashboard
   - `RENDER_STAGING_SERVICE_ID` - Service ID from Render
   - `RENDER_PROD_SERVICE_ID` - Service ID from Render

**Alternative: Deploy to Heroku**

```yaml
# .github/workflows/deploy-heroku.yml
name: Deploy to Heroku

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to Heroku
        uses: akhileshns/heroku-deploy@v3.12.12
        with:
          heroku_api_key: ${{ secrets.HEROKU_API_KEY }}
          heroku_app_name: ${{ secrets.HEROKU_APP_NAME }}
          heroku_email: ${{ secrets.HEROKU_EMAIL }}
```

**Deploy to Vercel:**

```yaml
# .github/workflows/deploy-vercel.yml
name: Deploy to Vercel

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'
```

</details>

#### 💡 Key Takeaways
- Use GitHub Secrets for API keys
- Deploy staging first, then production
- Add health check to verify deployment
- Use `workflow_dispatch` for manual triggers
- Environment protection for production

---

### Problem 18: GitHub Actions - Run Database Migrations

#### 📝 Problem Statement
Create a workflow that automatically runs database migrations when deploying:
- Run migrations before deploying
- Only on production/staging
- Rollback if migration fails
- Log migration results

#### 🔍 Detailed Explanation

**Why Run Migrations in CI/CD?**
- Ensures database schema matches code
- Automates manual steps
- Prevents deployment with mismatched schema
- Can be rolled back

<details>
<summary>🔽 Click to View Solution</summary>

```yaml
# ============================================
# Solution: Run Migrations in CI/CD
# ============================================

# File: .github/workflows/deploy-with-migrations.yml
name: Deploy with Migrations

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm test
      
      - name: Run migrations on staging
        run: npm run migrate:up
        env:
          DATABASE_URL: ${{ secrets.STAGING_DATABASE_URL }}
      
      - name: Deploy to staging
        run: |
          curl -X POST https://api.render.com/deploy/${{ secrets.RENDER_STAGING_SERVICE_ID }} \
          -H "Authorization: Bearer ${{ secrets.RENDER_API_KEY }}"
      
      - name: Wait for staging
        run: sleep 60
      
      - name: Test staging
        run: curl -f https://myapp-staging.onrender.com/health
      
      - name: Run migrations on production
        run: npm run migrate:up
        env:
          DATABASE_URL: ${{ secrets.PROD_DATABASE_URL }}
      
      - name: Deploy to production
        run: |
          curl -X POST https://api.render.com/deploy/${{ secrets.RENDER_PROD_SERVICE_ID }} \
          -H "Authorization: Bearer ${{ secrets.RENDER_API_KEY }}"
```

**Migration scripts in package.json:**

```json
{
    "scripts": {
        "migrate:create": "node scripts/create-migration.js",
        "migrate:up": "node scripts/run-migrations.js up",
        "migrate:down": "node scripts/run-migrations.js down",
        "migrate:status": "node scripts/run-migrations.js status"
    }
}
```

**Migration runner script:**

```javascript
// scripts/run-migrations.js
const { Pool } = require('pg');
const fs = require('fs');
const path = require('path');

const command = process.argv[2];
const databaseUrl = process.env.DATABASE_URL;

if (!databaseUrl) {
    console.error('DATABASE_URL not set');
    process.exit(1);
}

const pool = new Pool({ connectionString: databaseUrl });

async function createMigrationsTable() {
    await pool.query(`
        CREATE TABLE IF NOT EXISTS migrations (
            id SERIAL PRIMARY KEY,
            name VARCHAR(255) NOT NULL UNIQUE,
            executed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        )
    `);
}

async function getExecutedMigrations() {
    const result = await pool.query('SELECT name FROM migrations ORDER BY id');
    return new Set(result.rows.map(row => row.name));
}

async function runMigrations() {
    await createMigrationsTable();
    
    const executed = await getExecutedMigrations();
    const migrationsDir = path.join(__dirname, '../migrations');
    const files = fs.readdirSync(migrationsDir).filter(f => f.endsWith('.sql')).sort();
    
    for (const file of files) {
        if (executed.has(file)) {
            console.log(`✓ Already executed: ${file}`);
            continue;
        }
        
        console.log(`→ Running migration: ${file}`);
        const sql = fs.readFileSync(path.join(migrationsDir, file), 'utf8');
        
        try {
            await pool.query('BEGIN');
            await pool.query(sql);
            await pool.query('INSERT INTO migrations (name) VALUES ($1)', [file]);
            await pool.query('COMMIT');
            console.log(`✓ Completed: ${file}`);
        } catch (error) {
            await pool.query('ROLLBACK');
            console.error(`✗ Failed: ${file}`);
            console.error(error);
            process.exit(1);
        }
    }
    
    await pool.end();
}

async function rollbackLastMigration() {
    await createMigrationsTable();
    
    const result = await pool.query(
        'SELECT name FROM migrations ORDER BY id DESC LIMIT 1'
    );
    
    if (result.rows.length === 0) {
        console.log('No migrations to rollback');
        return;
    }
    
    const lastMigration = result.rows[0].name;
    const rollbackFile = lastMigration.replace('.sql', '.rollback.sql');
    const rollbackPath = path.join(__dirname, '../migrations', rollbackFile);
    
    if (!fs.existsSync(rollbackPath)) {
        console.error(`No rollback file found for ${lastMigration}`);
        process.exit(1);
    }
    
    console.log(`→ Rolling back: ${lastMigration}`);
    const sql = fs.readFileSync(rollbackPath, 'utf8');
    
    try {
        await pool.query('BEGIN');
        await pool.query(sql);
        await pool.query('DELETE FROM migrations WHERE name = $1', [lastMigration]);
        await pool.query('COMMIT');
        console.log(`✓ Rollback complete: ${lastMigration}`);
    } catch (error) {
        await pool.query('ROLLBACK');
        console.error(`✗ Rollback failed: ${lastMigration}`);
        console.error(error);
        process.exit(1);
    }
    
    await pool.end();
}

async function showStatus() {
    await createMigrationsTable();
    const executed = await getExecutedMigrations();
    const migrationsDir = path.join(__dirname, '../migrations');
    const files = fs.readdirSync(migrationsDir).filter(f => f.endsWith('.sql')).sort();
    
    console.log('\nMigration Status:\n');
    for (const file of files) {
        const status = executed.has(file) ? '✓' : '○';
        console.log(`${status} ${file}`);
    }
    
    await pool.end();
}

// Run based on command
if (command === 'up') {
    runMigrations();
} else if (command === 'down') {
    rollbackLastMigration();
} else if (command === 'status') {
    showStatus();
} else {
    console.log('Usage: node run-migrations.js [up|down|status]');
}
```

**Example migration file:**

```sql
-- migrations/20240115000000_create_users_table.sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);

-- migrations/20240115000000_create_users_table.rollback.sql
DROP INDEX IF EXISTS idx_users_email;
DROP TABLE IF EXISTS users;
```

</details>

#### 💡 Key Takeaways
- Always run migrations BEFORE deploying code
- Keep rollback scripts for every migration
- Use transactions to ensure migrations are atomic
- Log migration results for debugging
- Test migrations on staging first

---

### Problem 19: GitHub Actions - Run Tests in Parallel

#### 📝 Problem Statement
Speed up CI by running tests in parallel:
- Unit tests (fast, many)
- Integration tests (slower)
- E2E tests (slowest)
- Each type runs in parallel

#### 🔍 Detailed Explanation

**Why Parallel Tests?**
- Faster feedback
- Better resource utilization
- Identify which type failed

<details>
<summary>🔽 Click to View Solution</summary>

```yaml
# ============================================
# Solution: Parallel Test Execution
# ============================================

# File: .github/workflows/parallel-tests.yml
name: Parallel Tests

on:
  push:
    branches: [ main ]
  pull_request:

jobs:
  # Unit tests (fast)
  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm ci
      - run: npm run test:unit
      - name: Upload unit test results
        uses: actions/upload-artifact@v3
        with:
          name: unit-test-results
          path: junit.xml
  
  # Integration tests (needs database)
  integration-tests:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:14
        env:
          POSTGRES_PASSWORD: postgres
        options: --health-cmd pg_isready
        ports:
          - 5432:5432
    
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm ci
      - run: npm run test:integration
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/postgres
  
  # E2E tests (needs full app)
  e2e-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm ci
      - run: npm run build
      - name: Start server
        run: |
          npm run start &
          sleep 5
      - run: npm run test:e2e
  
  # All tests must pass
  all-tests-passed:
    needs: [unit-tests, integration-tests, e2e-tests]
    runs-on: ubuntu-latest
    steps:
      - run: echo "All tests passed!"
```

**Split tests by file for even faster execution:**

```yaml
  split-tests:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        shard: [1, 2, 3, 4]  # Split into 4 shards
    
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm ci
      - run: npm run test:shard -- --shard=${{ matrix.shard }}/4
```

**package.json scripts:**
```json
{
    "scripts": {
        "test:unit": "jest --testMatch='**/*.test.js'",
        "test:integration": "jest --testMatch='**/*.integration.js' --runInBand",
        "test:e2e": "cypress run",
        "test:shard": "jest --shard"
    }
}
```

</details>

#### 💡 Key Takeaways
- Split tests by type for parallel execution
- Use services for database dependencies
- Run E2E tests last (slowest)
- Upload test artifacts for debugging
- Combine results with a final gate job

---

### Problem 20: GitHub Actions - Auto-Merge Dependabot PRs

#### 📝 Problem Statement
Automatically merge dependency updates from Dependabot if:
- All tests pass
- Security updates (patch version)
- Non-breaking changes

#### 🔍 Detailed Explanation

**What is Dependabot?**
- GitHub's automated dependency update tool
- Creates PRs when dependencies are outdated
- Can auto-merge safe updates

<details>
<summary>🔽 Click to View Solution</summary>

```yaml
# ============================================
# Solution: Auto-Merge Dependabot PRs
# ============================================

# File: .github/workflows/auto-merge-dependabot.yml
name: Auto-merge Dependabot PRs

on:
  pull_request:
    types: [opened, synchronize, reopened, labeled, unlabeled]

jobs:
  auto-merge:
    runs-on: ubuntu-latest
    if: github.actor == 'dependabot[bot]'
    
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      
      - name: Get PR info
        id: pr-info
        run: |
          PR_TITLE="${{ github.event.pull_request.title }}"
          echo "title=$PR_TITLE" >> $GITHUB_OUTPUT
          
          # Check if it's a security update
          if [[ "$PR_TITLE" == *"security"* ]]; then
            echo "is_security=true" >> $GITHUB_OUTPUT
          else
            echo "is_security=false" >> $GITHUB_OUTPUT
          fi
      
      - name: Auto-merge security updates
        if: steps.pr-info.outputs.is_security == 'true'
        run: |
          gh pr merge ${{ github.event.pull_request.number }} \
            --auto \
            --squash \
            --delete-branch
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Auto-merge patch updates
        if: contains(steps.pr-info.outputs.title, 'patch')
        run: |
          gh pr merge ${{ github.event.pull_request.number }} \
            --auto \
            --squash \
            --delete-branch
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

**Dependabot configuration:**

```yaml
# .github/dependabot.yml
version: 2
updates:
  # npm dependencies
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 10
    versioning-strategy: auto
    labels:
      - "dependencies"
      - "automerge"
    ignore:
      - dependency-name: "express"
        versions: ["5.x"]  # Don't update to v5 yet
  
  # GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
    labels:
      - "dependencies"
      - "automerge"
  
  # Docker
  - package-ecosystem: "docker"
    directory: "/"
    schedule:
      interval: "weekly"
    labels:
      - "dependencies"
```

**Branch protection rules:**
1. Go to Repository → Settings → Branches
2. Add branch protection rule for `main`
3. Require status checks to pass
4. Require branches to be up-to-date

**Manual merge for major updates:**

```yaml
# .github/workflows/dependabot-review.yml
name: Review Dependabot PRs

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  comment-major-update:
    runs-on: ubuntu-latest
    if: github.actor == 'dependabot[bot]'
    
    steps:
      - name: Check if major update
        run: |
          PR_TITLE="${{ github.event.pull_request.title }}"
          if [[ "$PR_TITLE" == *"major"* ]]; then
            echo "MAJOR_UPDATE=true" >> $GITHUB_ENV
          fi
      
      - name: Add comment for major updates
        if: env.MAJOR_UPDATE == 'true'
        run: |
          gh pr comment ${{ github.event.pull_request.number }} \
            --body "⚠️ **Major Version Update** ⚠️\n\nThis is a major version update. Please review breaking changes before merging.\n\ncc @team-leads"
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Request review for major updates
        if: env.MAJOR_UPDATE == 'true'
        run: |
          gh pr edit ${{ github.event.pull_request.number }} \
            --add-reviewer team-leads
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

</details>

#### 💡 Key Takeaways
- Dependabot keeps dependencies secure
- Auto-merge only safe updates (patch, security)
- Require tests to pass before merge
- Manual review for major version updates
- Use labels to categorize dependency PRs

---

## Summary: Deployment Cheat Sheet

### Platform Comparison

| Feature | Render | Heroku | Vercel | VPS + PM2 |
|---------|--------|--------|--------|-----------|
| Free Tier | Yes (always on) | Yes (sleeps) | Yes | No (pay for VPS) |
| Database | Free 256MB | Free 10k rows | No | You manage |
| SSL | Auto | Auto | Auto | Manual |
| Custom Domain | Yes | Yes | Yes | Yes |
| Ease of Use | Very Easy | Easy | Very Easy | Hard |
| Best For | Small apps | Prototypes | APIs | Full control |

### Quick Commands Reference

```bash
# Render
# Deploy via GitHub (automatic)
# Set env vars: Dashboard → Environment

# Heroku
heroku create myapp
git push heroku main
heroku config:set KEY=value
heroku logs --tail

# Vercel
vercel --prod
vercel env add SECRET_NAME

# PM2
pm2 start server.js -i max
pm2 save
pm2 startup
pm2 logs myapp
```

### Environment Variables Best Practices
1. Never commit `.env` files
2. Use `.env.example` as template
3. Validate required variables on startup
4. Different secrets for different environments
5. Rotate secrets regularly

### CI/CD Checklist
- [ ] Tests run on every push
- [ ] Deploy only after tests pass
- [ ] Run database migrations before deploy
- [ ] Health check after deploy
- [ ] Rollback plan exists
- [ ] Secrets stored securely
- [ ] Staging environment before production

---

**🎉 Congratulations! You've completed the Deployment module.**

These 20 problems cover everything a beginner needs to deploy Node.js applications to production.

</details>

---

This completes the Deployment module with 20 beginner-friendly problems covering environment configuration, production readiness, hosting platforms, and CI/CD basics.