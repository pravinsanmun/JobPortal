<div align="center">
  <img src="frontend/public/PathHire%20logo.png" alt="PathHire Logo" width="180"/>
  
  # PathHire
  
  **Modern Job Portal Connecting Talent with Opportunity**
  
  [![Java](https://img.shields.io/badge/Java-17-orange.svg)](https://www.oracle.com/java/)
  [![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.3.2-brightgreen.svg)](https://spring.io/projects/spring-boot)
  [![React](https://img.shields.io/badge/React-18.3.1-blue.svg)](https://reactjs.org/)
  [![MongoDB](https://img.shields.io/badge/MongoDB-Latest-green.svg)](https://www.mongodb.com/)
  
  [Features](#-features) • [Quick Start](#-quick-start) • [Tech Stack](#-tech-stack) • [API](#-api-endpoints) • [Team](#-team)
</div>

---

## ✨ Features

- 🔐 **Secure Authentication** - JWT-based auth with role-based access
- 💼 **Job Management** - Post, browse, and apply for jobs
- 👤 **Rich Profiles** - Skills, experience, certifications, and resume
- 🔍 **Smart Search** - Filter by location, type, experience, salary
- � **Real-time Notifications** - Application status updates
- 📱 **Responsive Design** - Works on all devices

## 🚀 Quick Start

### Prerequisites
- Java 17+, Node.js 18+, MongoDB, Maven 3.9+

### Installation

**1. Clone the repository**
```bash
git clone https://github.com/yourusername/pathhire.git
cd pathhire
```

**2. Start MongoDB**
```bash
# Windows
net start MongoDB

# macOS
brew services start mongodb-community

# Or use MongoDB Atlas cloud database
```

**3. Run Backend**
```bash
cd backend
mvn spring-boot:run
# Runs on http://localhost:8080
```

**4. Run Frontend**
```bash
cd frontend
npm install
npm start
# Runs on http://localhost:3000
```

Open [http://localhost:3000](http://localhost:3000) in your browser 🎉

## 🛠️ Tech Stack

**Backend**
- Java 17 + Spring Boot 3.3.2
- Spring Security + JWT Authentication
- MongoDB + Spring Data
- Maven

**Frontend**
- React 18.3.1 + TypeScript 5.5.3
- Redux Toolkit (State Management)
- Mantine UI + Tailwind CSS
- React Router + Axios

## 📡 API Endpoints

### Authentication
```http
POST /auth/register       # Register new user
POST /auth/login         # User login
POST /auth/send-otp      # Send verification OTP
POST /auth/verify-otp    # Verify OTP
```

### Jobs
```http
GET  /jobs/all           # Get all jobs
POST /jobs/post-job      # Post job (Recruiter)
POST /jobs/apply         # Apply for job
PUT  /jobs/update-status # Update application status
```

### Profile
```http
GET  /profile/{userId}   # Get profile
POST /profile/create     # Create profile
PUT  /profile/update     # Update profile
```

## 📁 Project Structure

```
pathhire/
├── backend/                 # Spring Boot API
│   ├── api/                # REST Controllers
│   ├── service/            # Business Logic
│   ├── repository/         # Data Access
│   ├── entity/             # Database Models
│   └── jwt/                # Authentication
│
├── frontend/               # React App
│   ├── Components/         # UI Components
│   ├── Pages/              # Page Components
│   ├── Slices/             # Redux State
│   └── App.tsx             # Routes
│
└── README.md
```

## � Security

- BCrypt password hashing
- JWT token authentication (1-hour expiration)
- Role-based authorization (Job Seeker / Recruiter)
- Input validation & sanitization
- CORS configuration

## 🎯 Use Cases

**Job Seekers**
- Create profile with skills & experience
- Browse and filter job listings
- Apply with one click
- Track application status

**Recruiters**
- Post detailed job listings
- Review applications
- Manage candidates
- Update application status

## 🚢 Deployment

**Backend (Docker)**
```bash
docker build -t pathhire-backend ./backend
docker run -p 8080:8080 pathhire-backend
```

**Frontend (Vercel)**
```bash
cd frontend
npm run build
vercel --prod
```

## 📊 Database

**MongoDB Collections**
- `users` - Authentication data
- `profiles` - User profiles
- `jobs` - Job listings with embedded applicants
- `notifications` - User notifications
- `otps` - OTP verification

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 👥 Team

Built with ❤️ by:

| Developer | Role |
|-----------|------|
| **Pravin Mundhe** | Backend & Authentication |
| **Yash Patil** | Frontend & UI/UX |
| **Tanmay Rahane** | Backend & Database |
| **Piyush Shelke** | Frontend & Integration |

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📧 Support

For questions or support:
- 📧 Email: pathhire.support@example.com
- 🐛 Issues: [GitHub Issues](https://github.com/yourusername/pathhire/issues)
- 📖 Docs: [Full Documentation](PROJECT_DOCUMENTATION.md)

---

<div align="center">
  
**⭐ Star this repo if you find it helpful!**

Made with 💼 by Team PathHire | © 2026

</div>
