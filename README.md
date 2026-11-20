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

4. Create your environment file from the template, then fill in your own values:

   ```bash
   cp .env.example .env
   ```

## Configuration

All settings are read from `.env` in the project root. The `.env` file holds secrets, so it is listed in `.gitignore` and never committed. Only `.env.example` (with empty values) is kept in the repository.

| Variable(s) | Description |
|-------------|-------------|
| `APP_NAME`, `APP_VERSION` | Application name and version |
| `FILE_ALLOWED_TYPES`, `FILE_MAX_SIZE`, `FILE_DEFAULT_CHUNK_SIZE` | Accepted upload types, maximum size (MB), and read chunk size (bytes) |
| `MONGODB_URL`, `MONGODB_DATABASE` | MongoDB connection |
| `POSTGRES_USERNAME`, `POSTGRES_PASSWORD`, `POSTGRES_HOST`, `POSTGRES_PORT`, `POSTGRES_MAIN_DATABASE` | PostgreSQL connection |
| `OPENAI_API_KEY`, `OPENAI_API_URL` | OpenAI key; set the URL only for an OpenAI-compatible server (e.g. Ollama) |
| `COHERE_API_KEY` | Cohere key |
| `GENERATION_BACKEND`, `GENERATION_MODEL_ID` | Provider and model used to generate answers |
| `EMBEDDING_BACKEND`, `EMBEDDING_MODEL_ID`, `EMBEDDING_MODEL_SIZE` | Provider, model, and vector size used for embeddings |
| `VECTOR_DB_BACKEND`, `VECTOR_DB_PATH`, `VECTOR_DB_DISTANCE_METHOD` | Vector store (`QDRANT` or `PGVECTOR`), local path, and similarity metric |
| `CELERY_BROKER_URL`, `CELERY_RESULT_BACKEND` | Redis URLs used by Celery |

When you add a new setting, add it to `.env.example` too (without the secret value).

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
