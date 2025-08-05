# Technical Architecture Documentation
## Zenudy/Gaptify Educational Testing Platform

**Document Version:** 1.0  
**Date:** August 5, 2025  

---

## Table of Contents

1. [System Architecture Overview](#1-system-architecture-overview)
2. [Frontend Architecture](#2-frontend-architecture)
3. [Backend Architecture](#3-backend-architecture)
4. [Database Architecture](#4-database-architecture)
5. [Security Architecture](#5-security-architecture)
6. [Deployment Architecture](#6-deployment-architecture)
7. [Integration Patterns](#7-integration-patterns)

---

## 1. System Architecture Overview

### 1.1 High-Level Architecture

The Zenudy/Gaptify platform follows a modern microservices-oriented architecture with the following key components:

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Super Admin   │    │ Learning Center │    │  Student Portal │
│     Portal      │    │  Admin Portal   │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  Student Test   │    │   Parent CTA    │    │      API        │
│   Interface     │    │    Portal       │    │    Gateway      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                        ┌─────────────────┐
                        │   Backend API   │
                        │   (Node.js)     │
                        └─────────────────┘
                                 │
                        ┌─────────────────┐
                        │   PostgreSQL    │
                        │    Database     │
                        └─────────────────┘
```

### 1.2 Technology Stack

#### Frontend Stack
- **Framework**: Angular 16.2.12
- **UI Components**: Bootstrap 4.5.0, Angular Material 16.2.14
- **State Management**: RxJS Observables
- **Build Tool**: Angular CLI with Webpack
- **Testing**: Jasmine, Karma
- **Additional Libraries**:
  - ngx-spinner (loading indicators)
  - ngx-toastr (notifications)
  - ngx-pagination (pagination)
  - CKEditor 5 (rich text editing)
  - Highcharts (data visualization)

#### Backend Stack
- **Runtime**: Node.js
- **Framework**: Express.js 4.21.1
- **Database**: PostgreSQL with pg driver
- **Authentication**: JWT tokens
- **File Handling**: express-fileupload
- **Email**: Nodemailer 6.9.16
- **Process Management**: PM2 5.4.3
- **Additional Libraries**:
  - cors (Cross-Origin Resource Sharing)
  - body-parser (request parsing)
  - puppeteer (PDF generation)
  - node-cron (scheduled tasks)

---

## 2. Frontend Architecture

### 2.1 Application Structure

The frontend consists of five main Angular applications:

#### 2.1.1 Super Admin Portal (`Super Admin/zenudy-web/`)
```
src/
├── app/
│   ├── components/
│   │   ├── home/
│   │   ├── login/
│   │   ├── settings/
│   │   ├── learning-center-manager/
│   │   ├── test-manager/
│   │   └── subscription/
│   ├── services/
│   │   ├── auth.service.ts
│   │   ├── helper.service.ts
│   │   └── learning-center.service.ts
│   ├── guards/
│   ├── interceptors/
│   └── pipes/
```

#### 2.1.2 Learning Center Admin Portal (`zenudy-web/`)
```
src/
├── app/
│   ├── components/
│   │   ├── add-question/
│   │   ├── edit-question/
│   │   ├── test-manager/
│   │   ├── candidate/
│   │   ├── report/
│   │   └── settings/
│   ├── services/
│   │   ├── question.service.ts
│   │   ├── candidate.service.ts
│   │   └── test.service.ts
```

#### 2.1.3 Student Portal (`Student Portal/zenudy-web/`)
```
src/
├── app/
│   ├── components/
│   │   ├── dashboard/
│   │   ├── login/
│   │   ├── update-profile/
│   │   └── report/
│   ├── services/
│   │   ├── student.service.ts
│   │   └── auth.service.ts
```

#### 2.1.4 Student Test Interface (`zenudy-ui/student_test/`)
```
src/
├── app/
│   ├── components/
│   │   ├── test/
│   │   │   ├── test-start/
│   │   │   ├── test-play/
│   │   │   ├── test-reading-play/
│   │   │   ├── test-math-play/
│   │   │   └── test-submit/
│   │   ├── calculator/
│   │   └── camera/
```

#### 2.1.5 Parent CTA Portal (`zenudy parent cta/parent-cta/`)
```
src/
├── app/
│   ├── components/
│   │   ├── parent-form/
│   │   ├── student-intake/
│   │   └── learning-center-signup/
```

### 2.2 Common Architectural Patterns

#### 2.2.1 Service Architecture
All applications follow a service-oriented architecture:
```typescript
@Injectable({ providedIn: 'root' })
export class AuthService {
  private apiUrl = environment.apiUrl;
  
  login(credentials: LoginCredentials): Observable<AuthResponse> {
    return this.http.post<AuthResponse>(`${this.apiUrl}/login`, credentials);
  }
}
```

#### 2.2.2 Component Communication
- Parent-Child: `@Input()` and `@Output()` decorators
- Service Communication: Shared services with RxJS Subjects
- State Management: Service-based state with BehaviorSubjects

#### 2.2.3 Routing Architecture
```typescript
const routes: Routes = [
  { path: '', redirectTo: 'login', pathMatch: 'full' },
  { path: 'login', component: LoginComponent },
  { path: 'home', component: HomeComponent, canActivate: [AuthGuard] },
  { path: 'test', loadChildren: () => import('./test/test.module').then(m => m.TestModule) }
];
```

### 2.3 UI/UX Architecture

#### 2.3.1 Responsive Design
- Bootstrap 4 grid system
- Mobile-first approach
- Breakpoints: 320px, 768px, 1024px, 1200px

#### 2.3.2 Theme System
- CSS custom properties for theming
- Learning center-specific branding
- Primary/secondary color customization

#### 2.3.3 Accessibility
- ARIA labels and roles
- Keyboard navigation support
- Screen reader compatibility
- High contrast mode support

---

## 3. Backend Architecture

### 3.1 API Architecture

The backend follows RESTful API principles with the following structure:

```
zenudy-api/
├── app.js                 # Application entry point
├── bin/www               # Server startup
├── config/
│   └── db.js            # Database configuration
├── controllers/
│   └── api/
│       ├── admin.js
│       ├── superAdmin.js
│       ├── students.js
│       ├── test.js
│       ├── questions.js
│       └── report.js
├── routes/
│   ├── adminRoutes.js
│   ├── superAdminRoutes.js
│   ├── candidateRoutes.js
│   └── studentRoutes.js
├── middleware/
├── services/
└── helpers/
```

### 3.2 Controller Architecture

#### 3.2.1 Admin Controller (`controllers/api/admin.js`)
```javascript
// User authentication and management
const userLogin = (request, response) => {
  // JWT token generation
  // User validation
  // Session management
};

// Test management operations
const getTestList = (request, response) => {
  // Database queries
  // Data formatting
  // Response handling
};
```

#### 3.2.2 Test Controller (`controllers/api/test.js`)
```javascript
// Test creation and configuration
const createTest = (request, response) => {
  // Validation
  // Database operations
  // File handling
};

// Question selection and assignment
const questionSelections = (request, response) => {
  // Bulk operations
  // Transaction management
};
```

### 3.3 Database Integration

#### 3.3.1 Connection Management
```javascript
const Pool = require('pg').Pool;

const pool = new Pool({
  user: process.env.DB_USER,
  host: process.env.DB_HOST,
  database: process.env.DATABASE,
  password: process.env.DB_PASSWORD,
  port: process.env.DB_PORT,
  ssl: { rejectUnauthorized: false }
});
```

#### 3.3.2 Query Patterns
```javascript
// Parameterized queries for security
pool.query(
  'SELECT * FROM users WHERE email = $1 AND status = $2',
  [email, true],
  (error, results) => {
    // Handle results
  }
);
```

### 3.4 Middleware Architecture

#### 3.4.1 Authentication Middleware
```javascript
const basicAuth = (req, res, next) => {
  // JWT token validation
  // User authorization
  // Request context setup
};
```

#### 3.4.2 Error Handling Middleware
```javascript
const errorHandler = (err, req, res, next) => {
  // Error logging
  // Response formatting
  // Status code determination
};
```

---

## 4. Database Architecture

### 4.1 Database Schema Design

#### 4.1.1 Core Entities

**Users and Authentication**
```sql
-- Main user table
users (
  user_id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE,
  password_hash VARCHAR(255),
  learning_center_id INTEGER,
  user_status BOOLEAN,
  created_date TIMESTAMP
);

-- Authentication tokens
user_tokens (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(user_id),
  token TEXT,
  login_time TIMESTAMP,
  logout_time TIMESTAMP
);
```

**Learning Centers**
```sql
learning_center (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255),
  display_title VARCHAR(255),
  sub_domain VARCHAR(100),
  admin_email VARCHAR(255),
  primary_color VARCHAR(7),
  secondary_color VARCHAR(7),
  subscription_id INTEGER,
  created_date TIMESTAMP
);
```

**Students**
```sql
student (
  id SERIAL PRIMARY KEY,
  student_id VARCHAR(50) UNIQUE,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  email VARCHAR(255),
  learning_center_id INTEGER,
  grade_level_id INTEGER,
  profile_picture TEXT,
  created_date TIMESTAMP
);
```

#### 4.1.2 Test Management Schema

**Tests**
```sql
test (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255),
  test_template_id INTEGER,
  total_questions INTEGER,
  total_marks DECIMAL,
  duration INTEGER,
  learning_center_id INTEGER,
  created_date TIMESTAMP
);

-- Test configuration
test_settings (
  id SERIAL PRIMARY KEY,
  test_id INTEGER REFERENCES test(id),
  reading_fluency_assessment VARCHAR(10),
  show_audio BOOLEAN,
  enable_voice_recording BOOLEAN,
  test_inactivity_time INTEGER
);
```

**Questions**
```sql
question (
  id SERIAL PRIMARY KEY,
  question_type_id INTEGER,
  question TEXT,
  question_audio TEXT,
  learning_center_id INTEGER,
  subject_id INTEGER,
  chapter_id INTEGER,
  created_date TIMESTAMP
);

-- Question options for MCQ/MRQ
question_option (
  id SERIAL PRIMARY KEY,
  question_id INTEGER REFERENCES question(id),
  option_text TEXT,
  option_audio TEXT,
  is_correct BOOLEAN,
  option_order INTEGER
);

-- Test-Question relationships
test_questions (
  id SERIAL PRIMARY KEY,
  test_id INTEGER REFERENCES test(id),
  question_id INTEGER REFERENCES question(id),
  right_marks DECIMAL,
  negative_marks DECIMAL,
  question_order INTEGER
);
```

#### 4.1.3 Assessment Data Schema

**Ongoing Tests**
```sql
ongoing_test (
  id SERIAL PRIMARY KEY,
  test_id INTEGER REFERENCES test(id),
  student_id INTEGER REFERENCES student(id),
  start_time TIMESTAMP,
  end_time TIMESTAMP,
  remaining_time INTEGER,
  is_enable INTEGER,
  no_of_attempts INTEGER
);
```

**Student Responses**
```sql
student_test_answers (
  id SERIAL PRIMARY KEY,
  ongoing_test_id INTEGER REFERENCES ongoing_test(id),
  question_id INTEGER REFERENCES question(id),
  selected_options TEXT,
  text_answer TEXT,
  audio_response TEXT,
  time_spent INTEGER,
  created_date TIMESTAMP
);
```

### 4.2 Database Performance Optimization

#### 4.2.1 Indexing Strategy
```sql
-- Primary indexes
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_student_learning_center ON student(learning_center_id);
CREATE INDEX idx_test_questions_test_id ON test_questions(test_id);
CREATE INDEX idx_ongoing_test_student_test ON ongoing_test(student_id, test_id);

-- Composite indexes for common queries
CREATE INDEX idx_test_center_date ON test(learning_center_id, created_date);
CREATE INDEX idx_questions_subject_chapter ON question(subject_id, chapter_id);
```

#### 4.2.2 Query Optimization
- Use EXPLAIN ANALYZE for query planning
- Implement connection pooling
- Use prepared statements for security and performance
- Implement proper transaction boundaries

---

## 5. Security Architecture

### 5.1 Authentication Architecture

#### 5.1.1 JWT Token System
```javascript
// Token generation
const generateToken = (user) => {
  return jwt.sign(
    { 
      user_id: user.user_id,
      learning_center_id: user.learning_center_id,
      role: user.role 
    },
    process.env.JWT_SECRET,
    { expiresIn: '24h' }
  );
};
```

#### 5.1.2 Token Validation
```javascript
const validateToken = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    return res.status(401).json({ message: 'Invalid token' });
  }
};
```

### 5.2 Authorization Architecture

#### 5.2.1 Role-Based Access Control
```javascript
const authorize = (roles) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ message: 'Insufficient permissions' });
    }
    next();
  };
};
```

#### 5.2.2 Resource-Level Authorization
```javascript
const checkLearningCenterAccess = (req, res, next) => {
  const userLearningCenterId = req.user.learning_center_id;
  const requestedLearningCenterId = req.params.learning_center_id;
  
  if (userLearningCenterId !== requestedLearningCenterId) {
    return res.status(403).json({ message: 'Access denied' });
  }
  next();
};
```

### 5.3 Data Security

#### 5.3.1 Input Validation
```javascript
const validateTestData = (req, res, next) => {
  const { name, duration, total_questions } = req.body;
  
  if (!name || typeof name !== 'string' || name.length > 255) {
    return res.status(400).json({ message: 'Invalid test name' });
  }
  
  if (duration && (!Number.isInteger(duration) || duration < 1)) {
    return res.status(400).json({ message: 'Invalid duration' });
  }
  
  next();
};
```

#### 5.3.2 SQL Injection Prevention
```javascript
// Always use parameterized queries
const getUserByEmail = (email) => {
  return pool.query(
    'SELECT * FROM users WHERE email = $1 AND user_status = $2',
    [email, true]
  );
};
```

### 5.4 File Security

#### 5.4.1 File Upload Validation
```javascript
const validateFileUpload = (req, res, next) => {
  if (!req.files) {
    return next();
  }
  
  const file = req.files.uploadFile;
  const allowedTypes = ['image/jpeg', 'image/png', 'application/pdf', 'audio/mp3'];
  const maxSize = 50 * 1024 * 1024; // 50MB
  
  if (!allowedTypes.includes(file.mimetype)) {
    return res.status(400).json({ message: 'File type not allowed' });
  }
  
  if (file.size > maxSize) {
    return res.status(400).json({ message: 'File too large' });
  }
  
  next();
};
```

---

## 6. Deployment Architecture

### 6.1 Production Environment

#### 6.1.1 Server Architecture
```
Load Balancer (Nginx)
├── Frontend Servers (Multiple instances)
│   ├── Super Admin Portal
│   ├── Admin Portal
│   ├── Student Portal
│   ├── Test Interface
│   └── Parent Portal
└── Backend API Servers (Multiple instances)
    ├── Node.js Application (PM2)
    ├── PostgreSQL Database
    └── File Storage
```

#### 6.1.2 PM2 Configuration
```javascript
// ecosystem.config.js
module.exports = {
  apps: [{
    name: 'zenudy-api',
    script: './bin/www',
    instances: 'max',
    exec_mode: 'cluster',
    env: {
      NODE_ENV: 'production',
      PORT: 3000
    },
    error_file: './logs/err.log',
    out_file: './logs/out.log',
    log_file: './logs/combined.log'
  }]
};
```

### 6.2 Development Environment

#### 6.2.1 Local Development Setup
```bash
# Backend setup
cd zenudy-api
npm install
npm start

# Frontend setup (for each portal)
cd zenudy-web
npm install
ng serve --port 4200

cd "Student Portal/zenudy-web"
ng serve --port 4201

cd "zenudy-ui/student_test"
ng serve --port 4202
```

### 6.3 Build and Deployment Process

#### 6.3.1 Frontend Build Process
```bash
# Production build for each portal
ng build --configuration production --output-path dist/

# Build artifacts optimization
# - Code minification
# - Tree shaking
# - AOT compilation
# - Bundle optimization
```

#### 6.3.2 Backend Deployment
```bash
# Install dependencies
npm ci --only=production

# Start with PM2
pm2 start ecosystem.config.js

# Database migrations
npm run migrate

# Health check
pm2 status
```

---

## 7. Integration Patterns

### 7.1 Frontend-Backend Integration

#### 7.1.1 HTTP Client Configuration
```typescript
@Injectable()
export class ApiService {
  private baseUrl = environment.apiUrl;
  
  constructor(private http: HttpClient) {}
  
  get<T>(endpoint: string): Observable<T> {
    return this.http.get<T>(`${this.baseUrl}${endpoint}`);
  }
  
  post<T>(endpoint: string, data: any): Observable<T> {
    return this.http.post<T>(`${this.baseUrl}${endpoint}`, data);
  }
}
```

#### 7.1.2 Error Handling
```typescript
@Injectable()
export class ErrorInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(req).pipe(
      catchError((error: HttpErrorResponse) => {
        if (error.status === 401) {
          // Handle authentication errors
          this.authService.logout();
        }
        return throwError(error);
      })
    );
  }
}
```

### 7.2 Database Integration Patterns

#### 7.2.1 Transaction Management
```javascript
const createTestWithQuestions = async (testData, questions) => {
  const client = await pool.connect();
  
  try {
    await client.query('BEGIN');
    
    // Create test
    const testResult = await client.query(
      'INSERT INTO test (name, learning_center_id) VALUES ($1, $2) RETURNING id',
      [testData.name, testData.learning_center_id]
    );
    
    const testId = testResult.rows[0].id;
    
    // Add questions
    for (const question of questions) {
      await client.query(
        'INSERT INTO test_questions (test_id, question_id) VALUES ($1, $2)',
        [testId, question.id]
      );
    }
    
    await client.query('COMMIT');
    return testId;
    
  } catch (error) {
    await client.query('ROLLBACK');
    throw error;
  } finally {
    client.release();
  }
};
```

### 7.3 File Handling Integration

#### 7.3.1 File Upload Processing
```javascript
const handleFileUpload = async (req, res) => {
  if (!req.files || !req.files.uploadFile) {
    return res.status(400).json({ message: 'No file uploaded' });
  }
  
  const file = req.files.uploadFile;
  const fileName = `${Date.now()}_${file.name}`;
  const uploadPath = path.join(__dirname, '../public/uploads/', fileName);
  
  try {
    await file.mv(uploadPath);
    
    // Save file reference to database
    await pool.query(
      'INSERT INTO uploaded_files (original_name, file_path, user_id) VALUES ($1, $2, $3)',
      [file.name, fileName, req.user.user_id]
    );
    
    res.json({ 
      success: true, 
      fileName: fileName,
      url: `/public/uploads/${fileName}`
    });
    
  } catch (error) {
    res.status(500).json({ message: 'File upload failed', error: error.message });
  }
};
```

### 7.4 Email Integration

#### 7.4.1 Email Service Configuration
```javascript
const nodemailer = require('nodemailer');

const emailTransporter = nodemailer.createTransporter({
  host: process.env.SMTP_HOST,
  port: process.env.SMTP_PORT,
  secure: true,
  auth: {
    user: process.env.SMTP_USER,
    pass: process.env.SMTP_PASS
  }
});

const sendTestAssignmentEmail = async (studentEmail, testName, testUrl) => {
  const mailOptions = {
    from: process.env.FROM_EMAIL,
    to: studentEmail,
    subject: `New Test Assignment: ${testName}`,
    html: `
      <h2>New Test Assignment</h2>
      <p>You have been assigned a new test: <strong>${testName}</strong></p>
      <p><a href="${testUrl}">Click here to start the test</a></p>
    `
  };
  
  try {
    await emailTransporter.sendMail(mailOptions);
    console.log('Test assignment email sent successfully');
  } catch (error) {
    console.error('Email sending failed:', error);
  }
};
```

---

**Document Control:**
- **Version**: 1.0
- **Created**: August 5, 2025
- **Classification**: Technical Documentation
- **Review Cycle**: Quarterly

This technical architecture document provides a comprehensive overview of the system's technical implementation, serving as a reference for developers, architects, and technical stakeholders.
