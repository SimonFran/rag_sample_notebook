# rag-sample-notebook

A minimal Retrieval-Augmented Generation (RAG) pipeline using local models and a PostgreSQL vector store.

## What it does

1. **Ingest** — Downloads the [SQuAD validation dataset](https://huggingface.co/datasets/rajpurkar/squad) from Hugging Face, splits it into overlapping chunks, embeds them with [Ollama](https://ollama.com/) (`nomic-embed-text`), and stores the vectors in PostgreSQL via `pgvector`.
2. **Query** — Retrieves the top-10 most relevant chunks for a question and passes them as context to a local LLM (`qwen2.5:32b` via Ollama) to produce a grounded answer.

## Stack

- **Embeddings & LLM**: Ollama (local, no API key required)
- **Vector store**: PostgreSQL + pgvector (via `langchain_postgres`)
- **Orchestration**: LangChain
- **Infrastructure**: Docker Compose (spins up the Postgres/pgvector container)

## Getting started

```bash
# Install dependencies
pip install -r requirements.txt

# Start the vector database
docker compose up -d

# Set the database connection string
cp .env.example .env  # then edit DATABASE_URL if needed

# Open the notebook
jupyter notebook general_knowledge_rag.ipynb
```

Make sure Ollama is running locally with the required models pulled:

```bash
ollama pull nomic-embed-text
ollama pull qwen2.5:32b
```
