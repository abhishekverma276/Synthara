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
[ ▶ Click to watch — multi-agent research pipeline in action
](https://drive.google.com/file/d/1EecKOrNrOXNoy__oqW27HIUOjfC2bsCc/view?usp=sharing)

</div>

---

<div align="center">

# 🔬 PaperMind
### Multi-Agent Research Intelligence

**AI-powered academic literature review — search, synthesize, cite, in minutes.**

![Version](https://img.shields.io/badge/version-1.1-gold?style=flat-square)
![Stack](https://img.shields.io/badge/stack-FastAPI%20%2B%20React%20%2B%20LangGraph-blueviolet?style=flat-square)
![LLM](https://img.shields.io/badge/LLM-Ollama%20%7C%20Groq%20%7C%20Gemini-orange?style=flat-square)
![Status](https://img.shields.io/badge/status-private%20beta-green?style=flat-square)

> *PaperMind deploys a coordinated team of AI agents across 7 academic databases, summarizes every relevant paper, and synthesizes a fully cited research report — automatically.*

---

[Features](#-features) · [Architecture](#-architecture) · [Agent Pipeline](#-agent-pipeline) · [Tech Stack](#-tech-stack) · [API](#-api-reference) · [Screenshots](#-screenshots)

</div>

---

## The Problem

Literature reviews are the most time-consuming part of academic research. A researcher manually searching arXiv, PubMed, Semantic Scholar, and cross-referencing citations can spend **days** before writing a single sentence. And when they do write, citations get misattributed — a trust-breaking mistake in academic work.

**PaperMind solves this end-to-end.**

---

## ✨ Features

### Core Research Engine
- **7 academic databases searched in parallel** — arXiv, Semantic Scholar, OpenAlex, PubMed, Europe PMC, Crossref, CORE + Tavily web fallback
- **Multi-agent synthesis** — not a summary of one paper, but a cross-paper synthesis identifying consensus, contradictions, trends, and gaps
- **Structured report sections** — Executive Summary, Methodology Landscape, Key Findings, Contradictions & Debates, Emerging Trends, Limitations, Research Gaps
- **Live token streaming** — report streams word-by-word via WebSocket as the LLM generates it

### Citation Intelligence
- **Clickable inline citations** — every `[N]` in the report links directly to the source paper
- **Citation validation** — backend tracks all reference numbers issued; any `[N]` the LLM invents beyond the reference list is flagged inline as `[N⚠]` with a warning banner
- **Structured reference objects** — every reference carries title, authors, year, URL, and source database

### Citation Graph
- **Interactive D3.js force graph** — visualize how papers cite each other
- **Neighbor highlighting** on hover — instantly see a paper's connections
- **Filter panel** — filter by source database, year range, minimum citation count
- **Year-based color encoding** — plasma gradient from oldest to newest papers
- **Statistics bar** — total nodes, edges, most-cited paper
- **Export PNG** — download the graph as an image
- **Detail panel** — click any node to see abstract, authors, full citation string with one-click copy
- **Zoom controls** — fit-to-view, zoom in/out

### PDF Knowledge Base
- **Upload your own papers** — PDFs are chunked, embedded (Gemini embeddings), and stored in ChromaDB
- **RAG agent** retrieves relevant chunks and injects them into the synthesis alongside web-sourced papers
- **Uploaded papers cited as regular numbered references** — no special status, treated equally

### Sharing & Collaboration
- **One-click share** — generates a public UUID-linked URL for any completed report
- **Read-only shared view** — recipients see the full report with no auth required
- **Export options** — Copy Markdown, Download `.md`, Export PDF, Export Word (`.docx`)

### User Experience
- **Real-time agent terminal** — watch the pipeline execute live with per-agent status, auto-collapses when report is ready
- **Follow-up questions** — LLM generates 5 targeted follow-up research directions after each report
- **Research history** — all sessions saved, searchable, restorable with one click
- **Feedback system** — floating feedback widget with star rating, category, and message; works for anonymous and authenticated users
- **Dark / light theme** — persisted in localStorage
- **Onboarding empty state** — feature cards and example query chips for first-time users

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT (React 18)                    │
│  SearchBar → AgentTerminal → ReportView → CitationGraph     │
│                    WebSocket (live streaming)               │
└───────────────────────┬─────────────────────────────────────┘
                        │ WS + REST
┌───────────────────────▼─────────────────────────────────────┐
│                   FastAPI  (Python 3.11)                    │
│   /api/v1/ws/research    — WebSocket research pipeline      │
│   /api/v1/history        — session CRUD                     │
│   /api/v1/upload-pdf     — PDF ingestion                    │
│   /api/v1/citation-graph — Semantic Scholar graph builder   │
│   /api/v1/share/:token   — public read-only report          │
│   /api/v1/feedback       — anonymous feedback collection    │
│                                                             │
│   SlowAPI rate limiting · JWT auth (Supabase or fallback)   │
└───────────────────────┬─────────────────────────────────────┘
                        │
┌───────────────────────▼─────────────────────────────────────┐
│               LangGraph Research Pipeline                   │
│                                                             │
│  ┌──────────┐    ┌──────────┐    ┌─────────────┐           │
│  │Supervisor│───▶│  Search  │───▶│     RAG     │           │
│  │ (router) │    │  Agent   │    │   Agent     │           │
│  └──────────┘    └──────────┘    └─────────────┘           │
│       ▲               │                 │                   │
│       │               ▼                 ▼                   │
│       │         ┌──────────┐    ┌─────────────┐            │
│       └─────────│Summarizer│───▶│ Synthesizer │──▶ Stream  │
│                 │  Agent   │    │   Agent     │            │
│                 └──────────┘    └─────────────┘            │
└───────────────────────┬─────────────────────────────────────┘
                        │
          ┌─────────────┼──────────────┐
          │             │              │
    ┌─────▼──────┐ ┌────▼──────┐ ┌────▼──────┐
    │  Supabase  │ │ ChromaDB  │ │ LLM Pool  │
    │ (Postgres  │ │ (vectors) │ │ Ollama /  │
    │  + Auth)   │ │           │ │ Groq /    │
    └────────────┘ └───────────┘ │ Gemini    │
                                 └───────────┘
```

---

## 🤖 Agent Pipeline

The pipeline is built with **LangGraph** — a stateful directed graph where a Supervisor node routes between specialized agents based on what work remains.

```
Query
  │
  ▼
Supervisor ──► Search Agent
  ▲                │  Searches: arXiv · Semantic Scholar · OpenAlex
  │                │  PubMed · Europe PMC · Crossref · CORE · Tavily
  │                ▼
Supervisor ──► RAG Agent
  ▲                │  Queries ChromaDB with user-uploaded PDFs
  │                │  Injects relevant chunks into pipeline state
  │                ▼
Supervisor ──► Summarizer Agent
  ▲                │  Summarizes each paper individually
  │                │  Preserves key findings, methods, limitations
  │                ▼
Supervisor ──► Synthesizer Agent
                   │  Cross-paper synthesis (not per-paper summary)
                   │  Builds structured reference list [1..N]
                   │  Validates citation numbers in generated text
                   │  Streams tokens live via WebSocket
                   │  Generates 5 follow-up questions (optional)
                   ▼
              Report + References + Citation Warnings
```

### State management
All agents share a typed `ResearchState` (`LangGraph TypedDict`) that accumulates across nodes:

```python
class ResearchState(TypedDict):
    query:               str
    papers:              list[dict]        # fetched from 7 APIs
    pdf_chunks:          list[dict]        # from ChromaDB RAG
    summaries:           list[str]         # per-paper summaries
    report:              str               # final synthesis
    references:          list[dict]        # structured [{num, title, url, ...}]
    citation_warnings:   list[int]         # hallucinated [N] numbers
    follow_up_questions: list[str]
    want_follow_up:      bool              # user preference
    user_id:             Optional[str]
```

### LLM fallback chain
The LLM layer uses a priority-ordered fallback: **Ollama → Groq → Gemini**. Any rate limit or connection error on the primary provider is caught and the next available provider is tried transparently — no pipeline interruption.

---

## 🛠 Tech Stack

### Backend
| Layer | Technology |
|---|---|
| API framework | FastAPI 0.115 + Uvicorn |
| Agent orchestration | LangGraph 0.2 (stateful directed graph) |
| LLM providers | Ollama (local) · Groq (llama-3.3-70b) · Gemini 2.5 Flash |
| LLM abstraction | LangChain Core + provider adapters |
| Vector store | ChromaDB (persistent, per-user namespaced) |
| Embeddings | Gemini text-embedding-004 |
| Database | Supabase (PostgreSQL + pgvector) |
| Auth | Supabase JWT + fallback HMAC JWT |
| Rate limiting | SlowAPI (per-IP sliding window) |
| Streaming | WebSocket token-level via `asyncio.to_thread` |
| PDF parsing | PyMuPDF + RecursiveCharacterTextSplitter |
| Academic APIs | arXiv · Semantic Scholar · OpenAlex · PubMed (NCBI) · Europe PMC · Crossref · CORE · Tavily |
| Observability | LangSmith tracing (optional) |

### Frontend
| Layer | Technology |
|---|---|
| Framework | React 18 + Vite 5 |
| Markdown rendering | react-markdown + remark-gfm |
| Graph visualization | D3.js v7 (force simulation, zoom, drag, SVG export) |
| PDF export | jsPDF + html2canvas |
| Word export | docx.js (Packer) |
| Auth | Supabase JS client + session storage fallback |
| Styling | CSS custom properties (design tokens), zero CSS frameworks |

---

## 🔑 Key Engineering Decisions

**1. LangGraph over raw chains**
The supervisor-router pattern allows the pipeline to conditionally skip agents (e.g. skip RAG if no PDFs uploaded) and makes the execution graph inspectable and traceable via LangSmith.

**2. Token streaming through WebSocket**
Instead of waiting for the full report, the synthesizer streams each token through a `StreamManager` context variable injected at graph build time. The frontend accumulates tokens in React state — users see the report appear word-by-word.

**3. Citation validation at synthesis time**
The synthesizer builds a numbered reference list *before* calling the LLM, then regex-scans the generated report for `[N]` patterns and diffs them against the known reference set. Invalid citations are surfaced in the API response and rendered visually — not silently passed through.

**4. Per-user vector namespacing**
ChromaDB collections are namespaced by `user_id`, so uploaded PDFs are never cross-contaminated between users. Isolation is enforced at the vector store query layer without relying solely on Supabase RLS.

**5. Share tokens constructed on the frontend**
The share endpoint returns only a UUID token. The frontend constructs `window.location.origin + /share/<token>` — ensuring the link always points to the production frontend, never the API server.

---

## 📡 API Reference

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `WS` | `/api/v1/ws/research` | Optional | Main research pipeline (streaming) |
| `GET` | `/api/v1/providers` | None | Available LLM providers + active |
| `GET` | `/api/v1/history` | Required | User's research sessions |
| `GET` | `/api/v1/history/:id` | Required | Full session report |
| `DELETE` | `/api/v1/history/:id` | Required | Delete session |
| `POST` | `/api/v1/history/:id/share` | Required | Generate public share token |
| `GET` | `/api/v1/share/:token` | None | Fetch shared report (public) |
| `POST` | `/api/v1/upload-pdf` | Required | Ingest PDF into vector store |
| `GET` | `/api/v1/vector-store/stats` | Required | KB chunk count + status |
| `DELETE` | `/api/v1/vector-store/clear` | Required | Clear user's knowledge base |
| `POST` | `/api/v1/citation-graph` | Optional | Build citation graph from papers |
| `POST` | `/api/v1/feedback` | None | Submit user feedback |

### WebSocket message protocol

**Client → Server:**
```json
{ "query": "transformer attention mechanisms", "follow_up": true }
```

**Server → Client (event stream):**
```json
{ "type": "pipeline_start",    "message": "Starting research pipeline..." }
{ "type": "agent_start",       "message": "🔍 Searching 6 academic sources...", "data": { "agent": "search_agent" } }
{ "type": "agent_complete",    "message": "✅ Found 24 papers", "data": { "papers_found": 24 } }
{ "type": "report_token",      "message": "The" }
{ "type": "pipeline_complete", "message": "Done", "data": {
    "report": "...",
    "papers_found": 24,
    "steps_taken": 5,
    "session_id": "uuid",
    "references": [...],
    "citation_warnings": [],
    "follow_up_questions": ["...", "..."]
  }
}
```

**WebSocket close codes:** `4001` session expired · `4029` rate limit reached

---

## 🗄 Database Schema

```sql
-- Research sessions
CREATE TABLE research_sessions (
  id           UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id      UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  query        TEXT NOT NULL,
  report       TEXT,
  papers_found INTEGER DEFAULT 0,
  steps_taken  INTEGER DEFAULT 0,
  share_token  TEXT UNIQUE,
  created_at   TIMESTAMPTZ DEFAULT now()
);

-- User feedback
CREATE TABLE feedback (
  id           UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id      UUID REFERENCES auth.users(id) ON DELETE SET NULL,
  email        TEXT,
  rating       SMALLINT NOT NULL CHECK (rating BETWEEN 1 AND 5),
  category     TEXT,
  message      TEXT NOT NULL,
  page_context TEXT,
  created_at   TIMESTAMPTZ DEFAULT now()
);
```

---

## 🧠 How PaperMind Compares

| Capability | PaperMind | ChatGPT / Perplexity | Elicit | Connected Papers |
|---|---|---|---|---|
| Multi-agent LangGraph pipeline | ✅ | ❌ | ❌ | ❌ |
| 7 academic databases in parallel | ✅ | Partial | Partial | ❌ |
| Citation hallucination detection | ✅ | ❌ | Partial | N/A |
| Live WebSocket token streaming | ✅ | ✅ | ❌ | N/A |
| PDF upload + RAG synthesis | ✅ | ✅ | ❌ | ❌ |
| Interactive D3 citation graph | ✅ | ❌ | ❌ | ✅ |
| Shareable read-only report links | ✅ | ❌ | ❌ | ✅ |
| LLM provider fallback chain | ✅ | ❌ | ❌ | ❌ |
| Self-hostable (Ollama) | ✅ | ❌ | ❌ | ❌ |
| Export PDF / Word / Markdown | ✅ | Partial | Partial | ❌ |

---

## 🔒 Security

- JWT authentication via Supabase (or HMAC fallback for self-hosted deployments)
- Per-IP rate limiting on all endpoints — research pipeline: 3/min, PDF upload: 10/min
- User data isolation: vector store namespaced by `user_id`; session queries filtered server-side
- File validation: PDFs only, 10 MB max, content-type verified server-side
- Share tokens are single-use UUIDs with no expiry (revocable by deleting the session)

---

## 👤 Author

**Abhishek Verma**

Built end-to-end as a full-stack AI systems project — LLM orchestration, multi-agent graph design, vector retrieval, real-time WebSocket streaming, interactive D3 visualization, and production auth/rate-limiting.

---

<div align="center">

*This repository is private. Source code available on request.*

**⭐ Star if you find this impressive.**

</div>
