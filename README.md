# 📄 Patent Gap Finder

An MCP server that analyzes research papers and identifies **patentable opportunities** before someone else does.

---

## 🚀 Overview

Researchers and developers often create innovative solutions without realizing their patent potential — or worse, reinvent ideas that already exist.

**Patent Gap Finder** solves this by:

* Extracting key contributions from research papers
* Searching global patent databases
* Mapping the patent landscape
* Identifying **white-space opportunities**
* Drafting **USPTO-style patent claims**

All of this happens through a single MCP-powered workflow.

---

## ⚙️ Features

### 🔹 Paper Ingestion

* Parse PDFs (IEEE, ACM, etc.)
* Fetch papers from arXiv
* Extract structured sections
* Deduplicate using SHA-256

### 🔹 AI Understanding

* Convert research into patent-style claims
* IPC/CPC classification
* Generate optimized search keywords
* Session-based tracking

### 🔹 Patent Search

* Multi-source search:

  * USPTO
  * EPO
  * Google Patents (fallback)
* Async job processing with Celery
* Redis caching (7-day TTL)
* Deduplication across sources

### 🔹 (Upcoming)

* Patent landscape clustering
* White-space detection using embeddings
* Automated claim drafting
* PDF report generation

---

## 🏗️ Architecture

```
MCP Client (Claude / Web UI)
        │
        ▼
 FastMCP Server (Python)
        │
 ├── AI (Gemini)
 ├── Patent APIs (USPTO, EPO)
 ├── DB (PostgreSQL)
 ├── Cache (Redis)
 └── Workers (Celery)
```

---

## 🧰 Tech Stack

* **Backend:** Python 3.11+, FastMCP
* **AI:** Gemini 1.5 Flash
* **Database:** PostgreSQL
* **Vector DB:** Qdrant
* **Cache:** Redis
* **Async Jobs:** Celery
* **PDF Parsing:** PyMuPDF, pdfplumber

---

## 📦 Installation

```bash
git clone https://github.com/Rajdeep003/patent-gap-finder.git
cd patent-gap-finder

# Install dependencies
uv sync

# Setup environment
cp .env.example .env
```

---

## 🔧 Configuration

Add required keys in `.env`:

```env
GEMINI_API_KEY=your_key
DATABASE_URL=postgresql+asyncpg://postgres:password@localhost:5432/patent_gap_finder
REDIS_URL=redis://localhost:6379/0
QDRANT_URL=http://localhost:6333
```

Optional:

* EPO OPS credentials
* SerpAPI key

---

## ▶️ Running the Project

### Start services

```bash
docker compose up -d postgres redis qdrant
```

### Initialize DB

```bash
uv run python -m patent_gap_finder.db.init
```

### Start worker

```bash
uv run celery -A patent_gap_finder.workers.celery_app worker --loglevel=info
```

### Run server

```bash
uv run python -m patent_gap_finder.server
```

---

## 🔌 MCP Integration (Claude Desktop)

Add to config:

```json
{
  "mcpServers": {
    "patent-gap-finder": {
      "command": "uv",
      "args": ["run", "python", "-m", "patent_gap_finder.server"]
    }
  }
}
```

---

## 💡 Usage Example

**Input:**

```
Analyze this paper:
https://arxiv.org/abs/2301.07041
```

**Output:**

* Extracted claims
* Patent search results
* White-space opportunities
* Draft patent claims

---

## 📁 Project Structure

```
patent_gap_finder/
├── server.py
├── tools/
├── ai/
├── search/
├── db/
├── workers/
├── cache/
└── tests/
```

---

## 🧪 Testing

```bash
uv run pytest
```

---

## 🚀 Deployment

### Local

```bash
docker compose up
```

### Production

* Deploy on Railway / Docker
* Add PostgreSQL + Redis + Qdrant services

---

## 📊 Free Tier Limits

| Service | Limit         |
| ------- | ------------- |
| Gemini  | 1500 req/day  |
| EPO     | ~2000 req/day |
| SerpAPI | 100/month     |

---

## 🤝 Contributing

1. Fork repo
2. Create branch
3. Add changes + tests
4. Submit PR

---

## 📜 License

MIT License
