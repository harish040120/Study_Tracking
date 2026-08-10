# DocuTalk — AI PDF Chatbot
## Interview Handbook (Local RAG Implementation)

---

## 1. Tech Stack (as currently implemented — local only)

| Layer | Technology |
|---|---|
| UI / Chat Interface | Chainlit |
| Orchestration | LangChain |
| LLM (Local Inference) | Ollama running **Qwen** model |
| Embeddings | **Nomic Embed** (local embedding model) |
| Vector Store | ChromaDB |
| Language | Python |
| Deployment | Runs **entirely locally** — no cloud/HF deployment yet |

> Note for interviews: The project currently runs fully on local infrastructure (Ollama for the LLM, local embeddings, local ChromaDB instance). Cloud/Hugging Face Space deployment is a planned next step, not yet completed — be upfront about this if asked.

---

## 2. Quick Structural Overview (kept brief, since you already know the architecture)

1. **User uploads PDF(s)** via the Chainlit interface.
2. **Document loading + chunking** — LangChain splits the PDF text into manageable chunks.
3. **Embedding** — each chunk is converted into a vector using the **Nomic embedding model** (run locally).
4. **Storage** — chunk vectors are stored in **ChromaDB** (local vector database).
5. **Query time** — user asks a question → question is embedded → ChromaDB performs a similarity search → most relevant chunks are retrieved.
6. **Generation** — retrieved chunks + question are passed as context to the **Qwen model via Ollama**, which generates the final answer.
7. **Response + sources** — Chainlit displays the answer along with the source chunks used.

---

## 3. Interview Q&A (30 Questions)

### 🟢 Core / Basic RAG Questions (Q1–Q10)

**Q1: What is RAG, and why did you use it instead of just prompting the LLM directly?**
> A: RAG (Retrieval-Augmented Generation) means the model doesn't rely purely on what it learned during training — instead, at query time, we first *retrieve* the most relevant real content from the uploaded PDF, and then feed that content to the LLM as context so it generates an answer grounded in the actual document. This avoids hallucination and lets the system work on documents the model has never seen, without any fine-tuning.

**Q2: Walk me through what happens the moment a user uploads a PDF.**
> A: The PDF is loaded and parsed into raw text using a LangChain document loader, then split into smaller overlapping chunks (since LLMs have context limits and smaller chunks give more precise retrieval). Each chunk is passed through the Nomic embedding model to produce a vector, and all vectors are stored in ChromaDB, ready to be searched once the user asks a question.

**Q3: Why do you chunk the document instead of embedding the whole PDF as one block?**
> A: A whole PDF embedded as a single vector would be too coarse — it would blur together many unrelated topics into one vector, making retrieval imprecise. Chunking lets each vector represent a focused, specific piece of content, so when a question is asked, we can retrieve just the passages that are actually relevant instead of the entire document.

**Q4: What is an embedding, in simple terms?**
> A: An embedding is a way of converting text into a list of numbers (a vector) that captures its meaning — texts with similar meaning end up with vectors that are close together in that numeric space. This lets us compare a question to document chunks mathematically (via distance/similarity) instead of just matching keywords.

**Q5: Why did you choose Nomic Embed for the embedding model?**
> A: Nomic Embed is a strong, open-source embedding model that runs efficiently locally without needing a paid API — since the whole project runs offline via Ollama, using a local embedding model keeps the entire pipeline (embedding + generation) self-contained with no external dependency or cost.

**Q6: Why Ollama and specifically the Qwen model for generation?**
> A: Ollama lets me run open LLMs locally with a simple interface, without needing an API key or internet connection during inference — good for privacy and zero-cost development. Qwen is a capable open-weight chat model that performs well on instruction-following and Q&A tasks, making it a solid choice for generating grounded answers from retrieved context.

**Q7: What is ChromaDB, and why did you pick it over other vector databases?**
> A: ChromaDB is a lightweight, easy-to-set-up vector database that's great for local development — it stores embeddings and supports fast similarity search without needing external infrastructure like a hosted vector DB service (e.g., Pinecone). Since the whole project runs locally right now, Chroma fits naturally without adding deployment complexity.

**Q8: How does the retrieval step actually decide which chunks are "relevant"?**
> A: The user's question is embedded using the same Nomic model used for the documents (this consistency matters — you must embed queries and documents with the same model so they live in the same vector space). ChromaDB then performs a similarity search — typically using cosine similarity or a related distance metric — and returns the top-k chunks whose vectors are closest to the question's vector.

**Q9: What role does LangChain play specifically in this project?**
> A: LangChain acts as the orchestration layer — it handles document loading, text splitting, wiring the embedding model to the vector store, managing the retrieval step, and constructing the final prompt that combines retrieved context with the user's question before sending it to the LLM. It abstracts away a lot of the "glue code" you'd otherwise have to write by hand.

**Q10: How does the app handle multiple PDFs uploaded at once?**
> A: All chunks from every uploaded PDF are embedded and stored together in the same ChromaDB collection, tagged with metadata identifying which file (and often which page) they came from. At retrieval time, the search isn't restricted to a single file — it pulls the most relevant chunks across all uploaded documents, and the source metadata is used afterward to tell the user exactly which document an answer came from.

---

### 🟡 Design Decisions & Trade-offs (Q11–Q18)

**Q11: What happens if the retrieved chunks don't actually contain the answer to the question?**
> A: This is a known limitation of basic RAG — if no relevant chunk exists, the LLM may either say it doesn't know (ideal, if the prompt is instructed to do so) or, worse, hallucinate an answer using its own general knowledge instead of the document. A good practice (and something worth improving) is explicitly instructing the prompt to only answer from the provided context and to say "I don't know" if the context is insufficient.

**Q12: How do you decide the chunk size and overlap, and why does overlap matter?**
> A: Chunk size is a trade-off — too small and you lose surrounding context needed to understand a passage; too large and retrieval becomes imprecise and you waste context window space on irrelevant text. Overlap (e.g., 50–100 tokens shared between consecutive chunks) prevents an answer-relevant sentence from being awkwardly split across two chunks and losing coherence in either one.

**Q13: Since everything runs locally, what are the trade-offs compared to using a cloud-hosted LLM?**
> A: Running locally with Ollama means zero API cost, full data privacy (nothing leaves the machine — important for sensitive documents), and no rate limits — but the trade-off is being constrained by local hardware (slower inference than a hosted GPU cluster, and a smaller/less capable model than something like GPT-4). For a personal project and demo purposes, that trade-off favors local for cost and simplicity.

**Q14: How do you maintain conversational memory across multiple questions?**
> A: Chainlit and LangChain support maintaining a session-level history of previous questions and answers, which can either be included directly in the prompt (short conversations) or summarized/condensed for longer ones. This lets follow-up questions like "what about the next section?" resolve correctly using prior context rather than being treated as a completely isolated query.

**Q15: What's the difference between "stuffing" retrieved chunks into the prompt versus other retrieval strategies?**
> A: "Stuffing" is the simplest approach — concatenate all retrieved chunks directly into the prompt as context. It works well for a small number of highly relevant chunks but breaks down if there's too much retrieved content to fit the context window. More advanced strategies (Map-Reduce, Refine, Map-Rerank) process chunks separately and combine partial answers — useful for larger retrieval sets, though "stuffing" is what a straightforward implementation like this uses.

**Q16: How would you evaluate whether your RAG pipeline is actually giving good answers?**
> A: Beyond manual spot-checking, a more rigorous approach involves separately evaluating **retrieval quality** (did we fetch the right chunks — measurable via precision/recall against a labeled test set of question-chunk pairs) and **generation quality** (given the right chunks, did the LLM produce a correct, non-hallucinated answer — often assessed with frameworks like RAGAS, or LLM-as-judge scoring). I haven't built formal evaluation into this project yet, but that would be the next step for production-readiness.

**Q17: Why use source citation, and how is it implemented?**
> A: Source citation builds user trust — instead of a black-box answer, the user can verify which part of the document backs a claim. It's implemented by carrying metadata (filename, chunk index/page) alongside each embedding in ChromaDB, so when a chunk is retrieved and used in the answer, that metadata is surfaced back to the UI alongside the generated response.

**Q18: What would break if someone uploaded a scanned (image-based) PDF instead of a text PDF?**
> A: A standard PDF text loader would extract little to no usable text from a scanned/image-only PDF, since there's no embedded text layer to parse — the pipeline would need an OCR step (e.g., Tesseract) inserted before chunking to convert the scanned images into text first. This is a real limitation of the current implementation that's worth acknowledging.

---

### 🔴 Advanced RAG Concepts (Q19–Q25)

**Q19: This is a fairly basic RAG setup. What's "Advanced RAG," and what would you add to move toward it?**
> A: Basic RAG is a single retrieve-then-generate pass. Advanced RAG adds intelligence *before*, *during*, and *after* that pass — things like query rewriting/expansion, hybrid search (combining keyword + vector search), re-ranking retrieved results with a cross-encoder, and self-correction loops where the model checks its own answer against the retrieved context before finalizing. I'd frame my current project as a solid baseline RAG implementation, with these as clear, well-understood next steps.

**Q20: What is "hybrid search," and why might it outperform pure vector similarity search?**
> A: Hybrid search combines dense vector search (semantic similarity) with traditional sparse keyword search (like BM25). Vector search is great for capturing meaning but can miss exact-match terms (like a specific product code, acronym, or rare proper noun) that a keyword search would catch directly. Combining both and merging/re-ranking the results tends to outperform either method alone, especially on domain-specific documents.

**Q21: What is re-ranking, and where would it fit in this pipeline?**
> A: Re-ranking is a second-pass refinement step — after an initial (fast but coarser) retrieval pulls, say, the top 20 candidate chunks via vector similarity, a more computationally expensive but more accurate model (often a cross-encoder, which jointly processes the query and each chunk together rather than comparing precomputed vectors) re-scores and reorders those 20 down to the best 3–5 before they're sent to the LLM. It would sit between the ChromaDB retrieval step and the final prompt construction.

**Q22: What is query rewriting/expansion, and why is it useful?**
> A: Users often ask short, ambiguous, or poorly-phrased questions that don't retrieve well as-is. Query rewriting uses the LLM itself (or a smaller model) to reformulate the user's question into one or more better-optimized search queries before hitting the vector store — for example, expanding "what about the pricing?" (ambiguous without context) into a fuller standalone question using the conversation history.

**Q23: What is HyDE (Hypothetical Document Embeddings), and how does it relate to retrieval quality?**
> A: HyDE is a technique where, instead of embedding the raw user question directly, the LLM first generates a hypothetical *answer* to the question, and that hypothetical answer is embedded and used for the similarity search instead. The idea is that a plausible-sounding answer is often semantically closer to the real relevant document chunks than a short question is — improving retrieval, particularly for terse or vague queries.

**Q24: What's the difference between a vector database's "similarity search" and "MMR" (Maximal Marginal Relevance) retrieval?**
> A: Plain similarity search just returns the top-k closest chunks to the query — which can result in redundant chunks that all say roughly the same thing. MMR retrieval balances relevance *and* diversity, penalizing chunks that are too similar to ones already selected, so the final retrieved set covers more distinct angles/information relevant to the query rather than repeating the same point five times.

**Q25: How would you handle a very large knowledge base (e.g., thousands of PDFs) differently from your current single-session setup?**
> A: At that scale, a few things change: (1) you'd likely need a more scalable, persistent, and possibly distributed vector store rather than an in-memory/local Chroma instance, (2) metadata filtering becomes important to narrow the search space before doing vector similarity (e.g., filter by document category/date first), and (3) you'd want approximate nearest neighbor (ANN) indexing (like HNSW, which most production vector DBs use under the hood) to keep search fast as the dataset grows, since exact nearest-neighbor search doesn't scale linearly to millions of vectors.

---

### 🟣 LangChain / LangGraph Concepts (Q26–Q30)

**Q26: What LangChain components does your project rely on, conceptually?**
> A: Document loaders (to parse the PDF), a text splitter (to chunk the content), an embeddings wrapper (interfacing with the local Nomic model), a vector store wrapper (interfacing with ChromaDB), a retriever (which wraps the vector store's similarity search into a standard interface), and a chain that combines the retriever's output with a prompt template before calling the LLM (Qwen via Ollama).

**Q27: What is a "chain" in LangChain, conceptually?**
> A: A chain is a composable sequence of steps — each step takes an input, does something (transform text, call a model, query a database), and passes its output to the next step. A basic RAG chain, for example, is: take a question → retrieve relevant docs → format a prompt with the question + docs → call the LLM → return the answer. LangChain lets you build and reuse these as modular, chainable units (especially with the newer LCEL — LangChain Expression Language — syntax using the `|` pipe operator).

**Q28: What is LangGraph, and how does it differ from a standard LangChain chain?**
> A: A standard LangChain chain is essentially a linear (or simple branching) sequence of steps. LangGraph is built for more complex, **stateful, and cyclical** workflows — modeled as a graph of nodes (each doing some work) and edges (defining transitions, including conditional ones), which lets you build things like agents that loop, re-try, or make decisions about what to do next based on intermediate results — something a simple linear chain can't naturally express.

**Q29: How might LangGraph be useful for evolving DocuTalk beyond its current basic RAG setup?**
> A: A LangGraph-based version could add a decision loop — for example: retrieve chunks → have the LLM self-assess whether the retrieved context is actually sufficient to answer confidently → if not, trigger a query-rewrite node and retry retrieval → only proceed to final answer generation once the context is judged adequate (or after a retry limit is hit, gracefully say "I don't know"). This kind of conditional, cyclical "retrieve-and-check" loop is exactly the pattern LangGraph is designed for, versus a single-pass chain.

**Q30: What is "agentic RAG," and how does it relate to LangGraph?**
> A: Agentic RAG treats retrieval as one *tool* among several that an LLM-driven agent can choose to call, rather than a fixed, always-executed step. The agent can reason about what it needs (e.g., "should I search the documents, or is this a general knowledge question I can answer directly, or do I need to search multiple times with different queries?"), and LangGraph is a natural fit for implementing this because it can model the agent's reasoning loop (think → act → observe → think again) as a graph with cycles, rather than forcing a rigid one-shot pipeline.

---

## 4. Small Overview — Advanced RAG Concepts (Quick Reference)

Use this as a fast recap right before the interview — you don't need to have *built* these, just be able to explain them if asked "what would you add next?"

| Concept | One-Line Explanation |
|---|---|
| **Hybrid Search** | Combine vector (semantic) search with keyword (BM25) search for better coverage of exact terms. |
| **Re-ranking** | Use a more accurate (but slower) cross-encoder model to re-score and reorder an initial set of retrieved chunks. |
| **Query Rewriting / Expansion** | Reformulate a vague user question into a clearer, better search query before retrieval. |
| **HyDE** | Embed a hypothetical LLM-generated answer instead of the raw question, to improve retrieval matching. |
| **MMR (Maximal Marginal Relevance)** | Retrieve chunks that are both relevant *and* diverse, avoiding redundant near-duplicate results. |
| **Self-RAG / Corrective RAG** | The model checks whether retrieved context is sufficient before answering, and retries retrieval if not. |
| **Agentic RAG** | Treat retrieval as a tool the LLM can choose to invoke (possibly multiple times, with different queries) rather than a fixed pipeline step. |
| **Multi-hop Retrieval** | Chain multiple retrieval steps together for questions that require connecting information across several documents/chunks. |
| **Metadata Filtering** | Narrow the vector search space first using structured filters (date, document type, author) before doing similarity search. |
| **ANN Indexing (e.g., HNSW)** | Approximate nearest-neighbor search structures that keep vector search fast at large scale, instead of exact brute-force comparison. |
| **LangGraph (stateful orchestration)** | Model complex, cyclical, decision-driven workflows (retry loops, agent reasoning) as a graph instead of a single linear chain. |

---

## 5. One-Line Elevator Pitch (memorize this)

> "DocuTalk is a fully local RAG-based PDF chatbot — I use LangChain to chunk and orchestrate the pipeline, Nomic embeddings to vectorize document content, ChromaDB as the vector store, and Ollama running the Qwen model for generation, all wired together through a Chainlit interface with source citations. It's currently a clean baseline RAG implementation running entirely offline, and I'm familiar with the advanced techniques — hybrid search, re-ranking, query rewriting, and LangGraph-based agentic retrieval loops — that would be the natural next steps to make it production-grade."
