# Software Requirements Specification (SRS)
## Zenudy/Gaptify Educational Testing Platform

**Document Version:** 1.0  
**Date:** August 5, 2025  
**Prepared by:** System Analysis Team  

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Overall Description](#2-overall-description)
3. [System Features](#3-system-features)
4. [External Interface Requirements](#4-external-interface-requirements)
5. [Non-Functional Requirements](#5-non-functional-requirements)
6. [System Architecture](#6-system-architecture)
7. [Database Requirements](#7-database-requirements)
8. [Security Requirements](#8-security-requirements)
9. [Appendices](#9-appendices)

---

## 1. Introduction

### 1.1 Purpose
This Software Requirements Specification (SRS) document describes the functional and non-functional requirements for the Zenudy/Gaptify Educational Testing Platform - a comprehensive web-based assessment system designed for educational institutions, learning centers, and students.

### 1.2 Scope
The Zenudy/Gaptify platform is a multi-portal educational assessment system that includes:
- **Super Admin Portal**: System-wide administration and learning center management
- **Admin Portal**: Learning center administration and test management
- **Student Portal**: Student dashboard and test access
- **Student Test Interface**: Dedicated testing environment
- **Parent Portal**: Parent access to student progress and information

### 1.3 Definitions, Acronyms, and Abbreviations
- **API**: Application Programming Interface
- **CTA**: Call To Action
- **LMS**: Learning Management System
- **MCP**: Multiple Choice with Partial Credit
- **MCQ**: Multiple Choice Question
- **MRQ**: Multiple Response Question
- **SPA**: Single Page Application
- **TFQ**: True/False Question

### 1.4 References
- Angular Framework Documentation (v16.2.12)
- Node.js Documentation
- PostgreSQL Documentation
- Bootstrap 4 Documentation

### 1.5 Overview
This document provides a comprehensive specification of the educational testing platform, including detailed functional requirements, system architecture, user interfaces, and technical constraints.

---

## 2. Overall Description

### 2.1 Product Perspective
The Zenudy/Gaptify platform is a comprehensive educational assessment solution that operates as a multi-tenant SaaS platform. It consists of:

#### 2.1.1 Frontend Applications
- **Super Admin Portal**: Angular-based administrative interface
- **Learning Center Admin Portal**: Institution management interface
- **Student Portal**: Student dashboard and profile management
- **Student Test Interface**: Dedicated testing environment
- **Parent CTA Portal**: Parent engagement interface

#### 2.1.2 Backend Services
- **RESTful API**: Node.js/Express-based backend
- **Database**: PostgreSQL database system
- **File Storage**: Document and media file management
- **Email Services**: Automated email notifications

### 2.2 Product Functions

#### 2.2.1 User Management
- Multi-level user authentication and authorization
- Profile management for students, parents, and administrators
- Learning center management and white-labeling

#### 2.2.2 Test Management
- Test creation with multiple question types
- Adaptive testing algorithms
- Audio-enabled questions and instructions
- Timer and session management

#### 2.2.3 Assessment Features
- Multiple question types (MCQ, MRQ, TFQ, Essay, Subjective)
- Audio recording capabilities
- Mathematical equation support
- Reading fluency assessment

#### 2.2.4 Reporting System
- Comprehensive test reports
- Diagnostic assessments (Math, Language, Reading)
- Progress tracking and analytics
- PDF and Excel report generation

### 2.3 User Classes and Characteristics

#### 2.3.1 Super Administrator
- **Characteristics**: Technical expertise, system-wide access
- **Responsibilities**: Platform management, learning center oversight, system configuration

#### 2.3.2 Learning Center Administrator
- **Characteristics**: Educational background, institutional authority
- **Responsibilities**: Test creation, student management, report generation

#### 2.3.3 Students
- **Characteristics**: Varying ages and technical skills
- **Responsibilities**: Taking tests, viewing results, profile management

#### 2.3.4 Parents/Guardians
- **Characteristics**: Interested in student progress, limited technical expertise
- **Responsibilities**: Monitoring student performance, profile updates

### 2.4 Operating Environment
- **Client Side**: Modern web browsers (Chrome, Firefox, Safari, Edge)
- **Server Side**: Node.js runtime environment
- **Database**: PostgreSQL 12+
- **Operating System**: Linux-based servers
- **Cloud Platform**: AWS/Azure compatible

### 2.5 Design and Implementation Constraints
- **Technology Stack**: Angular 16, Node.js, PostgreSQL
- **Browser Compatibility**: Modern browsers with ES6 support
- **Responsive Design**: Mobile and tablet compatibility
- **Security**: HTTPS encryption, JWT authentication
- **Performance**: Sub-3 second page load times

---

## 3. System Features

### 3.1 User Authentication and Authorization

#### 3.1.1 Description
Secure multi-level authentication system supporting different user roles and permissions.

#### 3.1.2 Functional Requirements
- **REQ-001**: System shall support JWT-based authentication
- **REQ-002**: System shall provide role-based access control
- **REQ-003**: System shall support password reset functionality
- **REQ-004**: System shall maintain session security and timeout
- **REQ-005**: System shall support multi-factor authentication capabilities

#### 3.1.3 User Classes
- Super Administrator
- Learning Center Administrator
- Teacher/Instructor
- Student
- Parent/Guardian

### 3.2 Test Creation and Management

#### 3.2.1 Description
Comprehensive test creation system supporting multiple question types and advanced features.

#### 3.2.2 Functional Requirements
- **REQ-006**: System shall support multiple question types:
  - Multiple Choice Questions (MCQ)
  - Multiple Response Questions (MRQ)
  - True/False Questions (TFQ)
  - Essay Questions
  - Subjective Questions
- **REQ-007**: System shall support audio-enabled questions
- **REQ-008**: System shall provide mathematical equation editor
- **REQ-009**: System shall support test templates and categories
- **REQ-010**: System shall allow test scheduling and assignment
- **REQ-011**: System shall support adaptive testing algorithms
- **REQ-012**: System shall provide test preview functionality

#### 3.2.3 Question Management Features
- **REQ-013**: System shall support question import/export
- **REQ-014**: System shall provide question bank organization
- **REQ-015**: System shall support question tagging and categorization
- **REQ-016**: System shall allow question difficulty levels
- **REQ-017**: System shall support grouped questions

### 3.3 Student Testing Interface

#### 3.3.1 Description
Dedicated testing environment providing secure and user-friendly test-taking experience.

#### 3.3.2 Functional Requirements
- **REQ-018**: System shall provide distraction-free testing interface
- **REQ-019**: System shall support timer functionality with visual indicators
- **REQ-020**: System shall auto-save student responses
- **REQ-021**: System shall support navigation between questions
- **REQ-022**: System shall provide question palette for status tracking
- **REQ-023**: System shall support audio playback for questions
- **REQ-024**: System shall support audio recording for responses
- **REQ-025**: System shall provide calculator functionality
- **REQ-026**: System shall support break time management
- **REQ-027**: System shall detect and handle browser/session interruptions

#### 3.3.3 Testing Features
- **REQ-028**: System shall support reading fluency assessment
- **REQ-029**: System shall provide mathematical expression input
- **REQ-030**: System shall support image-based questions
- **REQ-031**: System shall implement proctoring capabilities
- **REQ-032**: System shall support test submission and review

### 3.4 Reporting and Analytics

#### 3.4.1 Description
Comprehensive reporting system providing detailed insights into student performance and test analytics.

#### 3.4.2 Functional Requirements
- **REQ-033**: System shall generate detailed test reports
- **REQ-034**: System shall provide diagnostic assessment reports:
  - Math Diagnostic
  - Language Arts Diagnostic
  - Reading Diagnostic
- **REQ-035**: System shall support PDF report generation
- **REQ-036**: System shall support Excel report export
- **REQ-037**: System shall provide real-time progress tracking
- **REQ-038**: System shall generate comparison reports
- **REQ-039**: System shall support custom report templates
- **REQ-040**: System shall provide statistical analysis

#### 3.4.3 Report Types
- Individual student reports
- Class/group performance reports
- Test item analysis
- Learning center performance metrics
- Historical progress reports

### 3.5 Learning Center Management

#### 3.5.1 Description
Multi-tenant architecture supporting white-label learning center management.

#### 3.5.2 Functional Requirements
- **REQ-041**: System shall support multi-tenant architecture
- **REQ-042**: System shall provide white-label customization
- **REQ-043**: System shall support subdomain configuration
- **REQ-044**: System shall allow branding customization
- **REQ-045**: System shall support subscription management
- **REQ-046**: System shall provide learning center analytics
- **REQ-047**: System shall support user capacity management

#### 3.5.3 Customization Features
- Logo and favicon upload
- Color scheme customization
- Email template customization
- Domain/subdomain configuration
- Contact information management

### 3.6 Communication System

#### 3.6.1 Description
Automated communication system for notifications, reminders, and reporting.

#### 3.6.2 Functional Requirements
- **REQ-048**: System shall send automated email notifications
- **REQ-049**: System shall support customizable email templates
- **REQ-050**: System shall provide test assignment notifications
- **REQ-051**: System shall send completion and reminder emails
- **REQ-052**: System shall support bulk email functionality
- **REQ-053**: System shall maintain email delivery logs

---

## 4. External Interface Requirements

### 4.1 User Interfaces

#### 4.1.1 General UI Requirements
- **REQ-054**: Interface shall be responsive and mobile-friendly
- **REQ-055**: Interface shall follow Material Design principles
- **REQ-056**: Interface shall support accessibility standards (WCAG 2.1)
- **REQ-057**: Interface shall provide consistent navigation
- **REQ-058**: Interface shall support multiple languages

#### 4.1.2 Super Admin Interface
- Dashboard with system-wide metrics
- Learning center management
- User management and permissions
- System configuration settings
- Report generation and analytics

#### 4.1.3 Admin Interface
- Test creation and management
- Student and group management
- Question bank management
- Report generation
- Learning center settings

#### 4.1.4 Student Interface
- Dashboard with test assignments
- Profile management
- Test history and results
- Progress tracking
- Document upload

#### 4.1.5 Testing Interface
- Clean, distraction-free design
- Question navigation panel
- Timer display
- Progress indicators
- Audio controls
- Calculator integration

### 4.2 Hardware Interfaces
- **REQ-059**: System shall support standard computer hardware
- **REQ-060**: System shall support microphone for audio recording
- **REQ-061**: System shall support speakers for audio playback
- **REQ-062**: System shall support camera for proctoring (optional)

### 4.3 Software Interfaces

#### 4.3.1 Database Interface
- **REQ-063**: System shall interface with PostgreSQL database
- **REQ-064**: System shall support database connection pooling
- **REQ-065**: System shall implement database transaction management

#### 4.3.2 Email Service Interface
- **REQ-066**: System shall integrate with SMTP email services
- **REQ-067**: System shall support email template processing
- **REQ-068**: System shall handle email delivery failures

#### 4.3.3 File Storage Interface
- **REQ-069**: System shall support local file storage
- **REQ-070**: System shall support cloud storage integration
- **REQ-071**: System shall handle file upload/download operations

### 4.4 Communication Interfaces
- **REQ-072**: System shall use HTTPS for all communications
- **REQ-073**: System shall implement RESTful API architecture
- **REQ-074**: System shall support JSON data format
- **REQ-075**: System shall implement proper HTTP status codes

---

## 5. Non-Functional Requirements

### 5.1 Performance Requirements
- **REQ-076**: Page load time shall not exceed 3 seconds
- **REQ-077**: System shall support 1000+ concurrent users
- **REQ-078**: Database queries shall execute within 2 seconds
- **REQ-079**: File uploads shall support files up to 50MB
- **REQ-080**: API response time shall not exceed 1 second

### 5.2 Security Requirements
- **REQ-081**: System shall encrypt all data in transit using TLS 1.3
- **REQ-082**: System shall encrypt sensitive data at rest
- **REQ-083**: System shall implement JWT token-based authentication
- **REQ-084**: System shall enforce password complexity requirements
- **REQ-085**: System shall implement session timeout (30 minutes idle)
- **REQ-086**: System shall log all security-related events
- **REQ-087**: System shall prevent SQL injection attacks
- **REQ-088**: System shall prevent XSS attacks

### 5.3 Reliability Requirements
- **REQ-089**: System shall maintain 99.5% uptime
- **REQ-090**: System shall implement automatic backup procedures
- **REQ-091**: System shall recover from failures within 15 minutes
- **REQ-092**: System shall maintain data integrity during failures

### 5.4 Availability Requirements
- **REQ-093**: System shall be available 24/7 except scheduled maintenance
- **REQ-094**: Scheduled maintenance shall not exceed 4 hours per month
- **REQ-095**: System shall provide disaster recovery capabilities

### 5.5 Scalability Requirements
- **REQ-096**: System shall support horizontal scaling
- **REQ-097**: System shall handle increasing data volumes efficiently
- **REQ-098**: System shall support load balancing
- **REQ-099**: System shall support database clustering

### 5.6 Usability Requirements
- **REQ-100**: System shall provide intuitive user interface
- **REQ-101**: New users shall complete basic tasks within 10 minutes
- **REQ-102**: System shall provide comprehensive help documentation
- **REQ-103**: System shall support keyboard navigation
- **REQ-104**: System shall provide error messages and guidance

### 5.7 Compatibility Requirements
- **REQ-105**: System shall support Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
- **REQ-106**: System shall be compatible with iOS 13+ and Android 8+
- **REQ-107**: System shall support screen resolutions from 320px to 4K
- **REQ-108**: System shall maintain backward compatibility for 2 major versions

---

## 6. System Architecture

### 6.1 Architecture Overview
The system follows a modern three-tier architecture:

#### 6.1.1 Presentation Layer
- **Frontend Applications**: Multiple Angular SPAs
  - Super Admin Portal
  - Learning Center Admin Portal  
  - Student Portal
  - Student Test Interface
  - Parent CTA Portal

#### 6.1.2 Application Layer
- **Backend API**: Node.js/Express RESTful services
- **Authentication Service**: JWT-based authentication
- **Email Service**: Automated email notifications
- **File Management Service**: Document and media handling

#### 6.1.3 Data Layer
- **Primary Database**: PostgreSQL
- **File Storage**: Server-based file system
- **Logging System**: Application and security logs

### 6.2 Technology Stack

#### 6.2.1 Frontend Technologies
- **Framework**: Angular 16.2.12
- **UI Library**: Bootstrap 4.5.0, Angular Material
- **State Management**: RxJS, Services
- **Build Tools**: Angular CLI, Webpack
- **Testing**: Jasmine, Karma

#### 6.2.2 Backend Technologies
- **Runtime**: Node.js
- **Framework**: Express.js
- **Database ORM**: pg (node-postgres)
- **Authentication**: JWT
- **File Upload**: express-fileupload
- **Email**: Nodemailer

#### 6.2.3 Database
- **Database System**: PostgreSQL
- **Connection Pooling**: pg.Pool
- **Query Builder**: Native SQL with parameterized queries

### 6.3 Component Architecture

#### 6.3.1 Frontend Components
```
Angular Applications
├── Shared Components
│   ├── Header/Navigation
│   ├── Footer
│   ├── Authentication Guards
│   └── Common Services
├── Feature Modules
│   ├── User Management
│   ├── Test Management
│   ├── Question Management
│   ├── Reporting
│   └── Administration
└── Core Modules
    ├── HTTP Interceptors
    ├── Error Handling
    └── Configuration
```

#### 6.3.2 Backend Components
```
Node.js API
├── Controllers
│   ├── Authentication
│   ├── User Management
│   ├── Test Management
│   ├── Question Management
│   └── Reporting
├── Services
│   ├── Email Service
│   ├── File Service
│   └── Validation Service
├── Middleware
│   ├── Authentication
│   ├── Authorization
│   └── Error Handling
└── Routes
    ├── Admin Routes
    ├── Student Routes
    └── Super Admin Routes
```

---

## 7. Database Requirements

### 7.1 Database Schema Overview
The system uses PostgreSQL as the primary database with the following main entity groups:

#### 7.1.1 User Management
- `users` - System users (admins, students, parents)
- `user_tokens` - Authentication tokens
- `learning_center` - Learning center information
- `student` - Student-specific information

#### 7.1.2 Test Management
- `test` - Test definitions
- `test_questions` - Test-question relationships
- `test_settings` - Test configuration
- `ongoing_test` - Active test sessions

#### 7.1.3 Question Management
- `question` - Question content and metadata
- `question_options` - Multiple choice options
- `subjects` - Subject categorization
- `chapters` - Chapter organization
- `topics` - Topic classification

#### 7.1.4 Assessment Data
- `student_test_answers` - Student responses
- `test_results` - Completed test results
- `reports` - Generated report data

### 7.2 Data Relationships
- One-to-many: Learning Center → Users
- One-to-many: Test → Test Questions
- Many-to-many: Questions → Tests
- One-to-many: Student → Test Results

### 7.3 Database Performance Requirements
- **REQ-109**: Database shall support concurrent access for 1000+ users
- **REQ-110**: Query response time shall not exceed 2 seconds
- **REQ-111**: Database shall implement proper indexing strategy
- **REQ-112**: Database shall support transaction isolation

---

## 8. Security Requirements

### 8.1 Authentication Security
- **REQ-113**: Implement JWT-based stateless authentication
- **REQ-114**: Enforce strong password policies
- **REQ-115**: Support multi-factor authentication
- **REQ-116**: Implement account lockout after failed attempts

### 8.2 Authorization Security  
- **REQ-117**: Implement role-based access control (RBAC)
- **REQ-118**: Enforce principle of least privilege
- **REQ-119**: Validate user permissions for each request
- **REQ-120**: Implement resource-level authorization

### 8.3 Data Security
- **REQ-121**: Encrypt sensitive data at rest
- **REQ-122**: Use TLS 1.3 for data in transit
- **REQ-123**: Implement data anonymization for reports
- **REQ-124**: Secure file upload and storage

### 8.4 Application Security
- **REQ-125**: Prevent SQL injection attacks
- **REQ-126**: Prevent Cross-Site Scripting (XSS)
- **REQ-127**: Implement CSRF protection
- **REQ-128**: Validate and sanitize all inputs
- **REQ-129**: Implement secure session management

### 8.5 Infrastructure Security
- **REQ-130**: Use HTTPS for all communications
- **REQ-131**: Implement firewall protection
- **REQ-132**: Regular security updates and patches
- **REQ-133**: Implement intrusion detection
- **REQ-134**: Maintain security audit logs

---

## 9. Appendices

### 9.1 Glossary

**Adaptive Testing**: Testing methodology that adjusts question difficulty based on student responses.

**Learning Center**: An educational institution or organization using the platform.

**Multi-tenant**: Architecture allowing multiple learning centers to use the same system instance.

**Proctoring**: Supervision and monitoring of test-taking to ensure integrity.

**Reading Fluency**: Assessment of reading speed, accuracy, and comprehension.

**White-label**: Customizable branding allowing learning centers to use their own identity.

### 9.2 User Interface Mockups
[Note: UI mockups would be included in actual implementation]

### 9.3 Database Schema Diagrams
[Note: Detailed ERD diagrams would be included in actual implementation]

### 9.4 API Documentation
[Note: Complete API endpoint documentation would be included in actual implementation]

### 9.5 Deployment Architecture
[Note: Infrastructure and deployment diagrams would be included in actual implementation]

---

**Document Information:**
- **Version**: 1.0
- **Created**: August 5, 2025
- **Last Modified**: August 5, 2025
- **Status**: Draft
- **Classification**: Internal Use

---

*This document serves as the comprehensive Software Requirements Specification for the Zenudy/Gaptify Educational Testing Platform. It should be reviewed and updated regularly as the system evolves and new requirements are identified.*
