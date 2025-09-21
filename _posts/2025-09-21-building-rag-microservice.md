---
title: "Building a Privacy-First RAG Microservice with FastAPI & Open Source Tools"
categories:
  - Backend
  - Microservices
  - Genai
  - RAG
tags:
  - fastapi
  - llm
  - chroma
  - vllm
  - minio
  - huggingface
  - privacy
layout: single
---

> “I didn’t want to just *use* AI — I wanted to *understand* it.”

That’s why I built my own **RAG (Retrieval-Augmented Generation) microservice** from scratch using **FastAPI** and entirely **open-source tools** — no Bedrock, no OpenAI API, no managed services. Just raw, hands-on learning.

This project was born out of curiosity and a desire to peel back the layers of modern LLM-powered applications. I wanted to know: *What’s really happening under the hood?* And more importantly — *how can I keep my data private while still leveraging powerful models?*

---

## 🧰 The Tech Stack

Here’s what I wired together:

- **MinIO** → Self-hosted S3-compatible storage for document uploads
- **Chroma** → Lightweight, open-source vector database for storing embeddings
- **vLLM** → High-throughput LLM inference engine (for future chat integration)
- **Hugging Face Text Embeddings Inference** → Fast, scalable embedding generation
- **FastAPI** → The glue holding it all together with clean, async endpoints

All containerized via Docker and orchestrated with `docker-compose.yml` — because if it’s not reproducible, did it even happen?

---

## 🚀 Endpoints I Built

```bash
POST   /upload      → Upload file → stores in MinIO
DELETE /delete/{filename} → Delete file from MinIO
POST   /embed       → Generate embeddings for ALL files in MinIO
POST   /embed/{filename} → Embed a specific file
POST   /retrieve    → Retrieve top-k relevant chunks for a query
```

Simple? Yes.  
Powerful? Absolutely.

Each endpoint is designed to be modular, testable, and — most importantly — *understandable*. No black boxes.

---

## 🧠 What I Learned (The Hard Way)

Building RAG from scratch revealed surprising complexities:

### 1. File-Embedding Synchronization

What happens when you rename or delete a file? Do you delete its embeddings? How do you track which embeddings belong to which file version? I learned the hard way that **random hashes don’t cut it** — you need **deterministic hashing** (e.g., based on filename + content hash) to maintain consistency.

### 2. The Need for an Embedding Registry

I didn’t realize how crucial it is to maintain a **metadata registry** mapping files → embedding IDs → chunk IDs. Without it, you’re flying blind when files change or models get updated.

### 3. Pipeline Rebuilds Are Inevitable

Switching embedding models? Updating chunking strategies? You need a **full re-embedding pipeline** — with evaluation metrics to measure drift or performance loss. RAG isn’t “set and forget.” It’s a living system.

---

## 🔐 Why Privacy Matters (And Why I Built This)

Let’s be honest: **most LLM services keep your data**. Whether it’s for training, telemetry, or “improving the product,” your documents, queries, and context are rarely truly private.

I built this so I — and you — can:

✅ Use local files  
✅ Avoid sending data to third parties  
✅ Swap models freely (Llama, Mistral, Phi, etc.)  
✅ Own the entire stack — from ingestion to response

This isn’t just a toy project. It’s a **blueprint for private, auditable, enterprise-ready RAG**.

---

## 🗺️ What’s Next?

I’m now integrating **vLLM for chat endpoints** — letting users query their documents and get LLM-generated responses — all locally.

Future plans:

- Experiment with **different RAG architectures** (HyDE, sub-queries, re-ranking)
- Add **evaluation metrics** (hit rate, MRR, faithfulness)
- Support **multiple file types** (PDF, DOCX, PPTX, images via OCR)
- Build a **simple frontend** (maybe with Streamlit or React)

---

## 📁 Sneak Peek: Project Structure

For the curious (and fellow nerds), here’s how I organized the codebase:

```
.
├── Dockerfile
├── docker-compose.yml
├── backend/
│   └── app/
│       ├── routers/          # FastAPI route handlers
│       ├── services/         # Business logic
│       │   ├── embeddings/   # Document loading, chunking, embedding, vector storage
│       │   └── llm/          # Future LLM integration
│       ├── schemas/          # Pydantic models
│       ├── domain/           # Protocols
│       ├── core/             # Config
│       └── utils/            # Helpers (logging, ID generation)
└── uploads/                  # Local file storage (mounted to MinIO)
```

Clean separation of concerns. Testable components. Extensible design.

---

## 💬 Final Thoughts

This project taught me more than any tutorial or course ever could. There’s something deeply satisfying about wiring together open-source tools, debugging tokenizer mismatches, and finally seeing your RAG system return a relevant chunk from your own document.

If you’re thinking about dipping your toes into RAG — **don’t reach for an API first**. Build it yourself. Break it. Fix it. You’ll learn 10x more.

---

🔗 **GitHub Repo**: [github.com/debabrot/intelligent-doc-assistant](https://github.com/debabrot/intelligent-doc-assistant)  

---
