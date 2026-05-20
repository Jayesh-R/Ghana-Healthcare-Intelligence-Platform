# 🏥 Ghana Healthcare Intelligence Platform
### Databricks × Accenture Hackathon 2024 — Virtue Foundation Track

> **Bridging Medical Deserts with Agentic AI** — An intelligent document parsing and resource planning system that transforms 987 messy Ghana hospital records into actionable healthcare intelligence for NGO planners.

---

## 🎯 What It Does

The Ghana Healthcare Intelligence Platform is a full-stack agentic AI system that:

- **Finds medical deserts** — regions in Ghana with zero emergency, ICU, or surgical care
- **Parses unstructured hospital data** — converts messy Facebook posts and web listings into structured medical schema
- **Detects anomalies** — flags hospitals with suspicious or inconsistent capability claims
- **Plans resource deployment** — AI-powered recommendations on where to send doctors
- **Answers natural language queries** — NGO planners can ask questions in plain English and get cited, traceable answers

---

## 🚀 Live Demo

| Credential | Value |
|------------|-------|
| Demo URL | `http://localhost:5173` (local) |
| Test Email | `demo@virtuefoundation.org` |
| Test Password | `demo1234` |

> Register a new account.

---

## 📸 Screenshots

### Dashboard — Real-time stats from 987 hospitals
- 847 total facilities across Ghana
- 23 medical deserts identified
- 3 critical regions with zero emergency care
- 58% average data completeness

### AI Agent — RAG-powered natural language queries
- Ask: *"Which regions have no emergency care?"*
- Get: Real hospital names, citations, agent reasoning steps
- Powered by: FAISS semantic search + Groq/Llama-3.1

### Medical Deserts Map — Interactive Ghana coverage map
- Color-coded by gap severity (green/yellow/red)
- Click any region for detailed breakdown
- Identifies Upper West, Upper East, Northern as critical

### Anomaly Detection — 50 flagged suspicious records
- Hospitals claiming surgery with no equipment listed
- Facilities with ICU claims but zero bed capacity
- Incomplete records needing verification

### IDP Demo — Before/After parsing visualization
- Raw Facebook post → structured medical schema
- Extracts: procedure, equipment, capability, specialties
- Uses exact Virtue Foundation schema

### Resource Planner — AI deployment recommendations
- Select region + available doctors
- Get AI plan citing specific hospitals
- Copy plan to clipboard for immediate use

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────┐
│         NGO Planner (Browser)           │
└──────────────┬──────────────────────────┘
               │ HTTP/REST
┌──────────────▼──────────────────────────┐
│     Frontend — React + Vite + Tailwind  │
│  Dashboard │ AI Agent │ Medical Deserts │
│  Hospitals │ Anomalies│ IDP Demo        │
│  Resource Planner │ Login/Register      │
└──────────────┬──────────────────────────┘
               │ API calls
┌──────────────▼──────────────────────────┐
│      Backend — FastAPI + Python         │
│  /api/login    /api/register            │
│  /api/query    /api/parse               │
│  /api/hospitals /api/stats              │
│  /api/anomalies /api/me                 │
│         MLFlow Tracking                 │
└──────┬───────────────┬──────────────────┘
       │               │
┌──────▼──────┐ ┌──────▼──────────────────┐
│  AI Stack   │ │      Data Layer          │
│  FAISS RAG  │ │  SQLite (healthcare.db)  │
│  SentenceT. │ │  987 Ghana hospitals     │
│  Groq LLM   │ │  hospital_index.faiss    │
│  Llama-3.1  │ │  Ghana CSV dataset       │
└─────────────┘ └──────────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Frontend | React 18 + Vite | UI framework |
| Styling | Tailwind CSS | Design system |
| Maps | React-Leaflet | Ghana coverage map |
| Charts | Recharts | Data visualization |
| Backend | FastAPI (Python) | REST API |
| Database | SQLite | Hospital records storage |
| Auth | JWT + bcrypt | User authentication |
| Vector DB | FAISS | Semantic hospital search (RAG) |
| Embeddings | SentenceTransformers | Text → vectors |
| LLM | Groq / Llama-3.1-8b | AI responses |
| ML Tracking | MLFlow | Experiment logging |

---

## ⚙️ Setup & Installation

### Prerequisites
- Python 3.10+
- Node.js 18+
- Groq API key (free at console.groq.com)

### Backend Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/ghana-healthcare.git
cd ghana-healthcare

# Setup Python virtual environment
cd backend
python -m venv venv

# Activate (Windows)
.\venv\Scripts\Activate.ps1

# Activate (Mac/Linux)
source venv/bin/activate

# Install dependencies
pip install fastapi uvicorn groq python-jose passlib bcrypt \
            python-multipart python-dotenv faiss-cpu \
            sentence-transformers mlflow pandas
OR

pip install -r requirements.txt

# Add your Groq API key in agent.py
# Replace: client = Groq(api_key="gsk_your_key_here")

# Load Ghana hospital data
python load_data.py

# Start the backend
uvicorn main:app --reload
```

Backend runs at: `http://localhost:8000`

### Frontend Setup

```bash
cd healthcare-agent
npm install
npm run dev
```

Frontend runs at: `http://localhost:5173`

### MLFlow Dashboard (Optional)

```bash
cd backend
mlflow ui
```

MLFlow runs at: `http://localhost:5000`

---

## 📊 Dataset

- **Source**: Virtue Foundation Ghana Dataset
- **Size**: 987 healthcare facility records
- **Fields**: 41 columns including name, location, specialties, capabilities, procedures, equipment
- **Coverage**: All regions of Ghana
- **Format**: CSV → SQLite + FAISS vector index

---

## ✨ Core Features (MVP)

### 1. Unstructured Feature Extraction (IDP)
- Processes free-form text from `procedure`, `equipment`, `capability` columns
- Extracts structured medical data using Groq/Llama-3.1
- Uses exact Virtue Foundation schema (same prompts used to create the dataset)
- Live demo with 3 real examples + custom text input

### 2. Intelligent Synthesis
- Combines RAG semantic search with SQL structured queries
- FAISS index enables semantic understanding (not just keyword matching)
- "chest pain" → finds cardiology hospitals without keyword match
- Regional gap analysis combines unstructured capabilities with structured location data

### 3. Planning System
- Resource Deployment Planner for NGO coordinators
- Select target region + available medical resources
- AI generates specific hospital placement recommendations
- Cites real hospital names and patient impact estimates
- Copy plan to clipboard for immediate action

---

## 🎯 Stretch Goals Achieved

### ✅ Citations
- Row-level citations showing exact hospital records used
- Step-level agent reasoning (which data was used at each step)
- Semantic similarity scores shown per citation

### ✅ Map Visualization
- Interactive Ghana map with color-coded coverage
- Red = critical (no emergency care)
- Yellow = warning
- Green = adequate coverage
- Click regions for detailed stats + recommendations

### ✅ Anomaly Detection
- 50 flagged suspicious hospital records
- High/Medium/Low severity classification
- Specific issues: surgery claims without equipment, ICU without beds, no contact info

---

## 🔍 API Reference

### Authentication
```
POST /api/register    { name, email, password }
POST /api/login       { email, password } → { token, user }
GET  /api/me          Headers: Authorization: Bearer <token>
```

### Agent
```
POST /api/query       { question } → { answer, citations, agentSteps }
POST /api/parse       { text } → { procedure, equipment, capability, specialties }
```

### Data
```
GET /api/hospitals    → list of 987 hospitals with coordinates
GET /api/stats        → dashboard statistics
GET /api/anomalies    → flagged suspicious records
```

---

## 📁 Project Structure

```
ghana-healthcare/
├── backend/
│   ├── main.py          # FastAPI application + all endpoints
│   ├── agent.py         # AI agent with RAG search
│   ├── rag.py           # FAISS vector index + semantic search
│   ├── auth.py          # JWT authentication
│   ├── database.py      # SQLite setup
│   ├── load_data.py     # CSV → SQLite loader
│   ├── ghana_hospitals.csv
│   ├── healthcare.db    # SQLite database (generated)
│   └── hospital_index.faiss  # FAISS index (generated)
│
└── healthcare-agent/
    ├── src/
    │   ├── App.jsx              # Main app + routing
    │   ├── mockData.js          # Development mock data
    │   ├── pages/
    │   │   ├── Dashboard.jsx
    │   │   ├── AgentChat.jsx
    │   │   ├── MedicalDeserts.jsx
    │   │   ├── HospitalExplorer.jsx
    │   │   ├── Anomalies.jsx
    │   │   ├── IDPDemo.jsx
    │   │   ├── ResourcePlanner.jsx
    │   │   └── Login.jsx
    │   └── index.css
    ├── package.json
    └── vite.config.js
```

---

## 🌍 Social Impact

Every feature was built with a specific real-world impact in mind:

| Feature | Real Impact |
|---------|-------------|
| Medical Desert Detection | NGOs can identify where to build new facilities |
| Resource Planner | Reduces time to deploy doctors from weeks to minutes |
| Anomaly Detection | Prevents NGOs from sending doctors to facilities that misrepresent capabilities |
| IDP Parsing | Automates data entry from thousands of unstructured web sources |
| Natural Language Agent | Makes healthcare intelligence accessible to non-technical planners |

> By 2030, the world faces a shortage of 10 million healthcare workers. This system helps ensure existing workers are coordinated intelligently — potentially saving millions of lives through better resource allocation.

---

## 👥 Team

Built for the Databricks × Accenture Hackathon 2024 — Virtue Foundation Track.

---

## 📄 License

MIT License — see LICENSE file for details.

---

## 🙏 Acknowledgments

- **Virtue Foundation** — for providing the real Ghana hospital dataset
- **Databricks** — for hosting the hackathon
- **Groq** — for free LLM API access
- **FAISS** — Meta's vector similarity library
