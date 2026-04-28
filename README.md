<div align="center">

<br>

<pre>
███████╗██╗   ██╗███╗   ██╗████████╗██╗  ██╗ █████╗ ██████╗  █████╗
██╔════╝╚██╗ ██╔╝████╗  ██║╚══██╔══╝██║  ██║██╔══██╗██╔══██╗██╔══██╗
███████╗ ╚████╔╝ ██╔██╗ ██║   ██║   ███████║███████║██████╔╝███████║
╚════██║  ╚██╔╝  ██║╚██╗██║   ██║   ██╔══██║██╔══██║██╔══██╗██╔══██║
███████║   ██║   ██║ ╚████║   ██║   ██║  ██║██║  ██║██║  ██║██║  ██║
╚══════╝   ╚═╝   ╚═╝  ╚═══╝   ╚═╝   ╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝
</pre>

### ✦ &nbsp; M U L T I - A G E N T &nbsp; R E S E A R C H &nbsp; I N T E L L I G E N C E &nbsp; ✦

<br>

*Search every major database. Synthesize every paper. Cite everything. In minutes.*

<br>

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev)
[![LangGraph](https://img.shields.io/badge/LangGraph-0.2-7C3AED?style=for-the-badge&logo=langchain&logoColor=white)](https://langchain-ai.github.io/langgraph)

[![Gemini](https://img.shields.io/badge/Gemini_2.5_Flash-LLM-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://deepmind.google/technologies/gemini)
[![Groq](https://img.shields.io/badge/Groq_llama--3.3--70b-LLM-F55036?style=for-the-badge&logo=groq&logoColor=white)](https://groq.com)
[![Ollama](https://img.shields.io/badge/Ollama-Local_LLM-000000?style=for-the-badge&logo=ollama&logoColor=white)](https://ollama.com)

<br>

![Version](https://img.shields.io/badge/version-1.3-B8860B?style=flat-square)&nbsp;
![Status](https://img.shields.io/badge/status-private_beta-22C55E?style=flat-square)&nbsp;
![Databases](https://img.shields.io/badge/databases-7_academic_sources-6366F1?style=flat-square)&nbsp;
![Agents](https://img.shields.io/badge/agents-4_specialized-EC4899?style=flat-square)

<br>

---

[![▶ Watch Live Demo](https://img.shields.io/badge/▶%20WATCH%20LIVE%20DEMO-Google%20Drive-4285F4?style=for-the-badge&logo=googledrive&logoColor=white)](https://drive.google.com/file/d/1rTjv1J9bMLPllicf-6oc3y5q19l4XIrB/view?usp=sharing;)
[![Live App](https://img.shields.io/badge/🌐%20LIVE%20APP-synthara--research.vercel.app-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://synthara-research.vercel.app)

---

<br>

| [✨ Features](#-features) | [🏗 Architecture](#-architecture) | [🤖 Agent Pipeline](#-agent-pipeline) | [🛠 Tech Stack](#-tech-stack) | [📡 API](#-api-reference) |
|:---:|:---:|:---:|:---:|:---:|

<br>

</div>

---

## The Problem

Literature reviews are the most time-consuming part of academic research. A researcher manually searching arXiv, PubMed, Semantic Scholar, and cross-referencing citations can spend **days** before writing a single sentence. And when they do write, citations get misattributed — a trust-breaking mistake in academic work.

**Synthara solves this end-to-end.**

---

## ✨ Features

### Core Research Engine
- **7 academic databases searched in parallel** — arXiv, Semantic Scholar, OpenAlex, PubMed, Europe PMC, Crossref, CORE + Tavily web fallback
- **Multi-agent synthesis** — not a summary of one paper, but a cross-paper synthesis identifying consensus, contradictions, trends, and gaps
- **Structured report sections** — Executive Summary, Methodology Landscape, Key Findings, Contradictions & Debates, Emerging Trends, Limitations, Research Gaps
- **Papers at a Glance table** — every reviewed paper (including uploaded PDFs) listed with author, year, method, key finding, and source database
- **Metrics comparison table** — when 4+ papers report comparable numeric results, an optional table is added to the Methodology Landscape section
- **Live token streaming** — report streams word-by-word via WebSocket as the LLM generates it
- **Streaming fallback** — if the primary LLM stream fails mid-response, a fallback mechanism continues generation without pipeline interruption

### Citation Intelligence
- **Clickable inline citations** — every `[N]` in the report links directly to the source paper
- **Citation validation** — backend tracks all reference numbers issued; any `[N]` the LLM invents beyond the reference list is flagged inline as `[N⚠]` with a warning banner
- **Structured reference objects** — every reference carries title, authors, year, URL, and source database with human-readable labels

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
- **Upload your own papers** — PDFs are chunked, embedded (Gemini embeddings), and stored in Supabase pgvector
- **RAG agent** retrieves relevant chunks and injects them into the synthesis alongside web-sourced papers
- **Uploaded papers cited as regular numbered references** — no special status, treated equally

### Sharing & Collaboration
- **One-click share** — generates a public UUID-linked URL for any completed report
- **Read-only shared view** — recipients see the full report with no auth required
- **Export options** — Copy Markdown, Download `.md`, Export PDF, Export Word (`.docx`)

### User Experience
- **Real-time agent terminal** — watch the pipeline execute live with per-agent status, auto-collapses when report is ready
- **Follow-up questions** — LLM generates 5 targeted follow-up research directions after each report
- **Research history** — up to 50 sessions saved, searchable, restorable with one click; active session persists across page refreshes via localStorage
- **Collapsible sidebar panels** — organized, space-efficient layout with collapsible sections
- **Persistent tab state** — active tab (report, graph, sources) remembered in localStorage between visits
- **Feedback system** — floating feedback widget with star rating, category, and message; shown only to authenticated users
- **Dark / light theme** — persisted in localStorage
- **Onboarding empty state** — feature cards and example query chips for first-time users

### Usage Quota System
- **Per-user daily limits** — Free (3 research / 2 uploads per day), Pro (50 / 20), Enterprise (unlimited)
- **Quota enforced at the API layer** — WebSocket pipeline closes with code 4028 when limit reached; upload endpoint returns HTTP 429
- **Live quota bar in sidebar** — shows used / limit with color-coded progress bar (green → amber → red); resets at midnight UTC
- **Tier badge** — Free / Pro / Enterprise displayed in sidebar
- **Zero-config for new users** — no row needed; defaults to Free tier automatically

### LLM Cost Tracking
- **Per-session cost metering** — every LLM call (summarizer, synthesizer, follow-up) records input/output token counts and calculates USD cost using a provider pricing table
- **Stored per usage log row** — each research session logs its exact API spend to the database
- **GET /api/v1/costs** — returns total spend, average cost per query, and a day-by-day breakdown for any date range
- **Pricing table in core/costs.py** — single file to update when provider prices change (Groq, Gemini, Ollama)

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT (React 18)                    │
│  SearchBar → AgentTerminal → ReportView → CitationGraph     │
│            Sidebar (collapsible, localStorage state)        │
│                    WebSocket (live streaming)               │
└───────────────────────┬─────────────────────────────────────┘
                        │ WS + REST
┌───────────────────────▼─────────────────────────────────────┐
│                   FastAPI  (Python 3.11)                    │
│   /api/v1/ws/research    — WebSocket research pipeline      │
│   /api/v1/history        — session CRUD (50 sessions)       │
│   /api/v1/upload-pdf     — PDF ingestion                    │
│   /api/v1/citation-graph — Semantic Scholar graph builder   │
│   /api/v1/share/:token   — public read-only report          │
│   /api/v1/feedback       — authenticated feedback only      │
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
    ┌─────────────────────────┐ ┌────▼──────┐
    │       Supabase          │ │ LLM Pool  │
    │  Postgres + Auth        │ │ Ollama /  │
    │  pgvector (embeddings)  │ │ Groq /    │
    └─────────────────────────┘ │ Gemini    │
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
  ▲                │  Queries Supabase pgvector with user-uploaded PDFs
  │                │  Injects relevant chunks into pipeline state
  │                ▼
Supervisor ──► Summarizer Agent
  ▲                │  Summarizes each paper individually
  │                │  Preserves key findings, methods, limitations
  │                ▼
Supervisor ──► Synthesizer Agent
                   │  Cross-paper synthesis (not per-paper summary)
                   │  Generates "Papers at a Glance" table
                   │  Generates metrics comparison table (if applicable)
                   │  Builds structured reference list [1..N]
                   │  Validates citation numbers in generated text
                   │  Streams tokens live via WebSocket (with fallback)
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
    pdf_chunks:          list[dict]        # from pgvector RAG
    summaries:           list[str]         # per-paper summaries
    report:              str               # final synthesis
    references:          list[dict]        # structured [{num, title, url, ...}]
    citation_warnings:   list[int]         # hallucinated [N] numbers
    follow_up_questions: list[str]
    want_follow_up:      bool              # user preference
    user_id:             Optional[str]
```

### LLM fallback chain
The LLM layer uses a priority-ordered fallback: **Ollama → Groq → Gemini**. Any rate limit or connection error on the primary provider is caught and the next available provider is tried transparently — no pipeline interruption. A secondary streaming fallback handles mid-stream failures during synthesis.

---

## 🛠 Tech Stack

### Backend
| Layer | Technology |
|---|---|
| API framework | FastAPI 0.115 + Uvicorn |
| Agent orchestration | LangGraph 0.2 (stateful directed graph) |
| LLM providers | Ollama (local) · Groq (llama-3.3-70b) · Gemini 2.5 Flash |
| LLM abstraction | LangChain Core + provider adapters |
| Vector store | Supabase pgvector (per-user namespaced) |
| Embeddings | Gemini embedding-001 |
| Database | Supabase (PostgreSQL + pgvector) |
| Auth | Supabase JWT + fallback HMAC JWT |
| Rate limiting | SlowAPI (per-IP sliding window) |
| Streaming | WebSocket token-level via `asyncio.to_thread` + streaming fallback |
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
| State persistence | localStorage (active tab, session, theme) |

---

## 🔑 Key Engineering Decisions

**1. LangGraph over raw chains**
The supervisor-router pattern allows the pipeline to conditionally skip agents (e.g. skip RAG if no PDFs uploaded) and makes the execution graph inspectable and traceable via LangSmith.

**2. Token streaming through WebSocket**
Instead of waiting for the full report, the synthesizer streams each token through a `StreamManager` context variable injected at graph build time. The frontend accumulates tokens in React state — users see the report appear word-by-word. A streaming fallback ensures completion even if the primary stream drops.

**3. Citation validation at synthesis time**
The synthesizer builds a numbered reference list *before* calling the LLM, then regex-scans the generated report for `[N]` patterns and diffs them against the known reference set. Invalid citations are surfaced in the API response and rendered visually — not silently passed through.

**4. Per-user vector namespacing**
pgvector rows are tagged with `user_id` and all queries are filtered server-side, so uploaded PDFs are never cross-contaminated between users. Isolation is enforced at the query layer without relying solely on Supabase RLS.

**5. Share tokens constructed on the frontend**
The share endpoint returns only a UUID token. The frontend constructs `window.location.origin + /share/<token>` — ensuring the link always points to the production frontend, never the API server.

**6. Session persistence across page refreshes**
Active research session (query, report, references, tab state) is serialized to localStorage on the frontend. On reload, the last session is restored automatically — no re-search required.

---

## 📡 API Reference

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `WS` | `/api/v1/ws/research` | Optional | Main research pipeline (streaming) |
| `GET` | `/api/v1/providers` | None | Available LLM providers + active |
| `GET` | `/api/v1/history` | Required | User's research sessions (up to 50) |
| `GET` | `/api/v1/history/:id` | Required | Full session report |
| `DELETE` | `/api/v1/history/:id` | Required | Delete session |
| `POST` | `/api/v1/history/:id/share` | Required | Generate public share token |
| `GET` | `/api/v1/share/:token` | None | Fetch shared report (public) |
| `POST` | `/api/v1/upload-pdf` | Required | Ingest PDF into vector store |
| `GET` | `/api/v1/vector-store/stats` | Required | KB chunk count + status |
| `DELETE` | `/api/v1/vector-store/clear` | Required | Clear user's knowledge base |
| `POST` | `/api/v1/citation-graph` | Optional | Build citation graph from papers |
| `POST` | `/api/v1/feedback` | Required | Submit user feedback (auth only) |

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

## 🧠 How Synthara Compares

| Capability | Synthara | Consensus | Elicit | Connected Papers |
|---|---|---|---|---|
| Multi-agent synthesis pipeline | ✅ | ❌ | ❌ | ❌ |
| 7+ academic databases searched | ✅ | ✅ | Partial | ❌ |
| Structured long-form research report | ✅ | ❌ | Partial | ❌ |
| Citation hallucination detection | ✅ | ❌ | ❌ | ❌ |
| PDF upload + RAG into synthesis | ✅ | ❌ | ❌ | ❌ |
| Interactive citation graph | ✅ | ❌ | ❌ | ✅ |
| Live token streaming (WebSocket) | ✅ | ❌ | ❌ | ❌ |
| Shareable read-only report links | ✅ | ❌ | ❌ | ✅ |
| Export PDF / Word / Markdown | ✅ | ❌ | Partial | ❌ |
| Follow-up question generation | ✅ | ❌ | ❌ | ❌ |
| Free to use | ✅ | Partial | Partial | ✅ |

---

## 🔒 Security

- JWT authentication via Supabase (or HMAC fallback for self-hosted deployments)
- Per-IP rate limiting on all endpoints — research pipeline: 3/min, PDF upload: 10/min
- User data isolation: vector store namespaced by `user_id`; session queries filtered server-side
- File validation: PDFs only, 10 MB max, content-type verified server-side
- Share tokens are single-use UUIDs with no expiry (revocable by deleting the session)
- Feedback submission requires authentication — no anonymous abuse vector

---

## 👤 Author

**Abhishek Verma**

Built end-to-end as a full-stack AI systems project — LLM orchestration, multi-agent graph design, vector retrieval, real-time WebSocket streaming, interactive D3 visualization, and production auth/rate-limiting.

---

<div align="center">

*This repository is private. Source code available on request.*

**⭐ Star if you find this impressive.**

</div>
