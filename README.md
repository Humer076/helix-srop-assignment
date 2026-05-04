# Helix SROP — GenAI Assignment

## Overview

This project implements a Retrieval-Augmented Generation (RAG) based AI backend using FastAPI. It supports session-based conversations, multi-agent routing, and context-aware responses powered by a vector database (ChromaDB).

---

## Features

- RAG pipeline using ChromaDB
- Session-based memory (last messages context)
- Multi-agent routing:
  - Knowledge Agent (RAG-based responses)
  - Account Agent (user/account info)
- FastAPI backend with Swagger UI
- Agent trace logging for observability

---

## Setup Instructions

```bash
git clone <your-repo-link>
cd helix-srop-assignment

python -m venv .venv
.venv\Scripts\activate   # Windows

pip install -r requirements.txt

cp .env.example .env
# Add your GOOGLE_API_KEY

python -m uvicorn app.main:app
```

---

## API Usage

### Swagger UI

http://127.0.0.1:8000/docs

---

### Create Session

POST `/v1/sessions`

```json
{
  "user_id": "demo_user",
  "plan_tier": "pro"
}
```

---

### Chat Endpoint

POST `/v1/chat/{session_id}`

```json
{
  "query": "What is authentication?"
}
```

---

### Example Flow

1. Create a session
2. Ask:
   - "What is authentication?" → handled by Knowledge Agent
   - "What is my plan?" → handled by Account Agent
3. Follow-up:
   - "Explain it simply" → uses session memory

---

## Architecture

```
User → FastAPI → Orchestrator
                    ↓
        ┌───────────────┐
        │               │
Knowledge Agent     Account Agent
(RAG + ChromaDB)   (User Data)
        │
   Retriever → ChromaDB
```

---

## Design Decisions

### State Persistence

Used database-backed session storage with last 2 messages as memory for simplicity and efficiency.

---

### Chunking Strategy

Used fixed-size chunking for document ingestion to balance retrieval accuracy and performance.

---

### Vector Store Choice

ChromaDB was selected because:

- Lightweight and easy to integrate
- Suitable for local development
- Supports persistence

---

## Known Limitations

- Only short-term memory (last 2 messages)
- No reranking of retrieved documents
- No streaming responses
- Basic routing logic (lightweight orchestration)

---

## What I'd Do With More Time

- Add semantic reranking
- Implement streaming responses (SSE)
- Improve long-term memory
- Add authentication and production deployment
- Dockerize the application

---

## Time Spent

| Phase                | Time  |
|---------------------|------|
| Setup + FastAPI + DB | 3 hrs |
| RAG + ChromaDB       | 4 hrs |
| Agents + Routing     | 4 hrs |
| Debugging + Testing  | 3 hrs |
| README + Demo        | 2 hrs |
| Total                | ~16 hrs |

---

## Extensions Completed

- [x] Multi-agent routing
- [x] Session memory
- [ ] Streaming SSE
- [ ] Reranking
- [ ] Guardrails
- [ ] Docker

---

## Demo Video

<your-video-link>

---

## Project Structure

```
app/
 ├── agents/
 ├── api/
 ├── db/
 ├── rag/
 ├── core/
 ├── main.py
```

---

## Summary

This project demonstrates a clean implementation of a RAG-based AI system with session memory and agent-based routing using FastAPI and ChromaDB.