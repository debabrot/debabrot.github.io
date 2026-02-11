---
title: "Your Database Schema Is the Real Architecture"
categories:
  - Reliability
  - Microservices
  - System Design
tags:
  - system-design
  - schema-design
layout: single
---

We obsess over algorithms, debate frameworks, and refactor business logic—yet treat our database schema as an afterthought. We create tables to “make it work,” postponing hard decisions until scale forces our hand.

That’s backwards.

**Your schema isn’t just storage—it’s the contract that defines performance, cost, and evolvability.** A thoughtful design compounds value for years; a rushed one becomes technical debt no amount of application optimization can fully undo.

Here’s what that looks like in practice: designing the schema for a product similarity microservice serving real-time “customers also viewed” recommendations.

---

### The Hidden Tradeoffs in Every Schema Decision

When designing a schema, you're making explicit tradeoffs:

| Dimension | What It Means | Why It Matters |
|-----------|---------------|----------------|
| **Read Patterns** | Point lookup, range scan, or joins? | Dictates indexing and whether denormalization pays off |
| **Write Frequency** | Batch updates vs. real-time mutations | Optimize for read speed or write concurrency |
| **Latency Budget** | P99 requirements for critical paths | Joins add latency; denormalization adds storage |
| **Cost Sensitivity** | Storage vs. compute vs. I/O | 10% more storage might save 50% in query costs |
| **Schema Evolution** | How will fields change? | Rigid schemas break deployments; flexible ones sacrifice queryability |
| **Consistency Needs** | Strong vs. eventual | Can you shard freely or need transactional guarantees? |

These aren't academic. They manifest as 3 a.m. pages when your recommendation API spikes to 800ms during Black Friday.

---

### Relational vs. Document: It's Not Binary

Modern workloads often demand **hybrid approaches**:

- **Pure relational** excels for dynamic relationship traversal but joins kill latency in high-throughput reads.
- **Pure document** shines for self-contained aggregates but struggles with cross-document queries and referential integrity.

The sweet spot? **Use relational structure for identity and relationships; embed denormalized data where access patterns are predictable.** You get SQL's query power where you need flexibility and document-style reads where latency is non-negotiable.

---

### Case Study: Product Similarity Schema

For our service—sub-100ms recommendations, <100k products, weekly batch updates—we chose a hybrid PostgreSQL schema that deliberately denormalizes:

```sql
CREATE TABLE product_similarities (
    product_id VARCHAR(50) NOT NULL,
    recommendations JSONB NOT NULL,  -- Denormalized similarity list
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE,
    batch_id VARCHAR(50) NOT NULL,
    PRIMARY KEY (product_id, created_at)
);
```

**Why this works:**

- **Single-row reads:** Entire similarity list in one `JSONB` field. One index lookup (`product_id` + `is_active`)—no joins, no aggregation.
  
- **Versioned updates:** Weekly batches insert new versions instead of overwriting. Old versions stay active until new batch loads—zero downtime, instant rollbacks.
  
- **Operational simplicity:** PostgreSQL gives strong consistency and familiar tooling. No need for DynamoDB or Cassandra when writes are weekly and reads are point lookups.
  
- **Controlled flexibility:** `JSONB` lets us index nested fields later (e.g., by `score`) without schema migrations today.

We accepted tradeoffs: higher storage from versioning and periodic cleanup needs. But these are manageable compared to unpredictable join latency or fragile batch updates.

---

### The Takeaway

Schema design forces you to confront reality: *How will this actually be used?* It's where data models meet production constraints—latency budgets, cost ceilings, inevitable change.

Don't treat your schema as plumbing. Treat it as architecture. The hour spent modeling access patterns today saves weeks of firefighting tomorrow. When your API serves millions of requests per second, the database won't lie about the decisions you made—or deferred—when no one was watching.

---