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

**For Employers (Recruiters):**
- Register as recruiter account
- Email verification via OTP
- Login with JWT authentication
- Create company profile
- Post job openings
- Edit and delete job postings
- View all applicants for posted jobs
- Update application status (Pending/Accepted/Rejected)
- Search for talent/candidates
- View candidate profiles
- Receive notifications about new applications
- Manage posted jobs

### 8.2 Job Management

**Job Posting Features:**
- Job title and company name
- Job description with rich text editor
- Required skills (multiple selection)
- Experience level required
- Job type (Full-time, Part-time, Contract, Internship)
- Location
- Package/Salary offered
- About the company
- Automatic timestamp for posting date

**Job Search Features:**
- Search by job title or company
- Filter by location
- Filter by experience level
- Filter by job type
- Filter by required skills
- Sort by posting date
- Pagination support

### 8.3 Application Management

**Application Process:**
1. Job seeker views job details
2. Clicks "Apply Now"
3. System checks if already applied
4. Creates application with status "Pending"
5. Adds applicant to job's applicant list
6. Sends notification to employer
7. Job seeker can track status in Job History

**Application Status:**
- Pending: Initial state after application
- Accepted: Employer shortlisted the candidate
- Rejected: Application not selected

### 8.4 Profile Management

**Profile Components:**
- Personal Information (name, email, location)
- Current Job Title and Company
- About/Bio section
- Profile Picture
- Total Years of Experience
- Skills (add/remove dynamically)
- Work Experience (multiple entries with company, role, duration)
- Certifications (name, issuing organization, date)
- Saved Jobs list

### 8.5 Notification System

**Notification Types:**
- New job application received
- Application status updated
- New job matching skills posted
- Profile update confirmation
- OTP verification codes

**Notification Features:**
- Real-time updates
- Read/Unread status
- Delete notifications
- Notification count badge

### 8.6 UI/UX Features

**Design Elements:**
- Dark theme with custom color palette (Bright Sun & Mine Shaft)
- Responsive design (mobile-first approach)
- Smooth scroll animations (AOS library)
- Loading states and skeletons
- Toast notifications for user feedback
- Form validation with error messages
- Protected routes (redirect to login if not authenticated)
- 404 Not Found page
- Unauthorized access page
- Intuitive navigation with header and footer
- Search bars with autocomplete
- Dropdown filters
- Modal dialogs for confirmations
- Card-based layouts
- Carousel for testimonials

---

## 9. Project Outcomes

### 9.1 Technical Achievements

1. **Successful Full-Stack Implementation:**
   - Integrated React frontend with Spring Boot backend
   - Implemented RESTful API architecture
   - Achieved seamless data flow between layers

2. **Robust Security:**
   - JWT-based authentication system
   - Password encryption with BCrypt
   - Role-based access control
   - XSS protection with DOMPurify

3. **Scalable Architecture:**
   - Modular component structure
   - Separation of concerns (MVC pattern)
   - Reusable components and services
   - MongoDB for horizontal scaling

4. **Modern UI/UX:**
   - Responsive design across all devices
   - Smooth animations and transitions
   - Intuitive user interface
   - Accessibility considerations

5. **Email Integration:**
   - OTP-based verification system
   - Gmail SMTP configuration
   - Automated email notifications

### 9.2 Functional Achievements

1. **Complete Job Portal Functionality:**
   - User registration and authentication ✓
   - Profile management ✓
   - Job posting and management ✓
   - Job search and filtering ✓
   - Application submission and tracking ✓
   - Notification system ✓

2. **Performance Optimization:**
   - Lazy loading of components
   - Optimized database queries
   - Efficient state management
   - Build optimization for production

3. **Code Quality:**
   - TypeScript for type safety
   - Lombok for clean Java code
   - Consistent coding standards
   - Proper error handling
   - Comprehensive validation

### 9.3 Learning Outcomes

**Team Members Gained Expertise In:**

1. **Pravin Mundhe:**
   - Full-stack development
   - Spring Boot and Spring Security
   - MongoDB integration
   - JWT authentication implementation

2. **Yash Patil:**
   - React and TypeScript development
   - Redux state management
   - RESTful API integration
   - Responsive UI design

3. **Tanmay Rahane:**
   - Backend API development
   - Database schema design
   - Email service integration
   - Security best practices

4. **Piyush Shelke:**
   - Frontend component development
   - UI/UX implementation
   - Form validation
   - Animation and styling

### 9.4 Project Statistics

- **Total Lines of Code:** ~15,000+
- **Backend Files:** 54 Java files
- **Frontend Components:** 14+ major components
- **API Endpoints:** 25+ endpoints
- **Database Collections:** 5 collections
- **Dependencies:** 40+ libraries
- **Development Time:** 3-4 months
- **Team Size:** 4 developers

### 9.5 Screenshots and Demonstrations

**Key Application Screens:**

1. **Landing Page:**
   - Hero section with call-to-action
   - Featured job categories
   - How it works section
   - Testimonials from users
   - Footer with links

2. **Sign Up / Login Page:**
   - Email and password fields
   - Account type selection (Applicant/Recruiter)
   - OTP verification modal
   - Form validation messages

3. **Job Seeker Dashboard:**
   - Search bar with filters
   - Job listings in card format
   - Saved jobs section
   - Application history

4. **Job Details Page:**
   - Complete job information
   - Required skills badges
   - Company details
   - Apply button
   - Similar jobs section

5. **Profile Page:**
   - Profile picture upload
   - Personal information
   - Skills management
   - Experience timeline
   - Certifications list

6. **Employer Dashboard:**
   - Post new job button
   - Posted jobs list
   - Applicant management
   - Job statistics

7. **Application Management:**
   - List of applicants
   - Applicant profiles
   - Status update buttons
   - Filter by status

8. **Notifications Panel:**
   - Notification list
   - Read/Unread indicators
   - Delete options
   - Timestamp display

### 9.6 Future Enhancements

**Planned Features:**
1. AI-powered job recommendations based on profile
2. Video interview scheduling and integration
3. Resume parser and builder
4. Advanced analytics dashboard for employers
5. Mobile applications (iOS and Android)
6. Social media integration for profile import
7. Salary comparison and negotiation tools
8. Company reviews and ratings
9. Skill assessment tests
10. Chat system between recruiters and candidates
11. Calendar integration for interview scheduling
12. Multi-language support
13. Premium subscription features
14. Job alerts via email/SMS
15. Integration with LinkedIn and other job boards

### 9.7 Challenges Faced and Solutions

**Challenge 1: JWT Token Management**
- **Problem:** Token expiration handling and refresh mechanism
- **Solution:** Implemented token validation on each request and automatic logout on expiration

**Challenge 2: File Upload (Profile Pictures)**
- **Problem:** Storing and retrieving binary data in MongoDB
- **Solution:** Used Base64 encoding for storage and decoding for display

**Challenge 3: Real-time Notifications**
- **Problem:** Keeping notifications synchronized
- **Solution:** Implemented polling mechanism and state management with Redux

**Challenge 4: Complex State Management**
- **Problem:** Managing user, job, and application states across components
- **Solution:** Used Redux Toolkit with slices for organized state management

**Challenge 5: Responsive Design**
- **Problem:** Ensuring consistent UI across different screen sizes
- **Solution:** Used Tailwind CSS utility classes and Mantine's responsive props

**Challenge 6: Email Service Configuration**
- **Problem:** Gmail SMTP authentication and security
- **Solution:** Used app-specific passwords and proper SMTP configuration

---

## 10. Conclusion

JobHook successfully demonstrates a modern, full-stack job portal application built with industry-standard technologies. 
The project showcases the team's ability to:

- Design and implement complex web applications
- Work with modern frameworks and libraries
- Implement secure authentication and authorization
- Create responsive and user-friendly interfaces
- Integrate multiple technologies into a cohesive system
- Follow software engineering best practices
- Collaborate effectively as a team

The application provides real value to both job seekers and employers by streamlining the recruitment process, 
offering comprehensive profile management, and ensuring secure data handling. The modular architecture and clean 
code structure make it easy to maintain and extend with new features.

**Key Takeaways:**
- Full-stack development requires careful planning and coordination between frontend and backend
- Security should be a priority from the beginning, not an afterthought
- User experience is crucial for application adoption
- Modern tools and frameworks significantly accelerate development
- Proper state management is essential for complex applications
- Testing and validation prevent bugs and improve code quality

---

## 11. Team Contributions

**Pravin Mundhe:**
- Backend architecture design
- Spring Security and JWT implementation
- MongoDB schema design
- API development and testing

**Yash Patil:**
- Frontend architecture and routing
- Redux state management
- Component development
- API integration

**Tanmay Rahane:**
- Backend service layer implementation
- Email service integration
- Database operations
- Error handling and validation

**Piyush Shelke:**
- UI/UX design and implementation
- Responsive design
- Animation and styling
- Form components and validation

---

## 12. References and Resources

**Documentation:**
- Spring Boot Documentation: https://spring.io/projects/spring-boot
- React Documentation: https://react.dev/
- MongoDB Documentation: https://docs.mongodb.com/
- Mantine UI Documentation: https://mantine.dev/
- JWT Documentation: https://jwt.io/

**Libraries and Frameworks:**
- Redux Toolkit: https://redux-toolkit.js.org/
- Tailwind CSS: https://tailwindcss.com/
- Axios: https://axios-http.com/
- React Router: https://reactrouter.com/

**Tools:**
- Maven: https://maven.apache.org/
- npm: https://www.npmjs.com/
- Git: https://git-scm.com/

---

## 13. Appendix

### A. Installation Instructions

**Prerequisites:**
- Java 17 or higher
- Node.js 16 or higher
- MongoDB 7.x
- Maven 3.9+ (or use included wrapper)

**Backend Setup:**
```bash
cd backend
./mvnw clean install -DskipTests
./mvnw spring-boot:run
```

**Frontend Setup:**
```bash
cd frontend
npm install
npm start
```

**Database Setup:**
- Install MongoDB
- Start MongoDB service
- Database will be created automatically on first run

### B. Environment Variables

**Backend (application.properties):**
```properties
spring.data.mongodb.uri=mongodb://localhost:27017/jobportal
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=your-email@gmail.com
spring.mail.password=your-app-password
```

**Frontend:**
- API base URL configured in Axios instance
- Default: http://localhost:8080

### C. API Testing with Postman

**Sample Requests:**

1. **Register User:**
```
POST http://localhost:8080/api/auth/signup
Body: {
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123",
  "accountType": "APPLICANT"
}
```

2. **Login:**
```
POST http://localhost:8080/api/auth/login
Body: {
  "email": "john@example.com",
  "password": "password123"
}
```

3. **Get Jobs:**
```
GET http://localhost:8080/api/jobs
Headers: Authorization: Bearer <jwt-token>
```

### D. Project Structure

**Backend Structure:**
```
backend/
├── src/
│   ├── main/
│   │   ├── java/com/jobportal/
│   │   │   ├── api/              # REST Controllers
│   │   │   ├── dto/              # Data Transfer Objects
│   │   │   ├── entity/           # Database Entities
│   │   │   ├── exception/        # Custom Exceptions
│   │   │   ├── jwt/              # JWT Configuration
│   │   │   ├── repository/       # Database Repositories
│   │   │   ├── service/          # Business Logic
│   │   │   ├── utility/          # Helper Classes
│   │   │   ├── MyConfig.java     # CORS Configuration
│   │   │   └── SecurityConfig.java # Security Configuration
│   │   └── resources/
│   │       └── application.properties
│   └── test/                     # Test Files
├── pom.xml                       # Maven Configuration
└── Dockerfile                    # Docker Configuration
```

**Frontend Structure:**
```
frontend/
├── public/                       # Static Assets
├── src/
│   ├── Components/               # React Components
│   │   ├── ApplyJob/
│   │   ├── CompanyProfile/
│   │   ├── FindJobs/
│   │   ├── FindTalent/
│   │   ├── Footer/
│   │   ├── Header/
│   │   ├── JobDesc/
│   │   ├── JobHistory/
│   │   ├── LandingPage/
│   │   ├── PostedJob/
│   │   ├── PostJob/
│   │   ├── Profile/
│   │   ├── SignUpLogin/
│   │   └── TalentProfile/
│   ├── Data/                     # Static Data
│   ├── Pages/                    # Page Components
│   ├── Services/                 # API Services
│   ├── Slices/                   # Redux Slices
│   ├── Store.tsx                 # Redux Store
│   ├── App.tsx                   # Main App Component
│   └── index.tsx                 # Entry Point
├── package.json                  # npm Configuration
├── tailwind.config.js            # Tailwind Configuration
└── tsconfig.json                 # TypeScript Configuration
```

### E. Troubleshooting

**Common Issues:**

1. **MongoDB Connection Error:**
   - Ensure MongoDB service is running
   - Check connection string in application.properties
   - Verify port 27017 is not blocked

2. **CORS Error:**
   - Check CORS configuration in MyConfig.java
   - Ensure frontend URL is in allowed origins
   - Verify credentials support is enabled

3. **JWT Token Issues:**
   - Check token expiration time
   - Verify token is included in Authorization header
   - Ensure secret key is properly configured

4. **Email Service Not Working:**
   - Use app-specific password for Gmail
   - Enable "Less secure app access" or use OAuth2
   - Check SMTP settings

5. **Build Failures:**
   - Clear Maven cache: `mvn clean`
   - Delete node_modules and reinstall: `rm -rf node_modules && npm install`
   - Check Java and Node versions

---

## 14. Contact Information

**Project Team:**
- Pravin Mundhe - Backend Developer
- Yash Patil - Frontend Developer
- Tanmay Rahane - Backend Developer
- Piyush Shelke - Frontend Developer

**Project Repository:** [GitHub Link]
**Project Demo:** [Demo Link]
**Documentation:** This document

---

**Document Version:** 1.0  
**Last Updated:** April 6, 2026  
**Status:** Production Ready

---

© 2026 JobHook. All Rights Reserved.
