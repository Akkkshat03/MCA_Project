⚡ Quick AI — Full Stack AI SaaS Application

A subscription-based AI platform built with the PERN stack, featuring multiple AI-powered tools, user authentication, and monetization.

📌 Project Overview
Quick AI is a full-stack SaaS application that provides users with access to multiple AI-powered tools through a subscription model. Free users get limited credits, while Premium subscribers unlock all features including image generation, object removal, and resume analysis.
This project was developed as an MCA Final Year Major Project.

🚀 Live Demo
🌐 View Live Application ← Add your Vercel URL here

🛠️ Tech Stack
Frontend
TechnologyPurposeReact + ViteUI FrameworkTailwind CSSStylingReact Router DOMClient-side RoutingAxiosHTTP RequestsClerk (clerk-react)Authentication UIReact Hot ToastNotificationsReact MarkdownMarkdown RenderingLucide ReactIcons
Backend
TechnologyPurposeNode.js + ExpressServer & APIClerk (Express)Auth MiddlewareNeon (PostgreSQL)Serverless DatabaseCloudinaryImage Storage & ProcessingMulterFile Upload HandlingOpenAI SDKGemini API Integrationpdf-parsePDF Text ExtractionCORSCross-Origin Resource Sharing
External APIs & Services
ServicePurposeGoogle GeminiText Generation (Articles, Blog Titles, Resume Review)ClipDropText-to-Image GenerationCloudinary AIBackground & Object RemovalClerk BillingSubscription Management

✨ Features
🤖 AI Tools

Article Generator — Generate long-form articles on any topic using Google Gemini
Blog Title Generator — Create SEO-friendly blog titles by keyword and category
Image Generator — Text-to-image generation using ClipDrop API (Premium only)
Background Remover — AI-powered background removal from images (Premium only)
Object Remover — Remove specific objects from photos using Cloudinary AI (Premium only)
Resume Analyzer — Upload a PDF resume and get detailed AI feedback (Premium only)

👤 Authentication & User Management

Google OAuth and Email/Password sign-up via Clerk
Protected routes with JWT Bearer Token validation
User profile and account management

💰 Subscription & Monetization

Free Plan — 10 credits/month with access to basic tools
Premium Plan — $16/month with unlimited credits and all features
Billing managed through Clerk's built-in billing system
Backend middleware enforces plan-based access control

🌍 Community

Users can publish generated images to a community feed
Like/Unlike system for community creations
Real-time like count updates

📊 Dashboard

View complete creation history
Polymorphic rendering based on content type (article, image, resume)


🏗️ Architecture
quick-ai/
├── client/                   # React Frontend (Vite)
│   ├── src/
│   │   ├── components/       # Reusable UI Components
│   │   │   ├── Navbar.jsx
│   │   │   └── Sidebar.jsx
│   │   ├── pages/            # Application Pages
│   │   │   ├── Home.jsx
│   │   │   ├── Dashboard.jsx
│   │   │   ├── WriteArticle.jsx
│   │   │   ├── BlogTitles.jsx
│   │   │   ├── GenerateImages.jsx
│   │   │   ├── RemoveBackground.jsx
│   │   │   ├── RemoveObject.jsx
│   │   │   ├── ReviewResume.jsx
│   │   │   ├── Community.jsx
│   │   │   └── Layout.jsx
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

🔄 Request Flow
User Action (React)
      ↓
Axios Request + Bearer Token
      ↓
Express Server
      ↓
clerkMiddleware() → Validates Token
      ↓
requireAuth() → Checks Authentication
      ↓
auth middleware → Checks Plan + Credits
      ↓
Controller → Calls External API (Gemini/ClipDrop/Cloudinary)
      ↓
Saves to Neon PostgreSQL
      ↓
Returns Response to Frontend

🗄️ Database Schema
sqlCREATE TABLE creations (
  id          SERIAL PRIMARY KEY,
  user_id     TEXT NOT NULL,          -- Clerk User ID
  type        TEXT NOT NULL,          -- 'article' | 'image' | 'blog-title' | 'resume-review'
  prompt      TEXT,
  content     TEXT,                   -- Markdown text or Cloudinary URL
  publish     BOOLEAN DEFAULT FALSE,
  likes       TEXT[] DEFAULT '{}',    -- Array of User IDs
  created_at  TIMESTAMP DEFAULT NOW()
);

⚙️ Environment Variables
Client (client/.env)
envVITE_CLERK_PUBLISHABLE_KEY=pk_test_...
VITE_BASE_URL=http://localhost:3000
Server (server/.env)
envCLERK_SECRET_KEY=sk_test_...
GEMINI_API_KEY=AIza...
CLIPDROP_API_KEY=...
CLOUDINARY_CLOUD_NAME=...
CLOUDINARY_API_KEY=...
CLOUDINARY_API_SECRET=...
DATABASE_URL=postgresql://...
PORT=3000

📦 Installation & Setup
Prerequisites

Node.js v18+
npm or yarn
Clerk account
Google AI Studio account (Gemini API)
Cloudinary account
Neon PostgreSQL account

1. Clone the Repository
bashgit clone https://github.com/yourusername/quick-ai.git
cd quick-ai
2. Setup Frontend
bashcd client
npm install
Create client/.env and add your environment variables.
3. Setup Backend
bashcd ../server
npm install
Create server/.env and add your environment variables.
4. Run Development Servers
Terminal 1 — Start Backend:
bashcd server
nodemon server.js
Terminal 2 — Start Frontend:
bashcd client
npm run dev
5. Open in Browser
http://localhost:5173

🚀 Deployment
Both frontend and backend are deployed on Vercel:

Frontend → Vercel Static Build (rewrites all paths to index.html)
Backend → Vercel Serverless Functions (via vercel.json rewrite rules)


🔐 Middleware Logic
The auth middleware enforces usage limits:
Incoming Request
      ↓
Is User Premium? → YES → Allow Request
      ↓ NO
Check free_usage count
      ↓
Count < 10  → Allow & Increment Count
Count >= 10 → Reject (403: Limit Reached)

🎓 Key Learnings

Building a production-ready SaaS with subscription gating
Integrating multiple third-party APIs (Gemini, ClipDrop, Cloudinary, Clerk)
Handling ES Module vs CommonJS compatibility issues
JWT-based authentication with Clerk
Serverless PostgreSQL with Neon
CORS configuration for cross-origin requests
Rate limiting and API quota management


👨‍💻 Developer
Akshat Gupta
MCA Final Year — Major Project


📄 License
This project is for educational purposes as part of an MCA Final Year Project.
ShareContentpdfimport React, { useState } from "react";
import { Edit, Sparkle } from "lucide-react";
import { axios } from "axios";
import { useAuth } from "@clerk/clerk-react";
import toast from "react-hot-toast";
import Markdown from "react-markdown";

axios.default.baseURL = import.meta.env.VITE_BASE_URpastedimport { clerkClient } from "@clerk/express";
import sql from "../configs/db.js";
import { auth } from "./../middlewares/auth.js";
import axios from "axios";
import { v2 as cloudinary } from "cloudinary";
import fs from "fs";
import * as pdf from "pdf-parse";
// import pdf from "pdf-parse/libpastedimport React, { useState } from "react";
import { Edit, Sparkle } from "lucide-react";
import axios from "axios";
import { useAuth } from "@clerk/clerk-react";
import toast from "react-hot-toast";
import Markdown from "react-markdown";

axios.defaults.baseURL = import.meta.env.VITE_BASE_URL;
pastedimport { Hash, Sparkles } from "lucide-react";
import React, { useState } from "react";
import axios from "axios";
import toast from "react-hot-toast";
import Markdown from "react-markdown";
import { useAuth } from "@clerk/clerk-react";

//after backend
axios.defaults.baseURL = import.meta.epastedimport { clerkClient } from "@clerk/express";
import sql from "../configs/db.js";
import { auth } from "./../middlewares/auth.js";
import axios from "axios";
import { v2 as cloudinary } from "cloudinary";
import fs from "fs";
import * as pdf from "pdf-parse";
// import pdf from "pdf-parse/libpastedimport { Hash, Sparkles } from "lucide-react";
import React, { useState } from "react";
import axios from "axios";
import toast from "react-hot-toast";
import Markdown from "react-markdown";
import { useAuth } from "@clerk/clerk-react";

//after backend
axios.defaults.baseURL = import.meta.epasted
