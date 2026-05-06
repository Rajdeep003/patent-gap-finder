# 📄 Patent Gap Finder

An MCP-based system that analyzes research papers and helps identify **potential patent opportunities** before they are missed.

---

## 🚀 Overview

While working on research or projects, many innovations go unnoticed from a patent perspective — or developers unknowingly build something that already exists.

**Patent Gap Finder** is designed to bridge that gap by:

* Understanding research papers
* Searching existing patents
* Identifying unexplored areas (white-space)
* Suggesting possible patent claims

The entire workflow is automated and can be triggered through an MCP client like Claude Desktop.

---

## ✨ Key Features

### 📥 Research Paper Processing

* Supports PDF and arXiv links
* Extracts structured sections (abstract, methodology, etc.)
* Identifies key technical contributions

### 🤖 AI-Powered Analysis

* Converts research ideas into patent-style claims
* Generates IPC/CPC classifications
* Creates optimized search queries

### 🔍 Patent Search Engine

* Searches across:

  * USPTO
  * EPO
  * Google Patents (fallback)
* Removes duplicate patents
* Uses async processing for faster results

### 📊 (Work in Progress)

* Patent landscape clustering
* White-space detection using embeddings
* Automated patent claim drafting

---

## 🏗️ Architecture Overview

```
MCP Client (Claude Desktop / Web UI)
            │
            ▼
     FastMCP Server (Python)
            │
 ├── AI Layer (Gemini)
 ├── Patent APIs (USPTO, EPO)
 ├── Database (PostgreSQL)
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
* **Queue:** Celery
* **Parsing:** PyMuPDF, pdfplumber

---

## 📦 Installation

```bash
git clone https://github.com/Rajdeep003/patent-gap-finder.git
cd patent-gap-finder

uv sync
cp .env.example .env
```

---

## ⚙️ Configuration

Update your `.env` file:

```env
GEMINI_API_KEY=Your_API
DATABASE_URL=postgresql+asyncpg://postgres:password@localhost:5432/patent_gap_finder
REDIS_URL=redis://localhost:6379/0
QDRANT_URL=http://localhost:6333
```

Optional:

* EPO API credentials
* SerpAPI key

---

## ▶️ Running the Project

Start required services:

```bash
docker compose up -d postgres redis qdrant
```

Initialize database:

```bash
uv run python -m patent_gap_finder.db.init
```

Run Celery worker:

```bash
uv run celery -A patent_gap_finder.workers.celery_app worker --loglevel=info
```

Start the server:

```bash
uv run python -m patent_gap_finder.server
```

---

## 🔌 MCP Setup (Claude Desktop)

Add this to your config file:

```json
{
  "mcpServers": {
    "patent-gap-finder": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/patent-gap-finder",
        "run",
        "python",
        "-m",
        "patent_gap_finder.server"
      ]
    }
  }
}
```

---

## 💡 Example Usage

**Input:**

```
Analyze this paper:
https://arxiv.org/abs/2301.07041
```

**Output:**

* Extracted claims
* Patent search results
* Identified innovation gaps
* Suggested patent claims

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

* Deploy using Docker / Railway
* Add PostgreSQL, Redis, and Qdrant services

---

## 📊 Free Tier Usage

* Gemini: ~1500 requests/day
* EPO: ~2000 requests/day
* SerpAPI: 100/month

---

## 🤝 Contributing

Contributions are welcome!

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Open a pull request

---

## 📜 License

MIT License
Project updated

