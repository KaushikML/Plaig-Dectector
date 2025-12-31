# Plagiarism & AI Content Detector
Hybrid Web Search + Local Embeddings System (Hackathon Edition)

------------------------------------------------------------

PROJECT OVERVIEW

This project is a full-stack plagiarism and AI-content detection system that
analyzes uploaded documents against real web sources and generates detailed
PDF reports.

The system is designed with reliability and graceful degradation in mind:
- Core plagiarism detection runs fully offline using local embeddings
- External LLMs (Google Gemini) are optional enhancements
- If LLM quota is unavailable, the system still completes analysis

This makes the project robust for demos, hackathons, and real-world usage.

------------------------------------------------------------

CORE IDEA

Plagiarism detection should not fail because an LLM quota is exhausted.

To achieve this, the system uses:
- Web search and scraping for source discovery
- Local semantic embeddings (E5) for similarity scoring
- Optional LLM usage for query refinement and AI probability
- Strict tunable thresholds to reduce irrelevant matches

------------------------------------------------------------

PROJECT STRUCTURE

Plaig-Dectector/
│
├── backend/
│   ├── app.py               Flask entry point
│   ├── core_detector.py     Plagiarism & AI detection logic
│   ├── auth.py              Authentication routes
│   ├── database.py          MongoDB connection
│   ├── config.py            Configuration & env loading
│   ├── .env                 Environment variables
│   ├── requirements.txt
│   └── .venv/               Python virtual environment
│
├── frontend/                React (Vite)
│
└── README.md

------------------------------------------------------------

FEATURES

User Management
- User registration and login
- Secure session-based authentication
- User-specific report history

Plagiarism Detection
- Supports PDF, DOCX, TXT, and raw text input
- Web-scale comparison using Serper (Google Search API)
- Scrapes real web pages
- Semantic similarity using local E5 embeddings
- Fuzzy matching using RapidFuzz
- Configurable precision/recall tuning

AI Content Detection (Optional)
- Uses Google Gemini when quota is available
- Automatically falls back if unavailable
- Never blocks plagiarism analysis

Reporting
- Originality score
- Overlap excerpts with source URLs
- Fuzzy and semantic similarity scores
- Search queries used
- AI probability (if available)
- Downloadable PDF reports

Dashboard
- User statistics
- Analysis history
- Report downloads

------------------------------------------------------------

ARCHITECTURE OVERVIEW

User Upload
   ↓
Text Extraction
   ↓
Query Generation
   ├─ Gemini (optional)
   └─ Keyword fallback
   ↓
Serper Web Search
   ↓
Web Scraping
   ↓
Local E5 Embeddings (Offline)
   ↓
Similarity Scoring
   ↓
PDF Report Generation

------------------------------------------------------------

LOCAL EMBEDDINGS (WHY E5)

The system uses SentenceTransformers with the model:
intfloat/e5-large-v2

Advantages:
- Runs completely locally
- No API keys or quotas
- Strong semantic similarity performance
- Ideal for hackathons and demos

This removes the biggest reliability risk in plagiarism systems.

------------------------------------------------------------

TUNING KNOBS

Defined in core_detector.py:

MAX_QUERIES    = 6
MAX_PAGES     = 30
WINDOW_SENT   = 5
TH_FUZZ       = 72
TH_COS        = 0.86
MAX_OVERL_URL = 3

These control precision vs recall.

Recommended usage:
- Resume: stricter thresholds, MAX_OVERL_URL = 1–2
- Academic report: balanced defaults
- Blog/article: slightly relaxed thresholds
- Legal documents: very strict thresholds

------------------------------------------------------------

TECH STACK

Backend
- Python
- Flask
- MongoDB
- SentenceTransformers (E5 embeddings)
- RapidFuzz
- BeautifulSoup
- ReportLab
- NLTK
- Serper API
- Google Gemini (optional)

Frontend
- React
- Vite
- React Router
- Custom CSS

------------------------------------------------------------

ENVIRONMENT VARIABLES

Create backend/.env:

FLASK_SECRET_KEY=your_secret_key
MONGO_URI=mongodb://localhost:27017/plagiarism
SERPER_API_KEY=your_serper_key

Optional (LLM features):
GOOGLE_API_KEY=your_gemini_key
GEMINI_MODEL=gemini-2.0-flash

DEBUG=True

Gemini is optional. Plagiarism detection works without it.

------------------------------------------------------------

BACKEND SETUP (WITH .venv)

cd backend
python -m venv .venv
.\\.venv\\Scripts\\activate   (Windows)
pip install -r requirements.txt
python app.py

Backend runs at:
http://127.0.0.1:5000

------------------------------------------------------------

FRONTEND SETUP

cd frontend
npm install
npm run dev

Frontend runs at:
http://127.0.0.1:5173

------------------------------------------------------------

DESIGN PHILOSOPHY

- LLMs are enhancements, not dependencies
- Local ML for correctness
- Graceful degradation
- Transparent reporting
- Configurable precision

This project is built with production thinking, not just demo logic.
