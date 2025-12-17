# Scope-Of-Work-Document-RAG-Project

Short RAG Project Report (Markdown, ½–1 page)

Corpus Description
For my RAG system, I used the KTL Switchgear Upgrade Project – Scope of Work Document (015-0000-AAA-SOW-20014-01). This document is 352 pages and contains all contractual, mechanical, electrical, and instrumentation responsibilities for KTL-1 and KTL-2.

I chose this corpus because:

I am currently working on this project.

Engineers frequently need to quickly extract information from long SOW documents.

It is a realistic use case for retrieval-augmented generation.

The document was chunked into ~110 FAISS vectors, each chunk 500–800 characters, ensuring that related sentences remain together for better retrieval.

Retrieval Method
I used FAISS (Facebook AI Similarity Search) for vector similarity retrieval.

Embedding model: sentence-transformers/all-MiniLM-L6-v2 (Chosen for speed and good performance on technical text.)

Retrieval process:

Each chunk of text is embedded into a vector.

At query time, the question is embedded.

FAISS returns the top-k most relevant chunks.

Chunks are concatenated and passed to the LLM.

RAG Pipeline Overview (High-Level)
The system uses the classic RAG architecture:

User Question ↓ Embed Question → Retrieve Top-k Chunks → Concatenate Context ↓ LLM Prompt: [Question + Retrieved Context] ↓ Model Generates Answer

LLM used: google/flan-t5-xl for slow but high-quality instruction following.

Why FLAN-T5-XL?

Strong performance on extraction & summarization tasks.

Works well even with noisy technical text.

Does not hallucinate as much as GPT-style models when given context.

Limitations / Problems Observed
Missing Information: The SOW document does not contain marshalling cabinet quantities or tag numbers, so the RAG model cannot answer those questions (“Not found in document”).

Fragmented Content: Technical PDFs contain tables, figures, and cross-references that do not extract cleanly. Some drawings and tables became incomplete during extraction.

LLM Sensitivity: FLAN-T5-XL performs well but is slow on CPU and sometimes responds “Not found” even when partial information exists in distant chunks.

Retrieval Noise: Some chunks include multiple unrelated items, reducing answer precision.

Despite these issues, the RAG system successfully answers scope-related questions and retrieves useful engineering information.
