# ResumeAI – ATS Optimizer 🚀

AI-powered resume builder and ATS scoring tool. Built with React + Node.js + Python FastAPI.

---

## 🗂️ Project Structure

```
resumeai/
├── frontend/          # React + Vite + Tailwind
├── backend/           # Node.js + Express + MongoDB
└── ai-service/        # Python + FastAPI + NLP
```

---

## ⚡ Quick Start (Local Development)

### Prerequisites
- Node.js 18+
- Python 3.10+
- MongoDB Atlas account (free) or local MongoDB

---

### 1. Clone & Setup

```bash
git clone https://github.com/YOUR_USERNAME/resumeai.git
cd resumeai
```

---

### 2. Frontend Setup

```bash
cd frontend
npm install

# Create .env (already included, update if needed)
# VITE_API_URL=http://localhost:5000/api

npm run dev
# Runs on http://localhost:3000
```

---

### 3. Backend Setup

```bash
cd backend
npm install

# Edit .env:
# MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/resumeai
# JWT_SECRET=your_super_secret_32_char_minimum_key
# AI_SERVICE_URL=http://localhost:8000

npm run dev
# Runs on http://localhost:5000
```

**MongoDB Atlas Setup (free):**
1. Go to https://cloud.mongodb.com → Create free cluster
2. Create database user (username + password)
3. Allow network access (0.0.0.0/0 for dev)
4. Copy connection string → paste in MONGODB_URI

---

### 4. AI Service Setup

```bash
cd ai-service
python -m venv venv

# Windows:
venv\Scripts\activate

# Mac/Linux:
source venv/bin/activate

pip install -r requirements.txt

# First run downloads ~90MB sentence-transformer model
python main.py
# Runs on http://localhost:8000
```

---

## 🌐 Deployment

### Frontend → Vercel (Free)

```bash
cd frontend

# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Set environment variable in Vercel dashboard:
# VITE_API_URL = https://your-backend.onrender.com/api
```

Add `vercel.json` in frontend/:
```json
{
  "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
}
```

---

### Backend → Render (Free)

1. Push to GitHub
2. Go to https://render.com → New Web Service
3. Connect your repo, select `backend/` directory
4. Build command: `npm install`
5. Start command: `node src/app.js`
6. Add environment variables:
   - `MONGODB_URI`
   - `JWT_SECRET`
   - `AI_SERVICE_URL` (your AI service Render URL)
   - `NODE_ENV=production`
   - `FRONTEND_URL` (your Vercel URL)

---

### AI Service → Render (Free)

1. New Web Service → select `ai-service/` directory
2. Runtime: Python 3.11
3. Build command: `pip install -r requirements.txt`
4. Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. Add env vars: `MODEL_CACHE_DIR=/tmp/models`

---

## 🧪 Testing the API

```bash
# Health check
curl http://localhost:5000/health

# Register
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Test User","email":"test@test.com","password":"test123"}'

# Login
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@test.com","password":"test123"}'

# ATS Analyze (replace TOKEN)
curl -X POST http://localhost:5000/api/ats/analyze \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"resumeData":{"personalInfo":{"name":"Rahul"},"skills":{"technical":["React","Node.js"]}},"jobDescription":"We need a React developer with Node.js experience"}'

# Python AI service health
curl http://localhost:8000/health
```

---

## 📁 Key Files Reference

| File | Purpose |
|------|---------|
| `frontend/src/store/authSlice.js` | JWT auth state management |
| `frontend/src/store/resumeSlice.js` | Resume CRUD state |
| `frontend/src/pages/ATSPage.jsx` | ATS analyzer UI |
| `frontend/src/pages/BuilderPage.jsx` | Resume editor + live preview |
| `backend/src/controllers/ats.controller.js` | ATS orchestration (calls AI service) |
| `backend/src/utils/resume.parser.js` | PDF/DOCX text extraction |
| `ai-service/app/services/ats_scorer.py` | Core NLP scoring engine |

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, Vite, Tailwind CSS, Redux Toolkit |
| Backend | Node.js, Express, MongoDB, Mongoose, JWT |
| AI Service | Python, FastAPI, sentence-transformers, NLTK, scikit-learn |
| Auth | JWT (7-day expiry, stored in localStorage) |
| File Parse | pdf-parse (PDF), mammoth (DOCX) |
| PDF Export | react-to-print (browser print → PDF) |

---

