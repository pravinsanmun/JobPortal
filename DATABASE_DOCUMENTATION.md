# JobHook Database Documentation
## Complete MongoDB Database Schema and Details

---

## Table of Contents

1. [Database Overview](#1-database-overview)
2. [Collections Summary](#2-collections-summary)
3. [Detailed Collection Schemas](#3-detailed-collection-schemas)
4. [Relationships and References](#4-relationships-and-references)
5. [Indexes and Performance](#5-indexes-and-performance)
6. [Data Types and Constraints](#6-data-types-and-constraints)
7. [Sample Documents](#7-sample-documents)
8. [Database Operations](#8-database-operations)
9. [Backup and Maintenance](#9-backup-and-maintenance)

---

## 1. Database Overview

**Database Name:** `jobportal`  
**Database Type:** MongoDB (NoSQL Document Database)  
**Version:** 7.x  
**Connection String:** `mongodb://localhost:27017/jobportal`  
**Character Set:** UTF-8  
**Auto Index Creation:** Enabled

### Database Architecture

JobHook uses MongoDB as its primary database, chosen for:
- **Flexible Schema:** Easy to add new fields without migrations
- **Document Model:** Natural fit for complex nested data (applicants, experiences, certifications)
- **Scalability:** Horizontal scaling through sharding
- **Performance:** Fast read/write operations
- **JSON-like Documents:** Easy integration with JavaScript/TypeScript frontend

### Collections Overview

The database consists of **5 main collections**:
1. **users** - User authentication and account information
2. **profiles** - Detailed user profiles with skills and experience
3. **jobs** - Job postings with embedded applicants
4. **notification** - System notifications for users
5. **otp** - One-time passwords for email verification

---

## 2. Collections Summary

| Collection | Purpose | Documents (Approx) | Primary Key | Indexes |
|------------|---------|-------------------|-------------|---------|
| **users** | Store user accounts | Variable | id (Long) | email (unique) |
| **profiles** | Store user profiles | Variable | id (Long) | None |
| **jobs** | Store job postings | Variable | id (Long) | None |
| **notification** | Store notifications | Variable | id (Long) | None |
| **otp** | Store OTP codes | Temporary | email (String) | TTL index |

### Collection Relationships

```
users (1) -----> (1) profiles
  |                      |
  | postedBy            | savedJobs
  |                      |
  v                      v
jobs (1) -----> (N) applicants (embedded)
  |
  | userId
  v
notification (N)
```

---

## 3. Detailed Collection Schemas

### 3.1 Collection: `users`

**Purpose:** Stores user authentication and account information

**Collection Name:** `users`  
**Document Count:** Dynamic (grows with registrations)  
**Average Document Size:** ~200 bytes

#### Schema Structure

```javascript
{
  _id: Long,                    // MongoDB ObjectId (mapped to Long)
  id: Long,                     // User ID (Primary Key)
  name: String,                 // User's full name
  email: String,                // User's email (UNIQUE)
  password: String,             // Encrypted password (BCrypt)
  accountType: String,          // Enum: APPLICANT, EMPLOYER, ADMIN
  profileId: Long               // Reference to profiles collection
}
```

#### Field Details

| Field | Type | Required | Unique | Default | Description |
|-------|------|----------|--------|---------|-------------|
| **_id** | ObjectId | Yes | Yes | Auto | MongoDB internal ID |
| **id** | Long | Yes | Yes | Auto | User identifier |
| **name** | String | Yes | No | - | Full name of user |
| **email** | String | Yes | Yes | - | Email address (login credential) |
| **password** | String | Yes | No | - | BCrypt hashed password |
| **accountType** | Enum | Yes | No | - | APPLICANT / EMPLOYER / ADMIN |
| **profileId** | Long | No | No | null | Reference to profile document |

#### Constraints and Validations

- **email:** Must be valid email format, unique across collection
- **password:** Minimum 8 characters (enforced at application layer)
- **accountType:** Must be one of: APPLICANT, EMPLOYER, ADMIN
- **name:** Cannot be empty or null

#### Indexes

```javascript
// Unique index on email for fast lookup and uniqueness
db.users.createIndex({ "email": 1 }, { unique: true })
```

---

### 3.2 Collection: `profiles`

**Purpose:** Stores detailed user profiles with skills, experience, and certifications

**Collection Name:** `profiles`  
**Document Count:** Dynamic (one per user)  
**Average Document Size:** ~2-5 KB (varies with picture size)

#### Schema Structure

```javascript
{
  _id: ObjectId,                          // MongoDB internal ID
  id: Long,                               // Profile ID (Primary Key)
  name: String,                           // User's full name
  email: String,                          // User's email
  jobTitle: String,                       // Current job title
  company: String,                        // Current company
  location: String,                       // City, State, Country
  about: String,                          // Bio/About section
  picture: Binary,                        // Profile picture (byte array)
  totalExp: Long,                         // Total years of experience
  skills: [String],                       // Array of skill names
  experiences: [Experience],              // Array of work experiences
  certifications: [Certification],        // Array of certifications
  savedJobs: [Long]                       // Array of saved job IDs
}
```

#### Field Details

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **_id** | ObjectId | Yes | MongoDB internal ID |
| **id** | Long | Yes | Profile identifier (matches user ID) |
| **name** | String | Yes | Full name |
| **email** | String | Yes | Email address |
| **jobTitle** | String | No | Current job title (e.g., "Software Engineer") |
| **company** | String | No | Current company name |
| **location** | String | No | Geographic location |
| **about** | String | No | Professional summary/bio |
| **picture** | Binary | No | Profile picture as byte array |
| **totalExp** | Long | No | Total years of experience |
| **skills** | Array[String] | No | List of skills (e.g., ["Java", "React"]) |
| **experiences** | Array[Object] | No | Work experience entries |
| **certifications** | Array[Object] | No | Certification entries |
| **savedJobs** | Array[Long] | No | IDs of jobs saved for later |

#### Nested Object: Experience

```javascript
{
  title: String,              // Job title
  company: String,            // Company name
  location: String,           // Work location
  startDate: DateTime,        // Start date
  endDate: DateTime,          // End date (null if currently working)
  working: Boolean,           // Currently working here
  description: String         // Job description/responsibilities
}
```

#### Nested Object: Certification

```javascript
{
  name: String,               // Certification name
  issuer: String,             // Issuing organization
  issueDate: DateTime,        // Date issued
  certificateId: String       // Certificate ID/number
}
```

#### Storage Considerations

- **picture:** Stored as byte array (Binary), converted to Base64 for frontend
- **skills:** Array can contain 0-50 skills typically
- **experiences:** Array can contain 0-20 work experiences
- **certifications:** Array can contain 0-30 certifications
- **savedJobs:** Array can contain 0-100 saved job IDs

---

### 3.3 Collection: `jobs`

**Purpose:** Stores job postings with embedded applicant information

**Collection Name:** `jobs`  
**Document Count:** Dynamic (grows with job postings)  
**Average Document Size:** ~3-10 KB (varies with applicants)

#### Schema Structure

```javascript
{
  _id: ObjectId,                    // MongoDB internal ID
  id: Long,                         // Job ID (Primary Key)
  jobTitle: String,                 // Job title
  company: String,                  // Company name
  applicants: [Applicant],          // Array of applicant objects (embedded)
  about: String,                    // About the company
  experience: String,               // Required experience (e.g., "2-5 years")
  jobType: String,                  // Full-time, Part-time, Contract, Internship
  location: String,                 // Job location
  packageOffered: Long,             // Salary package (annual)
  postTime: DateTime,               // When job was posted
  description: String,              // Detailed job description
  skillsRequired: [String],         // Required skills
  jobStatus: String,                // Enum: ACTIVE, CLOSED, DRAFT
  postedBy: Long                    // User ID of employer who posted
}
```

#### Field Details

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **_id** | ObjectId | Yes | MongoDB internal ID |
| **id** | Long | Yes | Job identifier |
| **jobTitle** | String | Yes | Job title (e.g., "Senior Java Developer") |
| **company** | String | Yes | Company name |
| **applicants** | Array[Object] | No | List of applicants (embedded documents) |
| **about** | String | No | Company description |
| **experience** | String | Yes | Required experience (e.g., "2-5 years") |
| **jobType** | String | Yes | Full-time / Part-time / Contract / Internship |
| **location** | String | Yes | Job location (city, state) |
| **packageOffered** | Long | No | Annual salary in currency units |
| **postTime** | DateTime | Yes | Timestamp when job was posted |
| **description** | String | Yes | Detailed job description (supports HTML) |
| **skillsRequired** | Array[String] | Yes | Required skills (e.g., ["Java", "Spring Boot"]) |
| **jobStatus** | Enum | Yes | ACTIVE / CLOSED / DRAFT |
| **postedBy** | Long | Yes | User ID of employer |

#### Nested Object: Applicant (Embedded)

```javascript
{
  applicantId: Long,              // User ID of applicant
  name: String,                   // Applicant's name
  email: String,                  // Applicant's email
  phone: Long,                    // Phone number
  website: String,                // Personal website/portfolio
  resume: Binary,                 // Resume file (byte array)
  coverLetter: String,            // Cover letter text
  timestamp: DateTime,            // When application was submitted
  applicationStatus: String,      // APPLIED, INTERVIEWING, OFFERED, REJECTED
  interviewTime: DateTime         // Scheduled interview time (if any)
}
```

#### Applicant Status Flow

```
APPLIED → INTERVIEWING → OFFERED
   ↓           ↓            ↓
REJECTED    REJECTED    REJECTED
```

#### Job Status Values

- **ACTIVE:** Job is currently accepting applications
- **CLOSED:** Job is no longer accepting applications
- **DRAFT:** Job is saved but not published yet

#### Application Status Values

- **APPLIED:** Initial application submitted
- **INTERVIEWING:** Candidate selected for interview
- **OFFERED:** Job offer extended
- **REJECTED:** Application rejected

---

### 3.4 Collection: `notification`

**Purpose:** Stores system notifications for users

**Collection Name:** `notification`  
**Document Count:** Dynamic (grows with user activity)  
**Average Document Size:** ~300 bytes

#### Schema Structure

```javascript
{
  _id: ObjectId,                // MongoDB internal ID
  id: Long,                     // Notification ID (Primary Key)
  userId: Long,                 // User ID who receives notification
  message: String,              // Notification message text
  action: String,               // Action type (e.g., "NEW_APPLICATION")
  route: String,                // Frontend route to navigate
  status: String,               // Enum: READ, UNREAD
  timestamp: DateTime           // When notification was created
}
```

#### Field Details

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **_id** | ObjectId | Yes | MongoDB internal ID |
| **id** | Long | Yes | Notification identifier |
| **userId** | Long | Yes | User who receives this notification |
| **message** | String | Yes | Notification message (e.g., "New application received") |
| **action** | String | No | Action type for frontend handling |
| **route** | String | No | Frontend route to navigate when clicked |
| **status** | Enum | Yes | READ / UNREAD |
| **timestamp** | DateTime | Yes | When notification was created |

#### Notification Types

1. **Application Received** (for employers)
   - Message: "New application received for [Job Title]"
   - Action: "VIEW_APPLICATION"
   - Route: "/posted-jobs"

2. **Application Status Updated** (for applicants)
   - Message: "Your application status has been updated to [Status]"
   - Action: "VIEW_STATUS"
   - Route: "/job-history"

3. **Job Posted Successfully** (for employers)
   - Message: "Your job posting '[Job Title]' is now live"
   - Action: "VIEW_JOB"
   - Route: "/posted-jobs"

4. **Profile Updated** (for all users)
   - Message: "Your profile has been updated successfully"
   - Action: "VIEW_PROFILE"
   - Route: "/profile"

#### Notification Status

- **UNREAD:** Notification not yet viewed by user (default)
- **READ:** Notification has been viewed by user

#### Cleanup Strategy

- Notifications older than 30 days can be archived or deleted
- Read notifications older than 7 days can be auto-deleted
- Implement periodic cleanup job

---

### 3.5 Collection: `otp`

**Purpose:** Stores one-time passwords for email verification

**Collection Name:** `otp`  
**Document Count:** Temporary (auto-deleted after expiration)  
**Average Document Size:** ~100 bytes  
**TTL (Time To Live):** 5 minutes

#### Schema Structure

```javascript
{
  _id: ObjectId,                // MongoDB internal ID
  email: String,                // Email address (Primary Key)
  otpCode: String,              // 6-digit OTP code
  creationTime: DateTime        // When OTP was created
}
```

#### Field Details

| Field | Type | Required | Unique | Description |
|-------|------|----------|--------|-------------|
| **_id** | ObjectId | Yes | Yes | MongoDB internal ID |
| **email** | String | Yes | Yes | Email address (used as primary key) |
| **otpCode** | String | Yes | No | 6-digit numeric OTP code |
| **creationTime** | DateTime | Yes | No | Timestamp when OTP was generated |

#### OTP Generation Rules

- **Length:** 6 digits
- **Format:** Numeric only (000000 - 999999)
- **Validity:** 5 minutes from creation time
- **Uniqueness:** One OTP per email at a time (overwrites previous)
- **Generation:** Random number generation

#### OTP Lifecycle

```
1. User requests OTP
   ↓
2. System generates 6-digit code
   ↓
3. OTP saved to database with creationTime
   ↓
4. Email sent to user
   ↓
5. User enters OTP within 5 minutes
   ↓
6. System validates OTP
   ↓
7. If valid: Delete OTP, proceed with action
   If invalid: Show error, allow retry
   If expired: Delete OTP, require new OTP
```

#### TTL Index

```javascript
// Auto-delete documents after 5 minutes
db.otp.createIndex(
  { "creationTime": 1 }, 
  { expireAfterSeconds: 300 }
)
```

#### Security Considerations

- OTP codes are not encrypted (short-lived, single-use)
- Rate limiting implemented at application layer
- Maximum 3 OTP requests per email per hour
- Failed verification attempts tracked

---

## 4. Relationships and References

### 4.1 Entity Relationship Diagram

```
┌─────────────────┐
│     users       │
│  id (PK)        │
│  email (UK)     │
│  password       │
│  accountType    │
│  profileId (FK) │────────┐
└─────────────────┘        │
         │                 │
         │ 1:1             │
         │                 ▼
         │         ┌─────────────────┐
         │         │    profiles     │
         │         │  id (PK)        │
         │         │  name           │
         │         │  skills[]       │
         │         │  experiences[]  │
         │         │  savedJobs[]    │
         │         └─────────────────┘
         │                 │
         │ 1:N             │ N:M (via savedJobs)
         │                 │
         ▼                 ▼
┌─────────────────┐       │
│      jobs       │◄──────┘
│  id (PK)        │
│  jobTitle       │
│  postedBy (FK)  │
│  applicants[]   │ (embedded)
│  skillsRequired │
└─────────────────┘
         │
         │ 1:N
         │
         ▼
┌─────────────────┐
│  notification   │
│  id (PK)        │
│  userId (FK)    │
│  message        │
└─────────────────┘

┌─────────────────┐
│      otp        │
│  email (PK)     │
│  otpCode        │
│  creationTime   │
└─────────────────┘
```

### 4.2 Relationship Details

#### users ↔ profiles (One-to-One)

- **Type:** One-to-One
- **Foreign Key:** users.profileId → profiles.id
- **Cascade:** Delete profile when user is deleted
- **Description:** Each user has exactly one profile

#### users ↔ jobs (One-to-Many)

- **Type:** One-to-Many
- **Foreign Key:** jobs.postedBy → users.id
- **Cascade:** Delete jobs when employer user is deleted
- **Description:** One employer can post multiple jobs

#### profiles ↔ jobs (Many-to-Many via savedJobs)

- **Type:** Many-to-Many
- **Implementation:** Array of job IDs in profiles.savedJobs
- **Description:** Users can save multiple jobs, jobs can be saved by multiple users

#### jobs ↔ applicants (One-to-Many Embedded)

- **Type:** One-to-Many (Embedded)
- **Implementation:** Applicants embedded in jobs.applicants array
- **Description:** One job can have multiple applicants

#### users ↔ notifications (One-to-Many)

- **Type:** One-to-Many
- **Foreign Key:** notification.userId → users.id
- **Cascade:** Delete notifications when user is deleted
- **Description:** One user can have multiple notifications

---

## 5. Indexes and Performance

### 5.1 Existing Indexes

#### Collection: users

```javascript
// Unique index on email for authentication
db.users.createIndex({ "email": 1 }, { unique: true })

// Index on id for fast lookups
db.users.createIndex({ "id": 1 })
```

**Purpose:** Fast user lookup by email during login, ensure email uniqueness

**Performance Impact:**
- Login query time: <5ms
- Registration uniqueness check: <5ms

#### Collection: otp

```javascript
// TTL index for automatic deletion
db.otp.createIndex(
  { "creationTime": 1 }, 
  { expireAfterSeconds: 300 }
)

// Unique index on email
db.otp.createIndex({ "email": 1 }, { unique: true })
```

**Purpose:** Auto-delete expired OTPs, ensure one OTP per email

**Performance Impact:**
- Automatic cleanup every 60 seconds
- No manual deletion required

### 5.2 Recommended Additional Indexes

#### Collection: jobs

```javascript
// Compound index for job search
db.jobs.createIndex({ 
  "jobStatus": 1, 
  "postTime": -1 
})

// Index on postedBy for employer dashboard
db.jobs.createIndex({ "postedBy": 1 })

// Text index for job search
db.jobs.createIndex({ 
  "jobTitle": "text", 
  "company": "text", 
  "description": "text" 
})

// Index on location for location-based search
db.jobs.createIndex({ "location": 1 })
```

#### Collection: notification

```javascript
// Compound index for user notifications
db.notification.createIndex({ 
  "userId": 1, 
  "status": 1, 
  "timestamp": -1 
})
```

#### Collection: profiles

```javascript
// Index on email for profile lookup
db.profiles.createIndex({ "email": 1 })

// Index on skills for talent search
db.profiles.createIndex({ "skills": 1 })
```

### 5.3 Query Performance Optimization

**Slow Query Identification:**
```javascript
// Enable profiling
db.setProfilingLevel(1, { slowms: 100 })

// View slow queries
db.system.profile.find().sort({ ts: -1 }).limit(10)
```

**Index Usage Analysis:**
```javascript
// Explain query execution
db.jobs.find({ jobStatus: "ACTIVE" }).explain("executionStats")
```

---

## 6. Data Types and Constraints

### 6.1 Data Type Mapping

| Java Type | MongoDB Type | Description | Example |
|-----------|--------------|-------------|---------|
| Long | NumberLong | 64-bit integer | 1234567890 |
| String | String | UTF-8 text | "John Doe" |
| byte[] | BinData | Binary data | Profile pictures, resumes |
| LocalDateTime | Date | ISO date/time | ISODate("2026-04-06T10:30:00Z") |
| Boolean | Boolean | true/false | true |
| List<String> | Array | Array of strings | ["Java", "React"] |
| Enum | String | Enum value as string | "APPLICANT" |

### 6.2 Field Constraints

#### users Collection

| Field | Constraint | Validation |
|-------|-----------|------------|
| email | Unique, Not Null | Valid email format |
| password | Not Null | Min 8 characters (BCrypt hashed) |
| name | Not Null | Min 2 characters |
| accountType | Not Null | Must be APPLICANT/EMPLOYER/ADMIN |

#### profiles Collection

| Field | Constraint | Validation |
|-------|-----------|------------|
| id | Not Null | Must match user ID |
| email | Not Null | Valid email format |
| totalExp | Optional | 0-50 years |
| skills | Optional | Max 50 items |
| picture | Optional | Max 5MB |

#### jobs Collection

| Field | Constraint | Validation |
|-------|-----------|------------|
| jobTitle | Not Null | Min 3 characters |
| company | Not Null | Min 2 characters |
| experience | Not Null | Valid format (e.g., "2-5 years") |
| jobType | Not Null | Full-time/Part-time/Contract/Internship |
| location | Not Null | Min 2 characters |
| description | Not Null | Min 50 characters |
| skillsRequired | Not Null | Min 1 skill |
| jobStatus | Not Null | ACTIVE/CLOSED/DRAFT |
| postedBy | Not Null | Valid user ID |

#### notification Collection

| Field | Constraint | Validation |
|-------|-----------|------------|
| userId | Not Null | Valid user ID |
| message | Not Null | Min 5 characters |
| status | Not Null | READ/UNREAD |
| timestamp | Not Null | Valid datetime |

#### otp Collection

| Field | Constraint | Validation |
|-------|-----------|------------|
| email | Unique, Not Null | Valid email format |
| otpCode | Not Null | 6 digits |
| creationTime | Not Null | Valid datetime |

### 6.3 Enum Values

#### AccountType
```java
APPLICANT   // Job seeker
EMPLOYER    // Company/Recruiter
ADMIN       // System administrator
```

#### JobStatus
```java
ACTIVE      // Accepting applications
CLOSED      // No longer accepting applications
DRAFT       // Saved but not published
```

#### ApplicationStatus
```java
APPLIED         // Initial application
INTERVIEWING    // Selected for interview
OFFERED         // Job offer extended
REJECTED        // Application rejected
```

#### NotificationStatus
```java
READ        // Notification viewed
UNREAD      // Notification not viewed
```

---

## 7. Sample Documents

### 7.1 Sample User Document

```json
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "id": 1001,
  "name": "Pravin Mundhe",
  "email": "pravin@example.com",
  "password": "$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy",
  "accountType": "APPLICANT",
  "profileId": 1001
}
```

### 7.2 Sample Profile Document

```json
{
  "_id": ObjectId("507f1f77bcf86cd799439012"),
  "id": 1001,
  "name": "Pravin Mundhe",
  "email": "pravin@example.com",
  "jobTitle": "Full Stack Developer",
  "company": "Tech Solutions Inc",
  "location": "Mumbai, Maharashtra, India",
  "about": "Passionate full-stack developer with 3 years of experience in building scalable web applications using React and Spring Boot.",
  "picture": BinData(0, "iVBORw0KGgoAAAANSUhEUgAA..."),
  "totalExp": 3,
  "skills": [
    "Java",
    "Spring Boot",
    "React",
    "TypeScript",
    "MongoDB",
    "REST API",
    "Git"
  ],
  "experiences": [
    {
      "title": "Full Stack Developer",
      "company": "Tech Solutions Inc",
      "location": "Mumbai, India",
      "startDate": ISODate("2023-01-15T00:00:00Z"),
      "endDate": null,
      "working": true,
      "description": "Developing and maintaining web applications using React and Spring Boot. Led team of 3 developers."
    },
    {
      "title": "Junior Developer",
      "company": "StartUp XYZ",
      "location": "Pune, India",
      "startDate": ISODate("2021-06-01T00:00:00Z"),
      "endDate": ISODate("2022-12-31T00:00:00Z"),
      "working": false,
      "description": "Worked on frontend development using React and contributed to backend APIs."
    }
  ],
  "certifications": [
    {
      "name": "AWS Certified Developer",
      "issuer": "Amazon Web Services",
      "issueDate": ISODate("2024-03-15T00:00:00Z"),
      "certificateId": "AWS-DEV-2024-12345"
    },
    {
      "name": "Oracle Certified Java Programmer",
      "issuer": "Oracle",
      "issueDate": ISODate("2023-08-20T00:00:00Z"),
      "certificateId": "OCP-JAVA-2023-67890"
    }
  ],
  "savedJobs": [2001, 2005, 2010]
}
```

### 7.3 Sample Job Document

```json
{
  "_id": ObjectId("507f1f77bcf86cd799439013"),
  "id": 2001,
  "jobTitle": "Senior Java Developer",
  "company": "Tech Giants Ltd",
  "applicants": [
    {
      "applicantId": 1001,
      "name": "Pravin Mundhe",
      "email": "pravin@example.com",
      "phone": 9876543210,
      "website": "https://pravin-portfolio.com",
      "resume": BinData(0, "JVBERi0xLjQKJeLjz9MK..."),
      "coverLetter": "I am excited to apply for the Senior Java Developer position...",
      "timestamp": ISODate("2026-04-05T14:30:00Z"),
      "applicationStatus": "APPLIED",
      "interviewTime": null
    },
    {
      "applicantId": 1002,
      "name": "Yash Patil",
      "email": "yash@example.com",
      "phone": 9876543211,
      "website": null,
      "resume": BinData(0, "JVBERi0xLjQKJeLjz9MK..."),
      "coverLetter": "With 5 years of Java experience, I believe I am a perfect fit...",
      "timestamp": ISODate("2026-04-04T10:15:00Z"),
      "applicationStatus": "INTERVIEWING",
      "interviewTime": ISODate("2026-04-10T15:00:00Z")
    }
  ],
  "about": "Tech Giants Ltd is a leading software company specializing in enterprise solutions.",
  "experience": "5-8 years",
  "jobType": "Full-time",
  "location": "Bangalore, Karnataka, India",
  "packageOffered": 1500000,
  "postTime": ISODate("2026-04-01T09:00:00Z"),
  "description": "<h2>Job Description</h2><p>We are looking for an experienced Java developer...</p>",
  "skillsRequired": [
    "Java",
    "Spring Boot",
    "Microservices",
    "Docker",
    "Kubernetes",
    "AWS"
  ],
  "jobStatus": "ACTIVE",
  "postedBy": 3001
}
```

### 7.4 Sample Notification Document

```json
{
  "_id": ObjectId("507f1f77bcf86cd799439014"),
  "id": 4001,
  "userId": 3001,
  "message": "New application received for Senior Java Developer position",
  "action": "VIEW_APPLICATION",
  "route": "/posted-jobs/2001",
  "status": "UNREAD",
  "timestamp": ISODate("2026-04-05T14:30:05Z")
}
```

### 7.5 Sample OTP Document

```json
{
  "_id": ObjectId("507f1f77bcf86cd799439015"),
  "email": "newuser@example.com",
  "otpCode": "123456",
  "creationTime": ISODate("2026-04-06T10:25:00Z")
}
```

---

## 8. Database Operations

### 8.1 Common Queries

#### User Operations

**Find user by email:**
```javascript
db.users.findOne({ email: "pravin@example.com" })
```

**Create new user:**
```javascript
db.users.insertOne({
  id: 1001,
  name: "Pravin Mundhe",
  email: "pravin@example.com",
  password: "$2a$10$...",
  accountType: "APPLICANT",
  profileId: null
})
```

**Update user password:**
```javascript
db.users.updateOne(
  { email: "pravin@example.com" },
  { $set: { password: "$2a$10$newHashedPassword" } }
)
```

**Delete user:**
```javascript
db.users.deleteOne({ id: 1001 })
```

#### Profile Operations

**Get profile by ID:**
```javascript
db.profiles.findOne({ id: 1001 })
```

**Add skill to profile:**
```javascript
db.profiles.updateOne(
  { id: 1001 },
  { $addToSet: { skills: "Docker" } }
)
```

**Remove skill from profile:**
```javascript
db.profiles.updateOne(
  { id: 1001 },
  { $pull: { skills: "Docker" } }
)
```

**Add work experience:**
```javascript
db.profiles.updateOne(
  { id: 1001 },
  { 
    $push: { 
      experiences: {
        title: "Senior Developer",
        company: "New Company",
        location: "Mumbai",
        startDate: ISODate("2026-01-01"),
        endDate: null,
        working: true,
        description: "Leading development team"
      }
    }
  }
)
```

**Save a job:**
```javascript
db.profiles.updateOne(
  { id: 1001 },
  { $addToSet: { savedJobs: 2001 } }
)
```

**Unsave a job:**
```javascript
db.profiles.updateOne(
  { id: 1001 },
  { $pull: { savedJobs: 2001 } }
)
```

#### Job Operations

**Get all active jobs:**
```javascript
db.jobs.find({ jobStatus: "ACTIVE" }).sort({ postTime: -1 })
```

**Search jobs by title:**
```javascript
db.jobs.find({ 
  jobTitle: { $regex: "Java", $options: "i" },
  jobStatus: "ACTIVE"
})
```

**Filter jobs by location and experience:**
```javascript
db.jobs.find({
  location: { $regex: "Mumbai", $options: "i" },
  experience: "2-5 years",
  jobStatus: "ACTIVE"
})
```

**Get jobs posted by employer:**
```javascript
db.jobs.find({ postedBy: 3001 }).sort({ postTime: -1 })
```

**Add applicant to job:**
```javascript
db.jobs.updateOne(
  { id: 2001 },
  {
    $push: {
      applicants: {
        applicantId: 1001,
        name: "Pravin Mundhe",
        email: "pravin@example.com",
        phone: 9876543210,
        website: null,
        resume: null,
        coverLetter: "I am interested...",
        timestamp: new Date(),
        applicationStatus: "APPLIED",
        interviewTime: null
      }
    }
  }
)
```

**Update application status:**
```javascript
db.jobs.updateOne(
  { 
    id: 2001,
    "applicants.applicantId": 1001
  },
  {
    $set: {
      "applicants.$.applicationStatus": "INTERVIEWING",
      "applicants.$.interviewTime": ISODate("2026-04-15T10:00:00Z")
    }
  }
)
```

**Close a job:**
```javascript
db.jobs.updateOne(
  { id: 2001 },
  { $set: { jobStatus: "CLOSED" } }
)
```

**Delete a job:**
```javascript
db.jobs.deleteOne({ id: 2001 })
```

#### Notification Operations

**Get user notifications:**
```javascript
db.notification.find({ userId: 1001 })
  .sort({ timestamp: -1 })
  .limit(20)
```

**Get unread notifications:**
```javascript
db.notification.find({ 
  userId: 1001,
  status: "UNREAD"
}).sort({ timestamp: -1 })
```

**Mark notification as read:**
```javascript
db.notification.updateOne(
  { id: 4001 },
  { $set: { status: "READ" } }
)
```

**Mark all notifications as read:**
```javascript
db.notification.updateMany(
  { userId: 1001, status: "UNREAD" },
  { $set: { status: "READ" } }
)
```

**Delete notification:**
```javascript
db.notification.deleteOne({ id: 4001 })
```

**Delete all read notifications:**
```javascript
db.notification.deleteMany({ 
  userId: 1001,
  status: "READ"
})
```

#### OTP Operations

**Create OTP:**
```javascript
db.otp.insertOne({
  email: "newuser@example.com",
  otpCode: "123456",
  creationTime: new Date()
})
```

**Verify OTP:**
```javascript
db.otp.findOne({
  email: "newuser@example.com",
  otpCode: "123456"
})
```

**Delete OTP after verification:**
```javascript
db.otp.deleteOne({ email: "newuser@example.com" })
```

### 8.2 Aggregation Queries

**Count applications per job:**
```javascript
db.jobs.aggregate([
  { $match: { postedBy: 3001 } },
  {
    $project: {
      jobTitle: 1,
      applicationCount: { $size: { $ifNull: ["$applicants", []] } }
    }
  }
])
```

**Get jobs with specific skills:**
```javascript
db.jobs.aggregate([
  { $match: { jobStatus: "ACTIVE" } },
  { $unwind: "$skillsRequired" },
  { $match: { skillsRequired: { $in: ["Java", "Spring Boot"] } } },
  { $group: { _id: "$_id", job: { $first: "$$ROOT" } } }
])
```

**Count applications by status:**
```javascript
db.jobs.aggregate([
  { $match: { id: 2001 } },
  { $unwind: "$applicants" },
  {
    $group: {
      _id: "$applicants.applicationStatus",
      count: { $sum: 1 }
    }
  }
])
```

**Get top skills from profiles:**
```javascript
db.profiles.aggregate([
  { $unwind: "$skills" },
  { $group: { _id: "$skills", count: { $sum: 1 } } },
  { $sort: { count: -1 } },
  { $limit: 10 }
])
```

---

## 9. Backup and Maintenance

### 9.1 Backup Strategies

#### Full Database Backup

**Using mongodump:**
```bash
# Backup entire database
mongodump --db jobportal --out /backup/jobportal_backup_2026-04-06

# Backup with compression
mongodump --db jobportal --gzip --out /backup/jobportal_backup_2026-04-06
```

**Using mongodump for specific collection:**
```bash
# Backup only users collection
mongodump --db jobportal --collection users --out /backup/users_backup
```

#### Restore Database

**Using mongorestore:**
```bash
# Restore entire database
mongorestore --db jobportal /backup/jobportal_backup_2026-04-06/jobportal

# Restore with drop existing
mongorestore --db jobportal --drop /backup/jobportal_backup_2026-04-06/jobportal
```

#### Automated Backup Script

**Windows PowerShell:**
```powershell
# backup-mongodb.ps1
$date = Get-Date -Format "yyyy-MM-dd_HH-mm"
$backupPath = "C:\backups\jobportal_$date"

mongodump --db jobportal --gzip --out $backupPath

# Delete backups older than 30 days
Get-ChildItem "C:\backups" -Directory | 
  Where-Object { $_.CreationTime -lt (Get-Date).AddDays(-30) } | 
  Remove-Item -Recurse -Force
```

**Schedule with Task Scheduler:**
```powershell
# Run daily at 2 AM
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-File C:\scripts\backup-mongodb.ps1"
$trigger = New-ScheduledTaskTrigger -Daily -At 2am
Register-ScheduledTask -TaskName "MongoDB Backup" -Action $action -Trigger $trigger
```

### 9.2 Database Maintenance

#### Check Database Statistics

```javascript
// Database stats
db.stats()

// Collection stats
db.users.stats()
db.profiles.stats()
db.jobs.stats()
db.notification.stats()
db.otp.stats()
```

#### Compact Collections

```javascript
// Compact to reclaim disk space
db.runCommand({ compact: "users" })
db.runCommand({ compact: "profiles" })
db.runCommand({ compact: "jobs" })
db.runCommand({ compact: "notification" })
```

#### Rebuild Indexes

```javascript
// Rebuild all indexes
db.users.reIndex()
db.profiles.reIndex()
db.jobs.reIndex()
db.notification.reIndex()
db.otp.reIndex()
```

#### Clean Up Old Data

**Delete old notifications:**
```javascript
// Delete notifications older than 30 days
db.notification.deleteMany({
  timestamp: { 
    $lt: new Date(Date.now() - 30 * 24 * 60 * 60 * 1000) 
  }
})
```

**Delete closed jobs older than 90 days:**
```javascript
db.jobs.deleteMany({
  jobStatus: "CLOSED",
  postTime: { 
    $lt: new Date(Date.now() - 90 * 24 * 60 * 60 * 1000) 
  }
})
```

### 9.3 Monitoring and Performance

#### Enable Profiling

```javascript
// Enable profiling for slow queries (>100ms)
db.setProfilingLevel(1, { slowms: 100 })

// View profiling data
db.system.profile.find().sort({ ts: -1 }).limit(10).pretty()
```

#### Monitor Current Operations

```javascript
// View current operations
db.currentOp()

// Kill long-running operation
db.killOp(operationId)
```

#### Check Index Usage

```javascript
// Get index statistics
db.users.aggregate([{ $indexStats: {} }])
db.jobs.aggregate([{ $indexStats: {} }])
```

#### Database Size Monitoring

```javascript
// Check database size
db.stats().dataSize
db.stats().storageSize
db.stats().indexSize

// Check collection sizes
db.users.stats().size
db.profiles.stats().size
db.jobs.stats().size
```

### 9.4 Security Best Practices

#### Enable Authentication

```javascript
// Create admin user
use admin
db.createUser({
  user: "admin",
  pwd: "securePassword123",
  roles: ["root"]
})

// Create application user
use jobportal
db.createUser({
  user: "jobportal_app",
  pwd: "appPassword123",
  roles: [
    { role: "readWrite", db: "jobportal" }
  ]
})
```

#### Connection String with Authentication

```
mongodb://jobportal_app:appPassword123@localhost:27017/jobportal
```

#### Enable SSL/TLS

```bash
# Start MongoDB with SSL
mongod --sslMode requireSSL --sslPEMKeyFile /path/to/mongodb.pem
```

#### Regular Security Audits

- Review user permissions quarterly
- Update MongoDB to latest stable version
- Monitor failed authentication attempts
- Implement IP whitelisting
- Use strong passwords (min 16 characters)
- Enable audit logging

### 9.5 Disaster Recovery Plan

#### Recovery Time Objective (RTO)

- **Target:** 4 hours
- **Steps:**
  1. Identify failure (15 minutes)
  2. Provision new server (1 hour)
  3. Restore from backup (2 hours)
  4. Verify data integrity (30 minutes)
  5. Resume operations (30 minutes)

#### Recovery Point Objective (RPO)

- **Target:** 24 hours (daily backups)
- **Acceptable Data Loss:** Last 24 hours of data

#### Backup Schedule

- **Daily:** Full database backup at 2 AM
- **Weekly:** Archive backup (kept for 3 months)
- **Monthly:** Long-term archive (kept for 1 year)

#### Testing Recovery

- **Frequency:** Quarterly
- **Process:**
  1. Restore backup to test environment
  2. Verify data integrity
  3. Test application connectivity
  4. Document any issues
  5. Update recovery procedures

---

## 10. Database Connection Configuration

### 10.1 Spring Boot Configuration

**application.properties:**
```properties
# MongoDB Configuration
spring.data.mongodb.uri=mongodb://localhost:27017/jobportal
spring.data.mongodb.auto-index-creation=true

# Connection Pool Settings
spring.data.mongodb.max-connection-idle-time=60000
spring.data.mongodb.max-connection-life-time=120000
spring.data.mongodb.min-connections-per-host=10
spring.data.mongodb.max-connections-per-host=100

# Timeout Settings
spring.data.mongodb.connect-timeout=10000
spring.data.mongodb.socket-timeout=60000
spring.data.mongodb.server-selection-timeout=30000
```

### 10.2 Connection String Options

**Basic Connection:**
```
mongodb://localhost:27017/jobportal
```

**With Authentication:**
```
mongodb://username:password@localhost:27017/jobportal?authSource=admin
```

**With Replica Set:**
```
mongodb://host1:27017,host2:27017,host3:27017/jobportal?replicaSet=rs0
```

**With SSL:**
```
mongodb://localhost:27017/jobportal?ssl=true
```

### 10.3 MongoDB Compass Connection

**Connection String:**
```
mongodb://localhost:27017
```

**Steps:**
1. Open MongoDB Compass
2. Enter connection string
3. Click "Connect"
4. Select "jobportal" database
5. Browse collections

---

## 11. Troubleshooting

### 11.1 Common Issues

#### Issue: Connection Refused

**Symptoms:**
- Application cannot connect to MongoDB
- Error: "Connection refused"

**Solutions:**
1. Check if MongoDB service is running:
   ```powershell
   Get-Service -Name MongoDB
   ```
2. Start MongoDB service:
   ```powershell
   Start-Service MongoDB
   ```
3. Verify port 27017 is not blocked
4. Check firewall settings

#### Issue: Duplicate Key Error

**Symptoms:**
- Error: "E11000 duplicate key error"
- Cannot insert document

**Solutions:**
1. Check unique indexes:
   ```javascript
   db.users.getIndexes()
   ```
2. Verify email uniqueness before insert
3. Handle duplicate key exception in code

#### Issue: Slow Queries

**Symptoms:**
- Application response time > 1 second
- Database CPU usage high

**Solutions:**
1. Enable profiling and identify slow queries
2. Add appropriate indexes
3. Optimize query patterns
4. Consider pagination for large result sets

#### Issue: Disk Space Full

**Symptoms:**
- Cannot insert new documents
- Error: "No space left on device"

**Solutions:**
1. Check disk usage:
   ```bash
   df -h
   ```
2. Compact collections to reclaim space
3. Delete old data (notifications, closed jobs)
4. Increase disk space

#### Issue: Index Build Failed

**Symptoms:**
- Index creation fails
- Error during index build

**Solutions:**
1. Check for duplicate values in unique index fields
2. Ensure sufficient disk space
3. Build index in background:
   ```javascript
   db.collection.createIndex({ field: 1 }, { background: true })
   ```

---

## 12. Summary

### Database Statistics

- **Total Collections:** 5
- **Total Indexes:** 3+ (with recommended: 10+)
- **Estimated Storage:** 100MB - 10GB (depends on usage)
- **Average Document Size:**
  - users: ~200 bytes
  - profiles: ~2-5 KB
  - jobs: ~3-10 KB
  - notification: ~300 bytes
  - otp: ~100 bytes

### Key Features

✅ Flexible schema design  
✅ Embedded documents for performance  
✅ Automatic index creation  
✅ TTL indexes for auto-cleanup  
✅ Support for binary data (images, resumes)  
✅ Efficient querying with indexes  
✅ Scalable architecture  
✅ ACID transactions support  

### Best Practices Implemented

✅ Unique constraints on email fields  
✅ Password encryption (BCrypt)  
✅ Embedded applicants for performance  
✅ TTL index for OTP expiration  
✅ Proper data type usage  
✅ Normalized where appropriate  
✅ Denormalized for performance  

---

**Document Version:** 1.0  
**Last Updated:** April 6, 2026  
**Maintained By:** JobHook Development Team

---

© 2026 JobHook. All Rights Reserved.
