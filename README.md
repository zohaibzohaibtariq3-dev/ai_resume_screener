# 🧠 AI Resume Screener + Interview Intelligence System

A production-ready full-stack AI system that:
- **Extracts** text from PDF resumes
- **Parses** structured data (name, email, skills, education, experience)
- **Detects** 60+ technical skills with expertise levels
- **Matches** resume against job descriptions using TF-IDF cosine similarity
- **Generates** tailored interview questions (technical, behavioral, role-based)

---

## 📁 Project Structure

```
ai_resume_screener/
├── backend/
│   ├── main.py                    ← FastAPI app entry point
│   ├── requirements.txt           ← Python dependencies
│   ├── Procfile                   ← For Railway deployment
│   ├── models/
│   │   └── schemas.py             ← Pydantic data models
│   ├── routes/
│   │   └── resume_routes.py       ← All API endpoints
│   └── utils/
│       ├── pdf_extractor.py       ← PDF → text extraction
│       ├── resume_parser.py       ← NLP field extraction
│       ├── skill_analyzer.py      ← 60+ skill detection + levels
│       ├── job_matcher.py         ← TF-IDF similarity scoring
│       └── question_generator.py  ← Interview question bank
├── frontend/
│   ├── app.py                     ← Streamlit UI
│   └── requirements.txt
├── .streamlit/
│   └── config.toml
├── render.yaml                    ← Render.com deployment config
├── TEST_DATA.txt                  ← Sample resume + JD for testing
└── README.md
```

---

## ⚡ Quick Start (Local)

### Step 1: Set up the Backend

```bash
# Clone or navigate into the project
cd ai_resume_screener/backend

# Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate        # Linux/Mac
venv\Scripts\activate           # Windows

# Install dependencies
pip install -r requirements.txt

# Download the spaCy language model (do this once)
python -m spacy download en_core_web_sm

# Start the FastAPI server
uvicorn main:app --reload --port 8000
```

✅ API running at: http://localhost:8000  
✅ Swagger docs at: http://localhost:8000/docs

---

### Step 2: Set up the Frontend

```bash
# In a new terminal
cd ai_resume_screener/frontend

# Install frontend dependencies
pip install -r requirements.txt

# Start Streamlit
streamlit run app.py
```

✅ Frontend at: http://localhost:8501

---

## 🔌 API Endpoints

| Method | Endpoint                          | Description                          |
|--------|-----------------------------------|--------------------------------------|
| GET    | `/api/v1/health`                  | Health check                         |
| POST   | `/api/v1/upload_resume`           | Upload PDF → extract text            |
| POST   | `/api/v1/parse_resume`            | Text → structured resume data        |
| POST   | `/api/v1/match_job`               | Resume + JD → match score            |
| POST   | `/api/v1/generate_questions`      | Resume + JD → interview questions    |
| POST   | `/api/v1/analyze_skills`          | Text → skill list only               |

---

## 🧪 Testing with Sample Data

Use the data in `TEST_DATA.txt`:

1. Copy the sample resume text
2. Create a dummy PDF (or use any real resume PDF)
3. Upload it on the Streamlit frontend
4. Paste the sample job description in the Job Match tab
5. Expected: ~80% match score, 10+ matched skills

Or test directly via Swagger at `http://localhost:8000/docs`

---

## 🚀 Deployment

### Backend → Render.com

1. Push your code to GitHub
2. Go to [render.com](https://render.com) → New → Web Service
3. Connect your repo
4. Render auto-detects `render.yaml` — no config needed
5. Build command runs automatically (installs deps + downloads spaCy model)
6. Your API URL: `https://your-app.onrender.com`

### Backend → Railway.app

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login and deploy
railway login
railway init
railway up
```

Set environment variables in Railway dashboard if needed.

### Frontend → Streamlit Cloud

1. Push code to GitHub
2. Go to [share.streamlit.io](https://share.streamlit.io)
3. New app → select repo → set main file: `frontend/app.py`
4. In `frontend/app.py`, change `API_BASE` to your deployed backend URL:
   ```python
   API_BASE = "https://your-backend.onrender.com/api/v1"
   ```
5. Deploy!

---

## 🏗️ Architecture

```
[User] → [Streamlit UI]
               ↓ HTTP POST (multipart PDF)
         [FastAPI Backend]
               ↓
    ┌──────────────────────────┐
    │   pdf_extractor.py       │  pdfplumber → raw text
    │   resume_parser.py       │  spaCy NER → name/email/phone
    │   skill_analyzer.py      │  regex + context → skills
    │   job_matcher.py         │  TF-IDF + cosine → score
    │   question_generator.py  │  template bank → questions
    └──────────────────────────┘
```

---

## 🔧 Tech Stack

| Layer     | Technology              | Why                                      |
|-----------|-------------------------|------------------------------------------|
| Backend   | FastAPI                 | Fast, async, auto-docs                   |
| PDF       | pdfplumber              | Better multi-column support than PyPDF2  |
| NLP       | spaCy                   | Fast NER for name/entity detection       |
| ML        | scikit-learn (TF-IDF)   | Lightweight, no GPU needed               |
| Frontend  | Streamlit               | Rapid data app UI                        |
| Serve     | Uvicorn                 | ASGI server for FastAPI                  |

---

## 📊 Skill Coverage

The system detects **60+ skills** across:
- **Languages**: Python, JS, Java, C++, Go, Rust, R, Scala...
- **Frameworks**: React, Django, FastAPI, Flask, Node.js, Spring...
- **ML/AI**: TensorFlow, PyTorch, scikit-learn, NLP, LLMs...
- **Cloud**: AWS, GCP, Azure, Docker, Kubernetes...
- **Data**: SQL, MongoDB, Redis, Spark, Pandas...
- **Tools**: Git, CI/CD, REST API, Agile...

---

## 🛡️ Error Handling

- Non-PDF uploads → `400 Bad Request`
- Empty files → `400 Bad Request`
- Files > 10MB → `413 Request Entity Too Large`
- Scanned PDFs (no text) → `422 Unprocessable Entity`
- Short JD → `400 Bad Request`
- Server errors → caught + returned as JSON

---

Built with ❤️ | FastAPI + spaCy + scikit-learn + Streamlit
