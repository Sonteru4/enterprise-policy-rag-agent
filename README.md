# Enterprise Policy Intelligence Agent

Guardrailed, privacy-first RAG platform for institutional policy documents — upload, analyze, and query internal PDFs without public retrieval.

## Overview

This is a production-style document intelligence system built for schools, nonprofits, and community institutions that need private AI over internal policy files. Users upload PDF or TXT documents, which are chunked, embedded with sentence-transformers, and indexed in a local FAISS vector store. A FastAPI backend exposes upload and query endpoints; a Streamlit frontend provides the chat UI.

The system ships two specialized agents: a **Document Analyzer** that flags outdated content and duplicate chunks into Excel audit reports, and a **Faculty Onboarding Agent** that generates Week 1 checklists and safety quizzes from policy excerpts. Guardrails block sensitive query terms before they reach the LLM.

## Tech Stack

- Python, FastAPI, Uvicorn
- Streamlit (frontend UI)
- LangChain, sentence-transformers, FAISS
- PyPDF2, pandas, openpyxl (Excel reports)
- Docker & Docker Compose
- Optional: OpenAI API or local Ollama LLM

## Features

- `POST /upload` — ingest PDF/TXT files into FAISS index
- `POST /query` — RAG Q&A with agent selection (`doc_analyzer` or `onboarding`)
- `GET /health` — service health check
- **Document Analyzer Agent** — detects outdated chunks (year &lt; 2022), flags exact duplicates, exports Excel report
- **Faculty Onboarding Agent** — generates onboarding checklist and 5-question safety quiz from policy docs
- **Guardrails** — banned-term filtering on user queries
- Dockerized deployment with GitHub Actions CI workflow

## Getting Started

### Prerequisites

- Python 3.8+
- pip
- Docker (optional)

### Installation

```bash
pip install -r requirements.txt
```

### Configuration

Create a `.env` file:

```ini
VECTORSTORE_PATH=./faiss.index
CHUNKS_PATH=./chunks.npy
```

### Run locally

```bash
# Backend (Swagger at http://127.0.0.1:8000/docs)
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload

# Frontend
streamlit run app/streamlit_app.py
```

### Run with Docker

```bash
docker-compose up --build
```

## Project Structure

```
app/
  core/         # agent.py, guardrails.py, retrieval.py, llm.py, langchain_onboarding_agent.py
  routers/      # upload.py, query.py
  reports/      # Excel audit outputs
  main.py       # FastAPI entry point
  streamlit_app.py
uploaded_docs/  # Ingested documents
docker-compose.yml
Dockerfile
```

## Use Case

Forward-deployed document intelligence for regulated or privacy-sensitive environments where teams need RAG over internal policies with audit trails, onboarding automation, and query guardrails — without exposing data to public search.
