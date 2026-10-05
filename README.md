# PrepPath AI

> **Opportunity-to-Application Readiness Platform**  
> Automated scholarship and grant eligibility analysis combining Gemini-powered document extraction with a deterministic rule evaluation engine.

---

## Overview

Applying for scholarships and academic grants is often hindered by dense, ambiguous policy documents and complex eligibility criteria (income caps, academic thresholds, category reservations, and state domicile requirements).

**PrepPath AI** solves this by providing an end-to-end evaluation pipeline:
1. **Document Ingestion:** Extracts clean text from uploaded PDF guidelines in-memory.
2. **Structured AI Extraction:** Leverages Google Gemini via structured outputs to extract granular demographic, academic, and document submission requirements—backed by exact source text citations.
3. **Deterministic Rule Engine:** Evaluates student profiles against extracted criteria using strict logic (no LLM hallucinations in eligibility math) to produce transparent pass/fail/unknown breakdowns.
4. **Interactive Dashboard:** Modern React + Tailwind interface for document upload, real-time extraction review, applicant profile matching, and document readiness tracking.

---

## Architecture

```
[ Scholarship PDF ] 
         │
         ▼
[ In-Memory PDF Parser (PyMuPDF) ]
         │  Plain text
         ▼
[ Gemini AI Extraction Engine ] ──▶ [ Pydantic Schemas ]
         │                              (Validated Extraction)
         ▼                                      │
[ Deterministic Evaluation Engine ] ◀───────────┘
         ▲
         │ (Student Profile Data)
[ Student Profile / Supabase ]
         │
         ▼
[ Eligibility Breakdown & Checklist ]
         │  (ELIGIBLE / INELIGIBLE / UNKNOWN)
         ▼
[ React 18 + Vite Frontend Dashboard ]
```

### Key Technical Pillars

- **Zero-Hallucination Evaluation:** AI is restricted to information extraction and normalization. All final eligibility decisions are made by a pure Python deterministic rule engine (`app/services/eligibility.py`) using explicit comparison operators (`<=`, `>=`, `==`).
- **Source Citation:** Extracted requirements and document criteria preserve the exact snippet of source text from the policy document for auditability.
- **In-Memory Document Handling:** Uploaded documents are parsed in memory using PyMuPDF stream processing without retaining persistent disk artifacts.

---

## Tech Stack

| Domain | Technology | Purpose |
|---|---|---|
| **Backend Framework** | FastAPI (Python 3.11+) | Asynchronous RESTful API & OpenAPI documentation |
| **Validation & Schemas** | Pydantic v2 / Pydantic Settings | Type validation, schema enforcement, environment loading |
| **Document Processing** | PyMuPDF (`fitz`) | High-performance in-memory PDF text extraction |
| **AI / Structured Extraction** | Google Gemini API (`google-genai` SDK) | Structured schema extraction with JSON constraints |
| **Frontend Framework** | React 18 + Vite | Fast, modular Single Page Application |
| **Styling & Icons** | Tailwind CSS + Lucide React | Clean, responsive UI with modern typography |
| **Database & Auth** | Supabase (PostgreSQL) | Applicant profile storage and authentication |
| **Testing** | Pytest | Unit and integration test suite |

---

## Repository Structure

```
PrepPath-AI/
├── app/
│   ├── main.py                  # FastAPI factory, CORS & route registration
│   ├── config.py                # Environment configuration via Pydantic Settings
│   ├── ai/
│   │   ├── gemini.py            # Gemini API client & structured generation helpers
│   │   └── opportunity_analyzer.py # PDF text prompt engineering & extraction
│   ├── routes/
│   │   ├── ai.py                # Extraction endpoints
│   │   ├── opportunities.py     # Opportunity listing & CRUD routes
│   │   └── profile.py           # Student profile management routes
│   ├── schemas/
│   │   ├── eligibility.py       # Evaluation result models
│   │   ├── opportunity.py       # Pydantic extraction & opportunity models
│   │   └── student.py           # Student profile input schemas
│   └── services/
│       ├── eligibility.py       # Deterministic rule evaluation engine
│       ├── opportunity_db.py    # Opportunity persistence layer
│       ├── pdf.py               # In-memory PDF text extraction service
│       └── supabase.py          # Supabase client integration
├── src/
│   ├── App.jsx                  # Main application routing and shell
│   ├── index.css                # Tailwind directives & base styles
│   ├── main.jsx                 # React root entrypoint
│   ├── pages/
│   │   ├── OpportunityAnalyzer.jsx # Document upload & eligibility evaluation UI
│   │   └── StudentProfile.jsx   # Student details & qualifications editor
│   └── services/
│       ├── api.js               # Backend API client
│       ├── auth.js              # Authentication helpers
│       └── supabase.js          # Client-side Supabase instance
├── samples/
│   ├── Guidelines_3042.pdf      # Sample scholarship guideline document
│   ├── PM-USP-CSSS.pdf          # Official PM-USP CSSS scholarship scheme rules
│   └── README.md                # Sample data documentation
├── tests/
│   ├── test_eligibility.py      # Unit tests for the rule evaluation engine
│   ├── test_pdf.py              # Tests for PDF validation & in-memory parsing
│   └── test_schemas.py          # Pydantic schema validation tests
├── requirements.txt             # Python dependencies
├── package.json                 # Frontend dependencies & scripts
├── vite.config.js               # Vite build configuration
├── tailwind.config.js           # Tailwind CSS configuration
└── README.md                    # Project documentation
```

---

## Getting Started

### Prerequisites

- **Python:** 3.11 or higher
- **Node.js:** 18.x or higher
- **Gemini API Key:** from [Google AI Studio](https://aistudio.google.com/)

---

### Backend Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/aarvy257-ai/PrepPath-AI.git
   cd PrepPath-AI
   ```

2. **Create and activate a virtual environment:**
   ```bash
   # Windows (PowerShell)
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1

   # Linux / macOS
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables:**
   Create a `.env` file in the project root:
   ```env
   ENVIRONMENT=development
   PORT=8000
   HOST=0.0.0.0

   # Gemini API Credentials
   GEMINI_API_KEY=your_gemini_api_key_here
   GEMINI_MODEL=gemini-3.5-flash

   # Supabase Credentials (optional for local mock testing)
   SUPABASE_URL=your_supabase_project_url
   SUPABASE_PUBLISHABLE_KEY=your_publishable_anon_key
   SUPABASE_SECRET_KEY=your_service_role_key
   ```

5. **Start the API server:**
   ```bash
   uvicorn app.main:app --reload --port 8000
   ```

6. **Verify the server:**
   - Health Check: `http://localhost:8000/health`
   - Interactive API Docs (Swagger UI): `http://localhost:8000/docs`

---

### Frontend Setup

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Configure frontend environment (optional):**
   Create a `.env.local` file:
   ```env
   VITE_SUPABASE_URL=your_supabase_project_url
   VITE_SUPABASE_PUBLISHABLE_KEY=your_publishable_anon_key
   ```

3. **Start the Vite development server:**
   ```bash
   npm run dev
   ```
   Open `http://localhost:5173` in your browser.

---

## Testing

The backend includes automated unit tests covering the rule engine, PDF parser, and schemas:

```bash
pytest tests/ -v
```

Test coverage includes:
- Verification of numeric threshold comparisons (income limits, GPA/percentage cutoffs).
- Domicile state, gender, and social category filtering.
- Handling of missing/partial applicant data (`UNKNOWN` evaluation state).
- In-memory PDF byte stream extraction and corrupted file rejection.

---

## Current Status & Roadmap

- [x] In-memory PDF parsing via PyMuPDF (`fitz`)
- [x] Structured extraction using Google Gemini API (`google-genai`)
- [x] Deterministic evaluation engine with strict multi-operator logic
- [x] FastAPI REST endpoints with OpenAPI documentation
- [x] React 18 + Vite frontend with Tailwind styling
- [x] Applicant profile management view
- [ ] OCR support for scanned image-only PDFs (Tesseract / Cloud Vision)
- [ ] Containerized deployment (Docker + Docker Compose)
- [ ] Production hosting on Cloud Run / Vercel
