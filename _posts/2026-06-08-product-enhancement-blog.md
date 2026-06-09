---
title: "Building a Product Enrichment Workflow with FastAPI and LangGraph"
categories:
    - AI
    - Python
    - LangGraph
    - FastAPI
tags:
    - langgraph
    - fastapi
    - llm
    - agents
layout: single
---

# Building a Product Enrichment Workflow with FastAPI, LangGraph, and Streamlit

Recently, I built a small proof-of-concept to explore agentic workflows using LangGraph.

The goal was simple: take partially complete product information, combine it with user-provided context, and use an LLM-powered workflow to generate enriched, structured product data.

Rather than building a monolithic prompt, I wanted to experiment with a multi-agent architecture and understand how orchestration frameworks like LangGraph fit into real applications.

## The Problem

Many product datasets contain incomplete information:

* Missing descriptions
* Sparse specifications
* Inconsistent metadata
* Additional context scattered across documents

This POC accepts a product, optional supporting context, and uploaded documents, then generates an enriched product profile using an AI workflow.

## Architecture

The application consists of three main layers:

```text
Streamlit Frontend
        │
        ▼
FastAPI Backend
        │
        ▼
LangGraph Workflow
        │
        ├── Retrieval Agent
        └── Enrichment Agent
        │
        ▼
Structured Product Output
```

### Frontend

The Streamlit UI provides:

* Product selection
* Context input
* Document upload support
* Enrichment results display

### Backend

The FastAPI service exposes a single `/enrich` endpoint and keeps route handlers intentionally thin. Business logic lives inside service and workflow layers.

### Agent Workflow

The enrichment process is orchestrated using LangGraph:

1. Retrieve relevant context
2. Aggregate user inputs
3. Generate enriched product information
4. Return structured output using Pydantic schemas

This separation made it easier to reason about responsibilities and experiment with agent behavior.

## Technology Choices

The project uses:

* Python 3.11
* FastAPI
* Streamlit
* LangGraph
* LangChain
* Pydantic v2
* OpenRouter
* DeepSeek V4 Flash

For observability, I also integrated OpenTelemetry tracing with Jaeger, which made it much easier to understand workflow execution and LLM interactions.

## What I Learned

A few takeaways from building this project:

* LangGraph provides a clean way to model multi-step AI workflows.
* Keeping API handlers thin improves maintainability.
* Structured outputs with Pydantic significantly reduce parsing headaches.
* Tracing becomes extremely valuable once workflows involve multiple agents and LLM calls.
* For small POCs, simple architectures often beat over-engineered solutions.

## Current Limitations

This project intentionally remains lightweight and does not include:

* Authentication
* Persistent storage
* Human-in-the-loop workflows
* Vector databases
* Batch processing
* Multi-user support

The objective was to learn agent orchestration patterns rather than build a production-ready platform.

## What's Next

Some areas I'd like to explore next:

* Evaluation frameworks for enrichment quality
* Human-in-the-loop approval workflows
* Persistence and workflow state management
* Retrieval-augmented enrichment using vector databases

Building this project was a useful exercise in combining modern Python APIs, agent orchestration, structured outputs, and observability into a single workflow.

The complete source code is available on GitHub.
