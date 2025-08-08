---
created: 2025-06-26
tags:
  - AI
  - LLM
  - RAG
  - VectorDatabase
  - SemanticSearch
  - SystemDesign
---

# RAG, Vector Databases, and Semantic Search

This note explains how Retrieval-Augmented Generation (RAG) uses vector databases and semantic search to provide relevant, context-aware answers from large datasets.

---

## What is Retrieval-Augmented Generation (RAG)?

**Retrieval-Augmented Generation (RAG)** is an AI framework for improving the quality of responses from Large Language Models (LLMs). Instead of relying solely on its pre-trained (and potentially outdated) internal knowledge, the LLM is given access to an external, up-to-date knowledge base.

The core idea is to first **retrieve** relevant information from your documents and then pass that information as context to the LLM to **generate** a more accurate and fact-based answer.

**Analogy:** It's the difference between a closed-book exam (standard LLM) and an open-book exam (RAG).

### Key Benefits of RAG
- **Reduces Hallucinations:** The model's answers are grounded in the provided text, making it less likely to invent facts.
- **Uses Current Data:** Allows LLMs to answer questions about information created after their training cut-off date.
- **Enables Private Data Usage:** You can use RAG on your own private documents without needing to retrain the entire model.
- **Provides Source Attribution:** Because you know which documents were retrieved, you can cite the sources used to generate the answer.

---

## The Core of Retrieval: Semantic Search

Traditional search methods (like `Ctrl+F`) are based on **keyword matching**. They find documents that contain the exact words from your query.

**Semantic Search**, on the other hand, searches based on **meaning, intent, and context**. It uses numerical representations of text, called **embeddings**, to understand the underlying concepts.

**Example:**
- A keyword search for "ways to make a car go faster" might miss a document titled "Improving Vehicle Performance."
- A semantic search would understand that these phrases are conceptually similar and would retrieve the relevant document.

---

## The Engine Room: Vector Databases

A **Vector Database** is a specialized database designed to store, manage, and query high-dimensional vectors, which are the embeddings created from your data.

### How They Work

1.  **Ingestion (Storing Data):**
    -   Your raw data (text documents, images, etc.) is broken down into manageable chunks.
    -   An **embedding model** (e.g., OpenAI's `text-embedding-ada-002`, `all-MiniLM-L6-v2`) converts each chunk into a vector (a long list of numbers).
    -   The vector database stores these vectors, often with a reference back to the original chunk of text.

2.  **Querying (Retrieving Data):**
    -   When you ask a question (the "query"), it is converted into a vector using the **exact same embedding model**.
    -   The database then performs a search to find the vectors in its storage that are "closest" or most similar to the query vector.

---

## Retrieval Algorithms: Finding the "Closest" Data

To find the most relevant data, RAG systems need to measure the "closeness" of the query vector to the vectors in the database. This is done using similarity metrics and efficient search algorithms.

### 1. Similarity Metrics

These mathematical functions calculate the distance or similarity between two vectors.
-   **Cosine Similarity (Most Common):** Measures the cosine of the angle between two vectors. It focuses on the orientation of the vectors, not their magnitude. A value closer to 1 means higher similarity. This is the standard for text embeddings.
-   **Euclidean Distance:** The straight-line "ruler" distance between the two points the vectors represent in space. A smaller distance means higher similarity.
-   **Dot Product:** A simpler calculation that is very similar to Cosine Similarity for normalized vectors (vectors with a length of 1).

### 2. Search Algorithms

Calculating the similarity score against every single vector in a database (brute-force or "exact nearest neighbor" search) is too slow for large datasets. Instead, vector databases use **Approximate Nearest Neighbor (ANN)** algorithms to find "good enough" matches very quickly.

ANN algorithms trade a tiny amount of accuracy for a massive gain in speed. Some of the most common algorithms used are:

-   **HNSW (Hierarchical Navigable Small World):** A graph-based algorithm that is currently one of the most popular due to its high speed and accuracy. It creates a multi-layered graph of vectors, allowing for efficient searching from a general "entry point" down to the most specific neighbors.
-   **IVF (Inverted File Index):** This algorithm first clusters the vectors into groups. When a query comes in, it only searches within the most promising cluster(s), drastically reducing the number of vectors to compare. A common variant is `IVF-PQ` (Inverted File with Product Quantization).
-   **ScaNN (Scalable Nearest Neighbors):** An algorithm developed by Google that uses a combination of techniques, including vector quantization, to achieve very fast and accurate results.
-   **LSH (Locality-Sensitive Hashing):** An older technique that uses special hash functions to group similar vectors together into the same "buckets."

---

## Putting It All Together: The RAG Workflow

1.  **Indexing (Offline Step):** Your documents are chunked, converted to embeddings, and stored in a vector database using an algorithm like HNSW.
2.  **User Query:** A user asks a question.
3.  **Query Embedding:** The user's question is converted into a query vector.
4.  **Semantic Search (Retrieval):** The vector database uses its ANN algorithm (e.g., HNSW) and a similarity metric (e.g., Cosine Similarity) to find the top `k` most relevant document chunks.
5.  **Context Augmentation:** The original query and the retrieved text chunks are combined into a single, expanded prompt.
6.  **Generation:** This expanded prompt is sent to the LLM.
7.  **Final Answer:** The LLM uses the rich context from the retrieved documents to generate a relevant, detailed, and fact-grounded answer.
