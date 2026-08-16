# Document RAG Assistant

A command-line retrieval-augmented generation application that answers questions using a local document collection. It creates document embeddings with Sentence Transformers, stores them in ChromaDB, retrieves relevant passages, and sends grounded context to a Hugging Face-hosted language model.

## Features

- Ingests `.txt` and `.pdf` documents
- Splits documents into overlapping chunks
- Generates normalized embeddings with `all-MiniLM-L6-v2`
- Persists vectors and source metadata in ChromaDB
- Retrieves the three most relevant chunks for each question
- Constrains responses to retrieved context
- Displays the source documents used for an answer

## Technology

- Python
- Hugging Face Inference API
- Sentence Transformers
- ChromaDB
- PyPDF

## Setup

Create and activate a virtual environment, then install the dependencies:

```bash
python -m venv .venv
python -m pip install -r requirements.txt
```

Place `.txt` or `.pdf` files in `documents_project4/`, then build the local vector index:

```bash
python ingest.py
```

Start the assistant:

```bash
python main.py
```

The application accepts a Hugging Face token securely at runtime. Alternatively, define `HF_TOKEN` or `HUGGINGFACEHUB_API_TOKEN` in your environment. Never commit a token to the repository.

The default hosted model can be replaced at the prompt if it is unavailable to your Hugging Face account or inference provider.

## Project structure

```text
.
|-- documents_project4/  Source documents
|-- ingest.py            Document loading, chunking, embedding, and indexing
|-- rag_chat.py          Retrieval and language-model interaction
|-- main.py              Application entry point
`-- requirements.txt     Python dependencies
```

## Grounding behavior

The assistant is instructed to answer only from retrieved context. When the answer is not present, it returns a fixed fallback response instead of intentionally using outside knowledge. This reduces unsupported answers but does not guarantee factual accuracy; users should verify responses against the displayed sources.

## Data attribution

The included demonstration documents summarize public information about Sensata Technologies. Review `documents_project4/urls.txt` for the source URLs and verify redistribution requirements before publishing the document collection.

## Suggested evaluation

Before treating the assistant as production-ready, create a test set containing representative questions, expected source documents, and expected answer facts. Measure retrieval relevance, grounded-answer accuracy, fallback accuracy, latency, and failure behavior.
