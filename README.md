# Job Portal

A full-stack Job Portal web application where **Students** can browse and apply for jobs, and **Recruiters** can post jobs, manage companies, and review applicants — all in one seamless platform.

---

## Features

### Student
- Register & log in as a Student
- Browse all available jobs
- Search and filter jobs by category, location, or type
- View detailed job descriptions
- Apply for jobs with resume upload
- View all applied jobs in profile
- Update profile info, bio, skills, and profile photo

### Recruiter (Admin)
- Register & log in as a Recruiter
- Create and manage companies
- Post new job listings with full details
- View all applicants for each job posting
- Review applicant profiles and resumes

---

## Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| React 18 | UI library |
| Vite | Build tool & dev server |
| React Router DOM v7 | Client-side routing |
| Redux Toolkit | Global state management |
| Axios | HTTP requests |
| Tailwind CSS | Styling |
| Radix UI | Accessible UI components |
| Lucide React | Icons |
| Sonner | Toast notifications |
| Embla Carousel | Category carousel |

### Backend
| Technology | Purpose |
|---|---|
| Node.js + Express | Server & REST API |
| MongoDB + Mongoose | Database |
| JSON Web Token (JWT) | Authentication |
| bcryptjs | Password hashing |
| Cloudinary | File/image/resume storage |
| Multer | File upload handling |
| Cookie Parser | Cookie-based auth |
| CORS | Cross-origin requests |
| dotenv | Environment variables |

---

## Project Structure

```
Job-Portal/
├── backend/
│   ├── controllers/
│   │   ├── application.controller.js
│   │   ├── company.controller.js
│   │   ├── job.controller.js
│   │   └── user.controller.js
│   ├── middlewares/
│   │   ├── isAuthenticated.js
│   │   └── multer.js
│   ├── models/
│   │   ├── application.model.js
│   │   ├── company.model.js
│   │   ├── job.model.js
│   │   └── user.model.js
│   ├── routes/
│   │   ├── application.route.js
│   │   ├── company.route.js
│   │   ├── job.route.js
│   │   └── user.route.js
│   ├── utils/
│   │   └── db.js
│   ├── .env
│   ├── index.js
│   └── package.json
│
└── frontend/
    ├── src/
    │   ├── components/
    │   │   ├── admin/        # Recruiter dashboard pages
    │   │   ├── auth/         # Login & Signup
    │   │   ├── shared/       # Navbar
    │   │   └── ui/           # Reusable UI components
    │   ├── hooks/            # Custom React hooks
    │   ├── redux/            # Redux slices & store
    │   ├── utils/
    │   │   └── constant.js   # API endpoint constants
    │   ├── App.jsx
    │   └── main.jsx
    ├── index.html
    └── package.json
```

---

## API Endpoints

### User — `/api/v1/user`
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/register` | Register a new user |
| POST | `/login` | Login user |
| GET | `/logout` | Logout user |
| PUT | `/profile/update` | Update user profile |

### Job — `/api/v1/job`
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/post` | Post a new job (Recruiter) |
| GET | `/get` | Get all jobs (with filters) |
| GET | `/getadminjobs` | Get jobs posted by recruiter |
| GET | `/get/:id` | Get single job by ID |

### Company — `/api/v1/company`
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/register` | Register a new company |
| GET | `/get` | Get all companies of recruiter |
| GET | `/get/:id` | Get company by ID |
| PUT | `/update/:id` | Update company details |

### Application — `/api/v1/application`
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/apply/:id` | Apply for a job |
| GET | `/get` | Get all applied jobs (Student) |
| GET | `/:id/applicants` | Get all applicants for a job |
| PUT | `/status/:id/update` | Update application status |

---

## Database Models

### User
- `fullname`, `email`, `phoneNumber`, `password`
- `role` — `student` or `recruiter`
- `profile` — bio, skills, resume URL, profile photo

### Job
- `title`, `requirements`, `salary`, `experienceLevel`
- `location`, `jobType`, `position`
- References: `company` (Company), `created_by` (User), `applications` (Application[])

### Company
- Name, description, logo, location, website, etc.
- Created by a recruiter user

### Application
- References: `job`, `applicant` (User)
- `status` — pending / accepted / rejected

---

## Getting Started

### Prerequisites
- Node.js v18+
- MongoDB Atlas account
- Cloudinary account

---

### 1. Clone the Repository

```bash
git clone https://github.com/arpitatinni/Job-Portal.git
cd Job-Portal
```

---

### 2. Backend Setup

```bash
cd backend
npm install
```

Create a `.env` file in the `/backend` directory:

```env
PORT=8000
MONGO_URI=your_mongodb_atlas_connection_string
SECRET_KEY=your_jwt_secret_key
CLOUD_NAME=your_cloudinary_cloud_name
API_KEY=your_cloudinary_api_key
API_SECRET=your_cloudinary_api_secret
```

Start the backend server:

```bash
npm run dev
```

> Server runs at `http://localhost:8000`

---

### 3. Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

> Frontend runs at `http://localhost:5173`

---

## Authentication

- JWT tokens are stored in **HTTP-only cookies** for security.
- The `isAuthenticated` middleware protects all private routes.
- Two user roles: **Student** and **Recruiter**, each with separate dashboards and permissions.

---

## Frontend Routes

| Route | Component | Access |
|-------|-----------|--------|
| `/` | Home | Public |
| `/login` | Login | Public |
| `/signup` | Signup | Public |
| `/jobs` | Jobs | Public |
| `/browse` | Browse | Public |
| `/profile` | Profile | Student |
| `/description/:id` | JobDescription | Public |
| `/admin/companies` | Companies | Recruiter |
| `/admin/companies/create` | CompanyCreate | Recruiter |
| `/admin/companies/:id` | CompanySetup | Recruiter |
| `/admin/jobs` | AdminJobs | Recruiter |
| `/admin/jobs/create` | PostJob | Recruiter |
| `/admin/jobs/:id/applicants` | Applicants | Recruiter |

---

## State Management (Redux)

The app uses **Redux Toolkit** with four slices:

| Slice | Manages |
|-------|---------|
| `authSlice` | Logged-in user data |
| `jobSlice` | Job listings & filters |
| `companySlice` | Company data |
| `applicationSlice` | Application data |

---

## Custom Hooks

| Hook | Purpose |
|------|---------|
| `useGetAllJobs` | Fetch all jobs |
| `useGetAllAdminJobs` | Fetch recruiter's jobs |
| `useGetAllCompanies` | Fetch recruiter's companies |
| `useGetAppliedJobs` | Fetch student's applications |
| `useGetCompanyById` | Fetch single company |

---

## Author

**Arpita Ghosh**  
