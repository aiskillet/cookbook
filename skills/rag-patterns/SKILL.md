---
name: rag-patterns
description: Build retrieval-augmented generation that actually grounds answers — chunking, embeddings, retrieval quality, and citations. Use when building or debugging a RAG / knowledge-base Q&A system.
---

# RAG Patterns

RAG is only as good as what you retrieve. Most RAG failures are retrieval failures, not model failures — fix chunking and retrieval before blaming the LLM.

## When to Activate
- Building Q&A over documents / a knowledge base
- A RAG system gives vague, wrong, or ungrounded answers
- Choosing chunking, embedding, or retrieval strategy

## The pipeline
1. **Ingest & chunk** — split docs into retrievable units.
2. **Embed & index** — vector store (+ often keyword index).
3. **Retrieve** — top-k relevant chunks for the query.
4. **Generate** — LLM answers **using only the retrieved context**, with citations.

## Where quality is won or lost
- **Chunking is the biggest lever.** Chunk on semantic boundaries (headings/paragraphs), not fixed character counts that cut mid-sentence. Add slight overlap. Keep chunks focused (too big = noise, too small = lost context).
- **Hybrid retrieval** — combine vector (semantic) + keyword/BM25 (exact terms, names, IDs). Pure vector misses exact matches; pure keyword misses paraphrases.
- **Rerank** the retrieved set with a cross-encoder/reranker — cheap and a big precision boost.
- **Attach metadata** (source, section, date) to chunks for filtering and citations.

## Generation
- **Instruct: answer only from context; if it's not there, say "I don't know."** This is the main hallucination defense.
- **Cite sources** — return which chunks/docs the answer came from; it builds trust and enables verification.
- Put the query + retrieved context clearly delimited in the prompt.

## Evaluate (don't eyeball)
- **Retrieval:** is the right chunk in the top-k? (recall). Fix retrieval first.
- **Answer:** faithful to context (no hallucination) and relevant to the question.
- Build a small eval set of real Q→expected-source pairs; measure changes.

## Checklist
- [ ] Semantic chunking with overlap + metadata
- [ ] Hybrid retrieval (vector + keyword), then rerank
- [ ] LLM answers only from context; says "don't know" otherwise
- [ ] Answers cite their sources
- [ ] Retrieval + faithfulness measured on an eval set
