# 🧠 PaperMind — Multi-Agent AI Research Assistant

> A production-grade multi-agent system that autonomously searches, summarizes,
> and synthesizes academic literature into structured research reports.

![Python](https://img.shields.io/badge/Python-3.11-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-0.115-green)
![LangGraph](https://img.shields.io/badge/LangGraph-1.1-purple)
![License](https://img.shields.io/badge/license-MIT-blue)

## 🏗️ Architecture
```
User Query
    │
    ▼
FastAPI + WebSocket
    │
    ▼
LangGraph Supervisor (deterministic routing)
    ├── Search Agent    → Arxiv, OpenAlex, EuropePMC, Crossref, PubMed, S2, CORE
    ├── RAG Agent       → ChromaDB vector store (uploaded PDFs)
    ├── Summarizer      → Per-paper structured summaries
    └── Synthesizer     → Final cited research report
```

## ⚡ Tech Stack

| Layer | Technology |
|---|---|
| Agent orchestration | LangGraph 1.1 (supervisor pattern) |
| Backend | FastAPI + WebSocket streaming |
| LLM | Ollama (local) → Groq → Gemini (fallback chain) |
| Vector store | ChromaDB + sentence-transformers (all-MiniLM-L6-v2) |
| PDF ingestion | PyMuPDF |
| Academic APIs | Arxiv, OpenAlex, EuropePMC, Crossref, PubMed, Semantic Scholar, CORE |
| Frontend | Streamlit |
| Deployment | Render (API) + Streamlit Cloud (UI) |

## 🚀 Quick Start
```bash
# 1. Clone and install
git clone https://github.com/yourusername/papermind
cd papermind
pip install -r requirements.txt

# 2. Configure environment
cp .env.example .env
# Add your GROQ_API_KEY or GEMINI_API_KEY

# 3. Start local LLM (optional — for free local inference)
ollama pull llama3.2:3b
ollama serve

# 4. Start the API
python main.py

# 5. Start the UI (separate terminal)
streamlit run streamlit_app.py
```

## 🔑 Key Features

- **Multi-agent orchestration** — LangGraph supervisor routes tasks across
  4 specialist agents with deterministic state-based logic
- **Parallel academic search** — queries 7 sources simultaneously,
  skips any that rate-limit or fail, deduplicates results
- **RAG pipeline** — upload PDFs, chunked and embedded locally,
  semantically retrieved per query
- **WebSocket streaming** — real-time agent progress streamed to UI
- **Multi-provider LLM** — Ollama (local) → Groq → Gemini fallback chain,
  swap providers with one `.env` change
- **Zero-cost MVP** — runs entirely on free tiers

## 📁 Project Structure
```
papermind/
├── main.py                  # FastAPI entrypoint
├── streamlit_app.py         # Streamlit frontend
├── core/
│   ├── config.py            # Settings management
│   ├── llm.py               # Multi-provider LLM factory
│   ├── vector_store.py      # ChromaDB singleton
│   ├── events.py            # WebSocket event types
│   └── stream_manager.py    # Per-request event queue
├── agents/
│   ├── state.py             # LangGraph shared state
│   ├── tools.py             # Academic API + RAG tools
│   ├── graph.py             # Agent graph definition
│   └── nodes/
│       ├── supervisor.py    # Deterministic router
│       ├── search_agent.py  # Parallel search
│       ├── rag_agent.py     # ChromaDB retrieval
│       ├── summarizer_agent.py
│       └── synthesizer_agent.py
└── api/
    └── routes.py            # REST + WebSocket endpoints
```

## 🌐 API Reference

| Endpoint | Method | Description |
|---|---|---|
| `/api/v1/research` | POST | Run full pipeline, return report |
| `/api/v1/ws/research` | WS | Stream agent events in real time |
| `/api/v1/upload-pdf` | POST | Ingest PDF into vector store |
| `/api/v1/vector-store/stats` | GET | ChromaDB chunk count |
| `/api/v1/providers` | GET | Active LLM provider status |
| `/api/v1/health` | GET | Health check |
| `/docs` | GET | Interactive Swagger UI |

## 🚢 Deployment

**Backend (Render free tier):**
```bash
# render.yaml already configured
# Connect GitHub repo → Render will auto-deploy
# Set env vars in Render dashboard
```

**Frontend (Streamlit Cloud):**
```bash
# Push to GitHub
# Go to share.streamlit.io → deploy streamlit_app.py
# Set PAPERMIND_API_URL to your Render URL
```
