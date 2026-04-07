# PathHire - Job Portal Flow Diagram

## Mermaid Diagram Code

```mermaid
graph TD
    Start[PathHire - Job Portal Platform]
    
    Start --> HomePage[Home Page]
    Start --> LoginRegister[Login/Register Page]
    
    HomePage --> Dashboard[Dashboard]
    LoginRegister --> Dashboard
    
    Dashboard --> JobSeekers[Job Seekers]
    Dashboard --> Recruiters[Recruiters]
    
    %% Job Seekers Flow
    JobSeekers --> BrowseJobs[Browse Jobs]
    JobSeekers --> ProfileMgmt[Profile Management]
    
    BrowseJobs --> ApplyJobs[Apply for Jobs]
    ApplyJobs --> ViewApplications[View Applications]
    ViewApplications --> AppStatus[Application Status Tracking]
    
    BrowseJobs -.-> SavedJobs[Saved Jobs]
    ProfileMgmt -.-> Resume[Upload Resume/Portfolio]
    AppStatus -.-> Notifications1[Email Notifications]
    
    %% Recruiters Flow
    Recruiters --> PostJobs[Post Jobs]
    Recruiters --> CompanyProfile[Company Profile]
    
    PostJobs --> ManageJobs[Manage Job Listings]
    ManageJobs --> ViewApplicants[View Applications]
    ViewApplicants --> ApplicantScreen[Applicant Screening]
    
    ApplicantScreen --> Interview[Interview Scheduling]
    Interview -.-> CandidateMgmt[Candidate Management]
    CompanyProfile -.-> JobTemplates[Job Templates]
    ViewApplicants -.-> Notifications2[Email Notifications]
    
    %% Bottom Level - Convergence
    AppStatus --> NotificationCenter[Notifications Center]
    CandidateMgmt --> NotificationCenter
    
    SavedJobs --> Analytics[Analytics Dashboard]
    JobTemplates --> Analytics
    
    NotificationCenter --> RealTime[Real-time Updates]
    Analytics --> Reports[Reports & Insights]
    
    %% Styling
    classDef mainNode fill:#FDB022,stroke:#FF8C00,stroke-width:3px,color:#000
    classDef seekerNode fill:#0099FF,stroke:#00D9FF,stroke-width:2px,color:#fff
    classDef recruiterNode fill:#9D4EDD,stroke:#FF006E,stroke-width:2px,color:#fff
    classDef featureNode fill:#2D2D2D,stroke:#FFD700,stroke-width:2px,color:#fff
    classDef endNode fill:#FF8C00,stroke:#FFD700,stroke-width:3px,color:#fff
    
    class Start mainNode
    class HomePage,LoginRegister,Dashboard mainNode
    class JobSeekers,BrowseJobs,ApplyJobs,ViewApplications,AppStatus,SavedJobs,ProfileMgmt,Resume seekerNode
    class Recruiters,PostJobs,ManageJobs,ViewApplicants,ApplicantScreen,Interview,CandidateMgmt,CompanyProfile,JobTemplates recruiterNode
    class NotificationCenter,Analytics,RealTime,Reports,Notifications1,Notifications2 endNode
```

## How to Use This Diagram

### Option 1: GitHub/GitLab Markdown
Simply paste this markdown file into your GitHub or GitLab repository. Both platforms automatically render Mermaid diagrams.

### Option 2: Mermaid Live Editor
1. Go to https://mermaid.live/
2. Copy the code between the ```mermaid``` tags
3. Paste it into the editor
4. Export as PNG or SVG

### Option 3: VS Code
1. Install "Markdown Preview Mermaid Support" extension
2. Open this file in VS Code
3. Press `Ctrl+Shift+V` to preview
4. Right-click and export

### Option 4: Online Tools
- **Mermaid Chart**: https://www.mermaidchart.com/
- **Draw.io**: Import Mermaid code
- **Notion**: Supports Mermaid diagrams

## Diagram Legend

### Node Types:
- **Orange Gradient**: Main platform nodes (PathHire, Dashboard)
- **Blue Gradient**: Job Seeker features
- **Purple Gradient**: Recruiter features
- **Yellow/Orange**: Shared features and analytics

### Arrow Types:
- **Solid arrows (→)**: Primary user flow
- **Dashed arrows (-.->)**: Secondary/optional features

## Key Features Shown:

### For Job Seekers:
- Browse and search jobs
- Apply for positions
- Track application status
- Save favorite jobs
- Manage profile and resume
- Receive notifications

### For Recruiters:
- Post job openings
- Manage job listings
- Review applications
- Screen candidates
- Schedule interviews
- Track candidate pipeline
- Company profile management

### Shared Features:
- Real-time notifications
- Analytics and reporting
- Email notifications
- Dashboard insights
