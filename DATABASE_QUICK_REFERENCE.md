# JobHook Database - Quick Reference Guide

## Database Connection

**Connection String:** `mongodb://localhost:27017/jobportal`

**Check MongoDB Status:**
```powershell
Get-Service -Name MongoDB
```

---

## Collections Overview

| Collection | Purpose | Key Fields |
|------------|---------|------------|
| **users** | User accounts | id, email (unique), password, accountType |
| **profiles** | User profiles | id, skills[], experiences[], certifications[] |
| **jobs** | Job postings | id, jobTitle, applicants[], postedBy |
| **notification** | Notifications | id, userId, message, status |
| **otp** | OTP codes | email (unique), otpCode, creationTime (TTL 5min) |

---

## Quick Queries

### Users
```javascript
// Find user by email
db.users.findOne({ email: "user@example.com" })

// Count total users
db.users.countDocuments()

// Find all employers
db.users.find({ accountType: "EMPLOYER" })
```

### Profiles
```javascript
// Get profile
db.profiles.findOne({ id: 1001 })

// Add skill
db.profiles.updateOne({ id: 1001 }, { $addToSet: { skills: "Docker" } })

// Remove skill
db.profiles.updateOne({ id: 1001 }, { $pull: { skills: "Docker" } })
```

### Jobs
```javascript
// Get active jobs
db.jobs.find({ jobStatus: "ACTIVE" }).sort({ postTime: -1 })

// Search by title
db.jobs.find({ jobTitle: { $regex: "Java", $options: "i" } })

// Count applications
db.jobs.aggregate([
  { $project: { jobTitle: 1, count: { $size: "$applicants" } } }
])
```

### Notifications
```javascript
// Get unread notifications
db.notification.find({ userId: 1001, status: "UNREAD" })

// Mark as read
db.notification.updateOne({ id: 4001 }, { $set: { status: "READ" } })
```

---

## Enum Values

**AccountType:** APPLICANT, EMPLOYER, ADMIN  
**JobStatus:** ACTIVE, CLOSED, DRAFT  
**ApplicationStatus:** APPLIED, INTERVIEWING, OFFERED, REJECTED  
**NotificationStatus:** READ, UNREAD

---

## Backup Commands

```bash
# Backup
mongodump --db jobportal --out /backup/jobportal_backup

# Restore
mongorestore --db jobportal /backup/jobportal_backup/jobportal
```

---

## Useful Commands

```javascript
// Database stats
db.stats()

// Collection stats
db.users.stats()

// View indexes
db.users.getIndexes()

// Rebuild indexes
db.users.reIndex()

// Enable profiling
db.setProfilingLevel(1, { slowms: 100 })
```

---

For complete documentation, see **DATABASE_DOCUMENTATION.md**
