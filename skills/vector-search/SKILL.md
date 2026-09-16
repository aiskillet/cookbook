---
name: vector-search
description: Build semantic/vector search that returns relevant results — embeddings, indexes (ANN), metadata filtering, and hybrid search. Use when adding similarity search or a vector DB.
---

# Vector Search

Vector search finds items by *meaning*, not exact keywords, by comparing embeddings. Powerful, but easy to get mediocre results from — the wins are in the embedding, the index, and combining with keyword search.

## When to Activate
- Adding semantic search, recommendations, or RAG retrieval
- Choosing/tuning a vector database or index
- Search that "misses obvious matches" or returns junk

## The pieces
1. **Embed** text/images into vectors with a model suited to your domain and language. The embedding model quality caps your ceiling — pick well.
2. **Index** vectors for **approximate nearest neighbor (ANN)** search (HNSW, IVF). Exact search doesn't scale; ANN trades a little recall for huge speed. Tune the recall/latency knobs.
3. **Query:** embed the query the *same way*, retrieve top-k by similarity (cosine/dot — match what the model was trained for; normalize if needed).

## Get good results
- **Hybrid search:** combine vector (semantic) with keyword/BM25 (exact terms, names, IDs, codes). Pure vector misses exact matches; pure keyword misses paraphrases. Fuse the scores.
- **Metadata filtering:** store metadata with vectors and filter (tenant, date, type) — both for relevance and access control (never return another user's data).
- **Chunk sensibly** for documents (semantic boundaries + overlap) — same lever as in RAG.
- **Rerank** the top candidates with a cross-encoder for a big precision boost.

## Watch out
- **Distance metric mismatch** with the embedding model → garbage results.
- **Stale index** — re-embed when the model or data changes.
- **Cost/latency** of embedding at query time — cache where possible.

## Checklist
- [ ] Domain-appropriate embedding model; same embedding for query + docs
- [ ] ANN index tuned for recall vs latency
- [ ] Correct similarity metric (normalize if needed)
- [ ] Hybrid (vector + keyword) + metadata filtering
- [ ] Rerank top-k; index kept fresh
