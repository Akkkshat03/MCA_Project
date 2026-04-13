# ⚡ Quick AI — Full Stack AI SaaS Platform

> A production-grade subscription-based AI platform built with the PERN stack, featuring 6 AI-powered tools, user authentication, subscription billing, and serverless deployment.

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express.js](https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)
![Cloudinary](https://img.shields.io/badge/Cloudinary-3448C5?style=for-the-badge&logo=cloudinary&logoColor=white)

---

## 🌐 Live Demo

🔗 **[View Live Application](https://quick-ai-psi-seven.vercel.app)**

---

## 📌 Project Overview

**Quick AI** is a full-stack SaaS application that provides users access to multiple AI-powered tools through a subscription model. Free users get 10 credits/month while Premium subscribers ($16/mo) unlock all features including image generation, object removal, and resume analysis.

Built as an **MCA Final Year Major Project** using the PERN stack.

---

## ✨ Features

### 🤖 AI Tools
| Tool | Description | Access |
|---|---|---|
| Article Generator | Long-form content via Google Gemini | Free + Premium |
| Blog Title Generator | SEO-friendly titles by keyword & category | Free + Premium |
| Image Generator | Text-to-image via ClipDrop API | Premium only |
| Background Remover | AI-powered background removal | Premium only |
| Object Remover | Remove specific objects from photos | Premium only |
| Resume Analyzer | PDF upload + AI feedback via Gemini | Premium only |

### 👤 Authentication & User Management
- Google OAuth and Email/Password via **Clerk**
- JWT Bearer Token validation on all protected routes
- User profile and account management built-in

### 💰 Subscription & Monetization
- **Free Plan** — 10 credits/month, basic tools
- **Premium Plan** — $8/month, unlimited credits + all features
- Billing managed through **Clerk's built-in billing system**
- Backend middleware enforces plan-based access control

### 🌍 Community & Dashboard
- Publish AI-generated images to a community feed
- Real-time like/unlike system with PostgreSQL array operations
- Dashboard showing complete creation history
- Polymorphic rendering based on content type

---

## 🛠️ Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| React + Vite | UI Framework |
| Tailwind CSS | Styling |
| React Router DOM | Client-side Routing |
| Axios | HTTP Requests |
| Clerk (clerk-react) | Authentication UI |
| React Hot Toast | Notifications |
| React Markdown | Markdown Rendering |
| Lucide React | Icons |

### Backend
| Technology | Purpose |
|---|---|
| Node.js + Express | Server & API |
| Clerk (Express) | Auth Middleware |
| Neon (PostgreSQL) | Serverless Database |
| Cloudinary | Image Storage & Processing |
| Multer | File Upload Handling |
| OpenAI SDK | Gemini API Integration |

### External APIs & Services
| Service | Purpose |
|---|---|
| Google Gemini AI | Text Generation |
| ClipDrop | Text-to-Image Generation |
| Cloudinary AI | Background & Object Removal |
| Clerk Billing | Subscription Management |
| Neon | Serverless PostgreSQL |

---

## 🏗️ Project Architecture

```
quick-ai/
├── client/                   # React Frontend (Vite)
│   ├── src/
│   │   ├── components/       # Navbar, Sidebar
│   │   ├── pages/            # All Application Pages
│   │   └── assets/           # Images, Icons, Dummy Data
│   └── .env                  # Frontend Environment Variables
│
└── server/                   # Node.js + Express Backend
    ├── configs/
    │   ├── db.js             # Neon PostgreSQL Connection
    │   ├── cloudinary.js     # Cloudinary Configuration
    │   └── multer.js         # File Upload Configuration
    ├── controllers/
    │   ├── aiController.js   # AI Tool Logic
    │   └── userController.js # User & Community Logic
    ├── middlewares/
    │   └── auth.js           # Clerk Auth + Plan Check
    ├── routes/
    │   ├── aiRoutes.js       # AI API Routes
    │   └── userRoutes.js     # User API Routes
    └── server.js             # Express App Entry Point
```

---

## 🔄 Request Flow

```
User Action (React)
      ↓
Axios Request + Bearer Token
      ↓
Express Server
      ↓
clerkMiddleware() → Validates Token
      ↓
requireAuth()    → Checks Authentication
      ↓
auth middleware  → Checks Plan + Credits
      ↓
Controller       → Calls External API
      ↓
Saves to Neon PostgreSQL
      ↓
Returns Response to Frontend
```

---

## 🔐 Middleware Logic — Usage Limit Enforcement

```
Incoming Request
      ↓
Is User Premium? → YES → Allow Request ✅
      ↓ NO
Check free_usage count in Clerk privateMetadata
      ↓
Count < 10  → Allow & Increment Count ✅
Count >= 10 → Reject 403: Limit Reached ❌
```

---

## 🗄️ Database Schema

```sql
CREATE TABLE creations (
  id          SERIAL PRIMARY KEY,
  user_id     TEXT NOT NULL,
  type        TEXT NOT NULL,   -- 'article' | 'image' | 'blog-title' | 'resume-review'
  prompt      TEXT,
  content     TEXT,            -- Markdown text or Cloudinary URL
  publish     BOOLEAN DEFAULT FALSE,
  likes       TEXT[] DEFAULT '{}',
  created_at  TIMESTAMP DEFAULT NOW()
);
```

---

## ⚙️ Environment Variables

### Client (`client/.env`)
```env
VITE_CLERK_PUBLISHABLE_KEY=pk_test_...
VITE_BASE_URL=http://localhost:3000
```

### Server (`server/.env`)
```env
CLERK_SECRET_KEY=sk_test_...
GEMINI_API_KEY=AIza...
CLIPDROP_API_KEY=...
CLOUDINARY_CLOUD_NAME=...
CLOUDINARY_API_KEY=...
CLOUDINARY_API_SECRET=...
DATABASE_URL=postgresql://...
PORT=3000
```

---

## 📦 Installation & Setup

### Prerequisites
- Node.js v18+
- Clerk account
- Google AI Studio account (Gemini API)
- Cloudinary account
- Neon PostgreSQL account

### 1. Clone the Repository
```bash
git clone https://github.com/Akkkshat03/MCA_Project.git
cd MCA_Project
```

### 2. Setup Frontend
```bash
cd client
npm install
```
Create `client/.env` with your environment variables.

### 3. Setup Backend
```bash
cd ../server
npm install
```
Create `server/.env` with your environment variables.

### 4. Run Development Servers

**Terminal 1 — Backend:**
```bash
cd server
nodemon server.js
```

**Terminal 2 — Frontend:**
```bash
cd client
npm run dev
```

### 5. Open in Browser
```
http://localhost:5173
```

---

## 🚀 Deployment

Both deployed on **Vercel**:

- **Frontend** → Vercel Static Build with `vercel.json` rewrites for React Router
- **Backend** → Vercel Serverless Functions via `vercel.json` rewrite rules

---

## 🎓 Key Learnings

- Architecting a production-ready SaaS with subscription gating
- Integrating multiple third-party APIs under one platform
- Handling ES Module vs CommonJS compatibility issues
- JWT-based authentication with Clerk
- Serverless PostgreSQL with Neon
- CORS configuration for cross-origin deployments
- Rate limiting and API quota management
- Resolving dual-package dependency conflicts in production

---

## 👨‍💻 Developer

**Akshat Kumar Gupta**
MCA Student | Full-Stack Developer
📧 [work.akshat3003@gmail.com](mailto:work.akshat3003@gmail.com)
🔗 [LinkedIn](https://www.linkedin.com/in/akshat-gupta)
🐙 [GitHub](https://github.com/Akkkshat03)

---
