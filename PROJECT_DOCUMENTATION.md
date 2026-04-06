# JobHook - Job Portal Application
## Complete Project Documentation

---

## Table of Contents

1. [Title](#1-title)
2. [Introduction](#2-introduction)
3. [Problem Statement](#3-problem-statement)
4. [Objectives and Scope](#4-objectives-and-scope)
   - 4.1 [Objectives](#41-objectives)
   - 4.2 [Scope](#42-scope)
5. [Methodological Details](#5-methodological-details)
   - 5.1 [Designing and Developing JobHook](#51-designing-and-developing-jobhook)
   - 5.2 [Deploying JobHook](#52-deploying-jobhook)
6. [Modern Engineering Tools Used](#6-modern-engineering-tools-used)
7. [Technical Architecture](#7-technical-architecture)
8. [Features and Functionality](#8-features-and-functionality)
9. [Project Outcomes](#9-project-outcomes)

---

## 1. Title

**JobHook - A Comprehensive Job Portal Application**

**Project Version:** 0.1.0  
**Development Team:** Pravin Mundhe, Yash Patil, Tanmay Rahane, Piyush Shelke  
**Project Type:** Full-Stack Web Application  
**Domain:** Human Resources & Recruitment Technology

---

## 2. Introduction

JobHook is a modern, full-stack job portal application designed to bridge the gap between job seekers and employers. 
The platform provides a comprehensive solution for job posting, job searching, application management, and professional 
profile creation. Built using cutting-edge technologies, JobHook offers a seamless user experience with robust security 
features, real-time notifications, and an intuitive interface.

The application follows a microservices-inspired architecture with a clear separation between frontend and backend,
ensuring scalability, maintainability, and optimal performance. The system implements JWT-based authentication, 
MongoDB for flexible data storage, and a responsive React-based frontend with TypeScript for type safety.

**Key Highlights:**
- Dual user roles: Job Seekers and Employers
- Real-time job application tracking
- Advanced profile management with skills, certifications, and experience
- Email notification system for OTP verification
- Responsive design for mobile and desktop devices
- Secure authentication and authorization

---

## 3. Problem Statement

The traditional job search and recruitment process faces several challenges:

1. **Fragmented Information:** Job seekers struggle to find relevant opportunities across multiple platforms
2. **Manual Application Tracking:** Employers lack efficient tools to manage and track applications
3. **Profile Management:** Limited options for candidates to showcase their complete professional profile
4. **Security Concerns:** Inadequate authentication mechanisms leading to data breaches
5. **Poor User Experience:** Complex interfaces that hinder both job seekers and recruiters
6. **Lack of Real-time Updates:** Delayed notifications about application status and new opportunities
7. **Limited Filtering Options:** Difficulty in finding jobs matching specific skills and experience levels

**Solution:** JobHook addresses these challenges by providing a unified, secure, and user-friendly platform that 
streamlines the entire recruitment lifecycle from job posting to candidate selection.

---

## 4. Objectives and Scope

### 4.1 Objectives

The primary objectives of the JobHook project are:

1. **Develop a Secure Platform:** Implement JWT-based authentication and Spring Security for robust user authentication
2. **Create User-Friendly Interface:** Design an intuitive UI using modern frameworks for seamless navigation
3. **Enable Efficient Job Management:** Provide tools for employers to post, edit, and manage job listings
4. **Facilitate Job Discovery:** Implement advanced search and filtering mechanisms for job seekers
5. **Build Comprehensive Profiles:** Allow users to create detailed professional profiles with skills and experience
6. **Implement Real-time Notifications:** Develop a notification system for application updates and job alerts
7. **Ensure Scalability:** Design architecture that can handle growing user base and data volume
8. **Maintain Data Integrity:** Use MongoDB for flexible and reliable data storage
9. **Optimize Performance:** Implement efficient data retrieval and caching mechanisms
10. **Enable Email Verification:** Integrate email service for OTP-based account verification

### 4.2 Scope

**In Scope:**
- User registration and authentication (Job Seekers and Employers)
- Profile creation and management with skills, certifications, and work experience
- Job posting, editing, and deletion by employers
- Job search with filters (location, experience, job type, skills)
- Job application submission and tracking
- Application status management by employers
- Real-time notification system
- Email-based OTP verification
- Saved jobs functionality
- Responsive web design for all devices
- RESTful API development
- MongoDB database integration

**Out of Scope:**
- Video interview integration
- AI-based job recommendations
- Salary negotiation tools
- Mobile native applications (iOS/Android)
- Third-party job board integrations
- Advanced analytics and reporting dashboards
- Payment gateway integration for premium features

---

## 5. Methodological Details

### 5.1 Designing and Developing JobHook

**Phase 1: Requirements Analysis and Planning**
- Identified target users (job seekers and employers)
- Defined functional and non-functional requirements
- Created user stories and use cases
- Designed database schema and entity relationships
- Planned API endpoints and data flow

**Phase 2: Technology Stack Selection**
- **Frontend:** React 18.3.1 with TypeScript for type safety
- **Backend:** Spring Boot 3.3.2 with Java 17
- **Database:** MongoDB for flexible document storage
- **Authentication:** JWT (JSON Web Tokens) with Spring Security
- **UI Framework:** Mantine UI 7.13.0 for modern components
- **State Management:** Redux Toolkit 2.2.7
- **Styling:** Tailwind CSS 3.4.7 for utility-first styling
- **Email Service:** Spring Mail with Gmail SMTP

**Phase 3: Backend Development**

1. **Project Setup:**
   - Initialized Spring Boot project with Maven
   - Configured MongoDB connection
   - Set up Spring Security with JWT

2. **Entity Design:**
   - User Entity (id, name, email, password, accountType, profileId)
   - Profile Entity (skills, experience, certifications, picture)
   - Job Entity (title, company, description, requirements, applicants)
   - Notification Entity (message, status, timestamp)
   - OTP Entity (email, otp, expiration)
   - Applicant Entity (userId, applicationStatus, appliedDate)

3. **API Development:**
   - AuthAPI: Login, signup, OTP verification
   - UserAPI: User management operations
   - ProfileAPI: Profile CRUD operations
   - JobAPI: Job posting, searching, application management
   - NotificationAPI: Notification retrieval and management

4. **Security Implementation:**
   - JWT token generation and validation
   - Password encryption using BCrypt
   - Role-based access control (APPLICANT/RECRUITER)
   - Custom authentication filters

5. **Service Layer:**
   - Business logic implementation
   - Data validation
   - Exception handling
   - Email service integration

**Phase 4: Frontend Development**

1. **Project Setup:**
   - Created React app with TypeScript
   - Configured Tailwind CSS and Mantine UI
   - Set up Redux store for state management
   - Implemented routing with React Router v6

2. **Component Development:**
   - Header: Navigation and user menu
   - Footer: Links and copyright information
   - LandingPage: Hero section, features, testimonials
   - SignUpLogin: Authentication forms with OTP verification
   - Profile: User profile management
   - FindJobs: Job search and filtering
   - JobDesc: Detailed job information
   - ApplyJob: Application submission
   - PostJob: Job creation form (for employers)
   - PostedJob: Manage posted jobs and applications
   - FindTalent: Browse candidate profiles (for employers)
   - TalentProfile: View candidate details
   - JobHistory: Track application status
   - Notifications: View system notifications

3. **State Management:**
   - User authentication state (JWT token, user info)
   - Job listings and filters
   - Profile data
   - Notifications
   - Application status

4. **API Integration:**
   - Axios for HTTP requests
   - JWT token management
   - Error handling and toast notifications
   - Protected routes implementation

5. **UI/UX Design:**
   - Responsive design for mobile, tablet, and desktop
   - Dark theme with custom color palette
   - Smooth animations using AOS (Animate On Scroll)
   - Form validation and user feedback
   - Loading states and error handling

**Phase 5: Testing and Quality Assurance**
- Unit testing for backend services
- Integration testing for API endpoints
- Frontend component testing
- Cross-browser compatibility testing
- Responsive design testing
- Security vulnerability assessment
- Performance optimization

### 5.2 Deploying JobHook

**Development Environment:**
- Local development with hot reload
- MongoDB running on localhost:27017
- Backend on port 8080
- Frontend on port 3000

**Build Process:**
1. **Backend Build:**
   ```bash
   mvn clean install -DskipTests
   ```
   - Compiles Java code
   - Packages as executable JAR
   - Includes all dependencies

2. **Frontend Build:**
   ```bash
   npm run build
   ```
   - Transpiles TypeScript to JavaScript
   - Bundles with Webpack
   - Optimizes assets
   - Generates production-ready static files

**Deployment Options:**

1. **Docker Deployment:**
   - Backend Dockerfile included
   - Multi-stage build for optimization
   - Container orchestration with Docker Compose

2. **Cloud Deployment:**
   - Backend: AWS EC2, Heroku, or Azure App Service
   - Frontend: Netlify, Vercel, or AWS S3 + CloudFront
   - Database: MongoDB Atlas (cloud-hosted)

3. **Production Configuration:**
   - Environment variables for sensitive data
   - HTTPS/SSL certificate configuration
   - CORS policy setup
   - Database connection pooling
   - Logging and monitoring

---

## 6. Modern Engineering Tools Used

### 6.1 Backend Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| **Java** | 17 | Primary programming language |
| **Spring Boot** | 3.3.2 | Application framework |
| **Spring Security** | 6.x | Authentication and authorization |
| **Spring Data MongoDB** | 4.x | Database integration |
| **Spring Mail** | 3.x | Email service |
| **JWT (JJWT)** | 0.11.5 | Token-based authentication |
| **Lombok** | Latest | Reduce boilerplate code |
| **Maven** | 3.9.7 | Build and dependency management |
| **MongoDB** | 7.x | NoSQL database |

### 6.2 Frontend Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| **React** | 18.3.1 | UI library |
| **TypeScript** | 4.9.5 | Type-safe JavaScript |
| **Redux Toolkit** | 2.2.7 | State management |
| **React Router** | 6.25.1 | Client-side routing |
| **Mantine UI** | 7.13.0 | Component library |
| **Tailwind CSS** | 3.4.7 | Utility-first CSS framework |
| **Axios** | 1.7.5 | HTTP client |
| **TipTap** | 2.5.9 | Rich text editor |
| **AOS** | 2.3.4 | Scroll animations |
| **Day.js** | 1.11.12 | Date manipulation |
| **JWT Decode** | 4.0.0 | JWT token parsing |
| **React Hot Toast** | 2.4.1 | Toast notifications |
| **DOMPurify** | 3.1.6 | XSS protection |

### 6.3 Development Tools

| Tool | Purpose |
|------|---------|
| **Visual Studio Code** | Code editor |
| **Git** | Version control |
| **Postman** | API testing |
| **MongoDB Compass** | Database GUI |
| **Chrome DevTools** | Frontend debugging |
| **Maven Wrapper** | Build automation |
| **npm** | Package management |
| **ESLint** | Code linting |

### 6.4 Design and Collaboration Tools

| Tool | Purpose |
|------|---------|
| **Figma** | UI/UX design |
| **Tabler Icons** | Icon library |
| **Google Fonts (Poppins)** | Typography |
| **GitHub** | Code repository |

---

## 7. Technical Architecture

### 7.1 System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Client Layer                          │
│  (React + TypeScript + Redux + Tailwind + Mantine UI)       │
└─────────────────────┬───────────────────────────────────────┘
                      │ HTTP/HTTPS (REST API)
                      │ JWT Token Authentication
┌─────────────────────▼───────────────────────────────────────┐
│                   API Gateway Layer                          │
│              (Spring Boot Controllers)                       │
│  ┌──────────┬──────────┬──────────┬──────────┬──────────┐  │
│  │ AuthAPI  │ UserAPI  │ JobAPI   │ProfileAPI│NotifAPI  │  │
│  └──────────┴──────────┴──────────┴──────────┴──────────┘  │
└─────────────────────┬───────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────┐
│                   Security Layer                             │
│        (Spring Security + JWT Filter + BCrypt)              │
└─────────────────────┬───────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────┐
│                   Service Layer                              │
│     (Business Logic + Validation + Email Service)           │
└─────────────────────┬───────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────┐
│                  Repository Layer                            │
│            (Spring Data MongoDB)                             │
└─────────────────────┬───────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────┐
│                   Database Layer                             │
│                  (MongoDB 7.x)                               │
│  Collections: users, profiles, jobs, notifications, otps    │
└─────────────────────────────────────────────────────────────┘
```

### 7.2 Database Schema

**Collections:**

1. **users**
   - id (Long, Primary Key)
   - name (String)
   - email (String, Unique Index)
   - password (String, Encrypted)
   - accountType (Enum: APPLICANT/RECRUITER)
   - profileId (Long, Reference)

2. **profiles**
   - id (Long, Primary Key)
   - name (String)
   - email (String)
   - jobTitle (String)
   - company (String)
   - location (String)
   - about (String)
   - picture (Binary)
   - totalExp (Long)
   - skills (Array of Strings)
   - experiences (Array of Objects)
   - certifications (Array of Objects)
   - savedJobs (Array of Long)

3. **jobs**
   - id (Long, Primary Key)
   - jobTitle (String)
   - company (String)
   - applicants (Array of Applicant Objects)
   - about (String)
   - experience (String)
   - jobType (String)
   - location (String)
   - packageOffered (Long)
   - postTime (DateTime)
   - description (String)
   - skillsRequired (Array of Strings)
   - jobStatus (Enum: ACTIVE/CLOSED)
   - postedBy (Long, User Reference)

4. **notifications**
   - id (Long, Primary Key)
   - userId (Long)
   - message (String)
   - status (Enum: READ/UNREAD)
   - timestamp (DateTime)

5. **otps**
   - email (String, Primary Key)
   - otp (String)
   - expirationTime (DateTime)

### 7.3 API Endpoints

**Authentication APIs:**
- POST `/api/auth/signup` - User registration
- POST `/api/auth/login` - User login
- POST `/api/auth/verify-otp` - OTP verification
- POST `/api/auth/resend-otp` - Resend OTP

**User APIs:**
- GET `/api/users/{id}` - Get user details
- PUT `/api/users/{id}` - Update user
- DELETE `/api/users/{id}` - Delete user

**Profile APIs:**
- GET `/api/profiles/{id}` - Get profile
- POST `/api/profiles` - Create profile
- PUT `/api/profiles/{id}` - Update profile
- POST `/api/profiles/upload-picture` - Upload profile picture
- POST `/api/profiles/add-skill` - Add skill
- POST `/api/profiles/add-experience` - Add experience
- POST `/api/profiles/add-certification` - Add certification
- POST `/api/profiles/save-job` - Save job

**Job APIs:**
- GET `/api/jobs` - Get all jobs (with filters)
- GET `/api/jobs/{id}` - Get job details
- POST `/api/jobs` - Create job posting
- PUT `/api/jobs/{id}` - Update job
- DELETE `/api/jobs/{id}` - Delete job
- POST `/api/jobs/{id}/apply` - Apply for job
- GET `/api/jobs/posted-by/{userId}` - Get jobs posted by user
- GET `/api/jobs/applied-by/{userId}` - Get jobs applied by user
- PUT `/api/jobs/{jobId}/applicant/{applicantId}` - Update application status

**Notification APIs:**
- GET `/api/notifications/{userId}` - Get user notifications
- PUT `/api/notifications/{id}/read` - Mark as read
- DELETE `/api/notifications/{id}` - Delete notification

### 7.4 Security Implementation

**JWT Authentication Flow:**
1. User submits credentials (email/password)
2. Backend validates credentials
3. If valid, generates JWT token with user details
4. Token sent to frontend and stored in Redux
5. Frontend includes token in Authorization header for subsequent requests
6. Backend validates token on each protected endpoint
7. If token invalid/expired, returns 401 Unauthorized

**Password Security:**
- BCrypt hashing algorithm
- Salt rounds: 10
- Passwords never stored in plain text

**CORS Configuration:**
- Allowed origins configured
- Credentials support enabled
- Specific HTTP methods allowed

---

## 8. Features and Functionality

### 8.1 User Management

**For Job Seekers (Applicants):**
- Register with email and password
- Email verification via OTP
- Login with JWT authentication
- Create and manage professional profile
- Upload profile picture
- Add skills, certifications, and work experience
- Search and filter jobs
- Apply for jobs
- Track application status
- Save jobs for later
- Receive notifications
- View job history
