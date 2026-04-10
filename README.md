# 🧠 PaperMind — Multi-Agent AI Research Assistant

> A production-grade multi-agent system that autonomously searches 7 academic databases in parallel, synthesizes findings across papers, and delivers structured research reports with citation grounding, contradiction detection, and confidence scoring — streamed token by token in real time.
  
[![Python](https://img.shields.io/badge/Python-3.11-3776ab?logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![LangGraph](https://img.shields.io/badge/LangGraph-1.1-6f42c1)](https://langchain-ai.github.io/langgraph)
[![React](https://img.shields.io/badge/React-18-61dafb?logo=react&logoColor=white)](https://react.dev)
[![Supabase](https://img.shields.io/badge/Supabase-Auth%20%2B%20pgvector-3ecf8e?logo=supabase&logoColor=white)](https://supabase.com)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

---

## Live Video:
https://drive.google.com/file/d/1N1kYi-Y_DtZqZEE0K56GQQRg1DAN9FZW/view?usp=sharing

> ▶ Click to watch — multi-agent research pipeline in action

</div>

---

## What is PaperMind?

PaperMind is a full-stack AI research assistant that replaces hours of literature review with a single query. It orchestrates a team of specialised AI agents — each responsible for one stage of the research pipeline — and streams their progress live to the browser via WebSockets.

**Who it's for:**
- Researchers and academics who need to synthesize literature fast
- Students preparing literature reviews or dissertations
- Engineers tracking the state of the art in a technical domain
- Anyone who has spent an afternoon clicking through Google Scholar

---

## Feature Highlights

| Feature | Details |
|---|---|
| **Multi-agent pipeline** | LangGraph supervisor routes tasks across 4 specialist agents with deterministic state-machine logic |
| **7 academic sources** | arXiv, OpenAlex, Europe PMC, Crossref, PubMed, Semantic Scholar, CORE — queried in parallel |
| **RAG over your PDFs** | Upload your own papers; ChromaDB embeds and retrieves them per query |
| **Real-time streaming** | WebSocket streams every agent event + report tokens as they generate |
| **Citation graph** | D3.js force-directed graph showing how your result papers cite each other, powered by Semantic Scholar |
| **Export anywhere** | Download report as Markdown, PDF (html2canvas + jsPDF), or Word (.docx) |
| **Follow-up questions** | LLM generates 3 contextual follow-up queries after every report |
| **Search history** | Every session saved to Supabase; reload any past report in one click |
| **Per-user knowledge base** | Vector store isolated per user ID — no data leakage between accounts |
| **Rate limiting** | 3 requests/minute per user on the research endpoint (slowapi) |
| **Multi-provider LLM** | Ollama (local) → Groq → Gemini fallback chain; swap with one env var |
| **Auth** | Supabase Auth (email + Google OAuth) with fallback JWT for local dev |
| **Dark / light theme** | System-aware with manual toggle, persisted to localStorage |

---

## Architecture

```
Browser (React + Vite)
│
│  WebSocket (/api/v1/ws/research)
│  REST      (/api/v1/*)
▼
FastAPI  ──  SlowAPI rate limiter  ──  Supabase JWT auth
│
▼
LangGraph Supervisor  (deterministic state router)
│
├── Search Agent ──► arXiv · OpenAlex · EuropePMC
│                    Crossref · PubMed · S2 · CORE
│                    (ThreadPoolExecutor, 7 workers)
│
├── RAG Agent ───► ChromaDB  ◄── Uploaded PDFs
│                  (sentence-transformers / all-MiniLM-L6-v2)
│
├── Summarizer ──► Per-paper structured summaries (LLM)
│
└── Synthesizer ──► Final cited report + follow-up questions (LLM)
                    Token-level streaming via asyncio queue
```

**State management:** a single `ResearchState` TypedDict flows through the graph. Every agent reads from it and returns a partial update — no shared mutable state.

**Streaming:** the synthesizer writes report tokens into an `asyncio.Queue` via a `contextvars` context variable. The WebSocket handler drains the queue and forwards each token to the browser in real time.

---

## Tech Stack

### Backend
| Concern | Technology |
|---|---|
| API framework | FastAPI 0.115 + Uvicorn |
| Agent orchestration | LangGraph 0.2 (supervisor pattern) |
| LLM providers | Ollama · Groq (`llama-3.3-70b`) · Gemini (`gemini-2.5-flash-lite`) |
| Embeddings | `sentence-transformers/all-MiniLM-L6-v2` (runs locally, free) |
| Vector store | ChromaDB (local, file-persisted) |
| PDF parsing | PyMuPDF (fitz) |
| Auth | Supabase JWT + python-jose fallback |
| Database | Supabase (Postgres) — research session history |
| Rate limiting | slowapi 0.1.9 (per-user, 3 req/min on research endpoint) |
| Observability | LangSmith tracing (optional) |

### Frontend
| Concern | Technology |
|---|---|
| Framework | React 18 + Vite 5 |
| Markdown rendering | react-markdown + remark-gfm |
| Citation graph | D3.js v7 (force-directed, lazy-loaded) |
| PDF export | jsPDF + html2canvas (lazy-loaded) |
| Word export | docx (lazy-loaded) |
| Auth | @supabase/supabase-js |
| Styling | CSS custom properties (no CSS framework) |

### Infrastructure
| Concern | Technology |
|---|---|
| Containerisation | Docker + docker-compose |
| Deployment | Fly.io / Render |
| CI/CD | GitHub Actions |
| Secrets | `.env` + platform environment variables |

---

## Project Structure

```
papermind/
│
├── main.py                        # FastAPI app, middleware, router registration
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── fly.toml / render.yaml
│
├── core/
│   ├── config.py                  # Pydantic settings (env-driven)
│   ├── auth.py                    # JWT decode, Supabase + fallback
│   ├── database.py                # Supabase session persistence
│   ├── llm.py                     # Multi-provider LLM factory
│   ├── vector_store.py            # ChromaDB singleton + user isolation
│   ├── limiter.py                 # slowapi Limiter (per-user key_func)
│   ├── events.py                  # StreamEvent / EventType dataclasses
│   ├── stream_manager.py          # asyncio queue wrapper
│   ├── stream_context.py          # contextvars for per-request stream
│   └── logging_config.py         # Structured JSON logging
│
├── agents/
│   ├── state.py                   # ResearchState TypedDict
│   ├── tools.py                   # Academic API fetchers + RAG tools
│   ├── graph.py                   # LangGraph graph builder
│   └── nodes/
│       ├── supervisor.py          # Deterministic routing logic
│       ├── search_agent.py        # Parallel multi-source search
│       ├── rag_agent.py           # ChromaDB retrieval
│       ├── summarizer_agent.py    # Per-paper LLM summarization
│       └── synthesizer_agent.py  # Report synthesis + streaming
│
├── api/
│   ├── routes.py                  # Research, upload, history, citation graph endpoints
│   └── auth_routes.py             # Login, signup, /me endpoints
│
└── papermind-react/               # Vite + React frontend
    ├── index.html
    ├── public/
    │   └── favicon.svg
    └── src/
        ├── App.jsx                # Root layout, tab switcher (Report / Citation Graph)
        ├── App.css                # Design tokens, dark/light themes
        ├── hooks/
        │   └── useResearch.js     # All API + WebSocket logic, auth hook
        └── components/
            ├── SearchBar.jsx      # Query input + follow-up toggle + history dropdown
            ├── AgentTerminal.jsx  # Live event log
            ├── ReportView.jsx     # Markdown renderer + export buttons
            ├── CitationGraph.jsx  # D3 force graph + node detail panel
            ├── Sidebar.jsx        # Provider status, KB stats, PDF upload
            ├── LoginModal.jsx     # Email / Google auth
            └── ThemeToggle.jsx
```

---

## Quick Start

### Prerequisites
- Python 3.11+
- Node.js 18+
- [Ollama](https://ollama.com) (optional — for free local inference)
- A [Supabase](https://supabase.com) project (optional — for auth + history; falls back to local JWT)

### 1. Clone

```bash
git clone https://github.com/yourusername/papermind.git
cd papermind
```

### 2. Backend

```bash
# Create virtual environment
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
```

Edit `.env` — the minimum required to run:

```env
# Choose your LLM provider: ollama | groq | gemini
LLM_PROVIDER=groq
GROQ_API_KEY=your_groq_api_key_here

# Optional — Gemini fallback
GEMINI_API_KEY=your_gemini_api_key_here

# Optional — web search
TAVILY_API_KEY=your_tavily_api_key_here

# App
APP_ENV=development
APP_HOST=0.0.0.0
APP_PORT=8000

# JWT secret for local auth (change in production)
SECRET_KEY=change-this-to-a-32-char-random-string

# Supabase (optional — enables persistent auth + history)
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_KEY=your_service_role_key
SUPABASE_JWT_SECRET=your_jwt_secret
```

```bash
# Start the API
python main.py
# → http://localhost:8000
# → http://localhost:8000/docs  (Swagger UI)
```

### 3. Frontend

```bash
cd papermind-react

# Install dependencies
npm install

# Configure
echo "VITE_API_URL=http://localhost:8000" > .env

# Dev server
npm run dev
# → http://localhost:5173

# Production build
npm run build
```

### 4. Local LLM (optional — fully free, no API keys)

```bash
ollama pull llama3.2:3b
ollama serve
# Set LLM_PROVIDER=ollama in .env
```

### 5. Docker (everything in one command)

```bash
docker compose up --build
# API → http://localhost:8000
```

---

## Environment Variables Reference

| Variable | Required | Default | Description |
|---|---|---|---|
| `LLM_PROVIDER` | Yes | `ollama` | Active LLM: `ollama`, `groq`, or `gemini` |
| `GROQ_API_KEY` | If using Groq | — | [Get free key](https://console.groq.com) |
| `GROQ_MODEL` | No | `llama-3.3-70b-versatile` | Groq model ID |
| `GEMINI_API_KEY` | If using Gemini | — | [Get free key](https://aistudio.google.com) |
| `GEMINI_MODEL` | No | `gemini-2.5-flash-lite` | Gemini model ID |
| `OLLAMA_BASE_URL` | No | `http://localhost:11434` | Ollama server URL |
| `OLLAMA_MODEL` | No | `llama3.2:3b` | Ollama model |
| `TAVILY_API_KEY` | No | — | Web search via [Tavily](https://tavily.com) |
| `SECRET_KEY` | Yes | *(insecure default)* | JWT signing secret — **change in prod** |
| `SUPABASE_URL` | No | — | Enables persistent auth + history |
| `SUPABASE_KEY` | No | — | Supabase service role key |
| `SUPABASE_JWT_SECRET` | No | — | Supabase JWT secret |
| `LANGCHAIN_API_KEY` | No | — | Enables [LangSmith](https://smith.langchain.com) tracing |
| `LANGCHAIN_TRACING_V2` | No | `false` | Enable LangSmith |
| `APP_ENV` | No | `development` | `development` enables Uvicorn auto-reload |
| `APP_PORT` | No | `8000` | API port |

---

## API Reference

All endpoints are prefixed `/api/v1`. Interactive docs at `/docs`.

### Research

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `POST` | `/research` | ✅ | Run full pipeline (REST, blocking). Rate-limited: 3 req/min per user |
| `WS` | `/ws/research` | ✅ | Stream agent events + report tokens in real time |
| `POST` | `/citation-graph` | ✅ | Fetch citation graph data from Semantic Scholar for a list of papers |

**WebSocket protocol:**

Send after connection:
```json
{ "query": "attention mechanism in transformers", "follow_up": true }
```

Receive event stream:
```json
{ "type": "pipeline_start",    "message": "...", "data": {} }
{ "type": "agent_start",       "message": "🔍 Searching 6 academic sources...", "data": { "agent": "search_agent" } }
{ "type": "agent_complete",    "message": "✅ Found 42 papers", "data": { "papers_found": 42 } }
{ "type": "report_token",      "message": "The attention mechanism...", "data": {} }
{ "type": "pipeline_complete", "message": "Research complete.", "data": { "report": "...", "papers_found": 42, "follow_up_questions": [...], "papers": [...] } }
```

### History

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/history` | ✅ | List last 20 research sessions |
| `GET` | `/history/{id}` | ✅ | Get full report for a session |
| `DELETE` | `/history/{id}` | ✅ | Delete a session |

### Knowledge Base

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `POST` | `/upload-pdf` | ✅ | Ingest a PDF into the user's vector store |
| `GET` | `/vector-store/stats` | ✅ | Chunk count and status |
| `DELETE` | `/vector-store/clear` | ✅ | Remove all user's documents |

### System

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/providers` | — | Active LLM provider and configured fallbacks |
| `GET` | `/health` | — | Health check |

---

## Deployment

### Fly.io (recommended)

```bash
fly auth login
fly launch          # detects fly.toml automatically
fly secrets set GROQ_API_KEY=... SUPABASE_URL=... SUPABASE_KEY=... SECRET_KEY=...
fly deploy
```

### Render

Connect your GitHub repo to Render — `render.yaml` is pre-configured. Set environment variables in the Render dashboard.

### Docker (self-hosted)

```bash
docker compose up -d
```

The `docker-compose.yml` mounts `./chroma_db` and `./uploaded_pdfs` as volumes so data persists across restarts.

---

## How It Works

### Agent Pipeline

1. **Supervisor** — reads `ResearchState.current_step` and sets `next` to route to the correct agent. Runs a fixed sequence: `search → rag → summarize → synthesize`.

2. **Search Agent** — launches 7 HTTP fetchers in a `ThreadPoolExecutor`. Each source returns up to 8 papers. Results are deduplicated by title and merged.

3. **RAG Agent** — embeds the query using `all-MiniLM-L6-v2` and retrieves the top-6 chunks from the user's ChromaDB collection. Falls back silently if the KB is empty.

4. **Summarizer Agent** — sends each paper's title + abstract to the LLM with a structured prompt. Summaries are stored in state and passed to the synthesizer.

5. **Synthesizer Agent** — receives all summaries + RAG chunks and generates a long-form research report with citations. Tokens are pushed into an `asyncio.Queue` and streamed to the browser in real time. Optionally generates 3 follow-up questions.

### LLM Fallback Chain

```python
# core/llm.py
providers = [
    ("ollama",  build_ollama),
    ("groq",    build_groq),
    ("gemini",  build_gemini),
]
# Tries each in order until one succeeds
```

`LLM_PROVIDER` sets the primary. If it fails at runtime, the next available provider is tried automatically.

### Per-User Vector Store Isolation

ChromaDB documents are tagged with `user_id` metadata. All queries filter by `user_id` so users never see each other's uploaded content.

---

## Contributing

Pull requests are welcome. For major changes, open an issue first.

```bash
# Run the API in dev mode (auto-reload)
APP_ENV=development python main.py

# Run the frontend dev server
cd papermind-react && npm run dev
```

Code style: Python follows PEP 8 with 4-space indentation. JavaScript uses 2-space indentation and no semicolons.

---

## License

MIT — see [LICENSE](LICENSE).

---

## Acknowledgements

- [LangChain / LangGraph](https://langchain-ai.github.io/langgraph) — agent orchestration
- [Semantic Scholar API](https://api.semanticscholar.org) — citation graph data
- [OpenAlex](https://openalex.org) — open academic metadata
- [Groq](https://groq.com) — fast free LLM inference
- [Supabase](https://supabase.com) — auth and database

---

<div align="center">
  <sub>Built by <a href="https://github.com/yourusername">Abhishek Verma</a></sub>
</div>
