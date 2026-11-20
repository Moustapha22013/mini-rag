# mini-rag

A minimal RAG (Retrieval-Augmented Generation) application: upload documents, index them in a vector database, and answer questions using an LLM grounded in those documents.

## Tech stack

| Area | Tools |
|------|-------|
| API | FastAPI, Uvicorn |
| Document processing | LangChain, PyMuPDF, NLTK |
| LLM / embeddings | OpenAI, Cohere |
| Vector stores | Qdrant, PostgreSQL + pgvector |
| Databases | MongoDB (Motor), PostgreSQL (SQLAlchemy, asyncpg, Alembic) |
| Background tasks | Celery, Redis, Flower |
| Monitoring | Prometheus client, starlette-exporter, fastapi-health |

## Requirements

- Python 3.12
- [Miniconda](https://docs.anaconda.com/miniconda/) (recommended)
- PostgreSQL client libraries (`libpq`) — needed to build `psycopg2`
- Running services as needed: MongoDB, PostgreSQL, Qdrant, Redis

## Installation

1. Clone the repository:

   ```bash
   git clone git@github.com:Moustapha22013/mini-rag.git
   cd mini-rag
   ```

2. Create and activate the conda environment:

   ```bash
   conda create -n mini-rag python=3.12
   conda activate mini-rag
   ```

3. Install the dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Create a `.env` file in the project root for your settings and API keys (for example `OPENAI_API_KEY`, `COHERE_API_KEY`, database URLs). Never commit this file.

## Running

Start the API server:

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 5000
```

Start a Celery worker (for background tasks):

```bash
celery -A celery_app worker --loglevel=info
```

Open the Flower dashboard to monitor tasks:

```bash
celery -A celery_app flower --port=5555
```

> Adjust `main:app` and `celery_app` to match the module names in the code.

## License

Not yet specified.
