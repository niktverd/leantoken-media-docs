# LightRAG official notes

These notes summarize the current upstream materials used to write the published Russian page in `docs/ru/lightrag/byok.md`.

## Core product positioning

- The official repository describes LightRAG as `Simple and Fast Retrieval-Augmented Generation`.
- LightRAG combines vector retrieval with entity extraction, relation extraction, and graph-aware retrieval.
- LightRAG Server is the primary operational surface for most users.
- The server provides Web UI, API, and an Ollama-compatible interface.

## Why model choice matters more than in basic RAG

- The README says LightRAG places higher demands on the LLM than traditional RAG.
- Upstream recommends an LLM with at least 32B parameters.
- Upstream recommends at least 32KB context length and prefers 64KB.
- Upstream explicitly says not to use reasoning models during document indexing.
- Upstream says the query stage benefits from a stronger model than the indexing stage.

## Embedding and reranker guidance

- The README says a high-performance embedding model is essential for LightRAG.
- Upstream examples mention `BAAI/bge-m3` and `text-embedding-3-large`.
- The embedding model must be fixed before the first indexing run.
- If the embedding model or vector dimension changes, vector-related tables or local data must be recreated.
- Upstream says reranker support significantly improves mixed-query performance.
- The README recommends `mix` as the default query mode when reranking is enabled.

## Installation and startup

- The repository documents source install, PyPI install, and Docker Compose deployment.
- For source install the documented path uses `make dev` or `uv sync --extra test --extra offline`.
- The Web UI build path in upstream uses `bun install --frozen-lockfile` and `bun run build` inside `lightrag_webui`.
- The basic startup command is `lightrag-server`.
- The production multiprocess startup command is `lightrag-gunicorn --workers 4`.

## Setup wizard

- Upstream now documents setup targets such as `make env-base`, `make env-storage`, `make env-server`, and `make env-security-check`.
- The setup wizard generates `.env` and, when needed, related compose fragments.

## Environment model

- LightRAG separates LLM, embeddings, reranker, storage, auth, and server settings in `.env`.
- LLM uses `LLM_BINDING`, `LLM_MODEL`, `LLM_BINDING_HOST`, and `LLM_BINDING_API_KEY`.
- Embeddings use `EMBEDDING_BINDING`, `EMBEDDING_MODEL`, `EMBEDDING_DIM`, `EMBEDDING_BINDING_HOST`, and `EMBEDDING_BINDING_API_KEY`.
- Reranking uses `RERANK_BINDING`, `RERANK_MODEL`, `RERANK_BINDING_HOST`, and `RERANK_BINDING_API_KEY`.
- The sample env defaults to `LLM_BINDING=openai` with `https://api.openai.com/v1`.
- The sample env defaults to `EMBEDDING_BINDING=openai` with `text-embedding-3-large` and `EMBEDDING_DIM=3072`.
- The sample env defaults to `RERANK_BINDING=null`.

## Server behavior and operational notes

- The startup directory must contain the `.env` file.
- LightRAG loads `.env` into environment variables at startup.
- System environment variables override `.env`.
- After editing `.env`, opening a new terminal session is recommended.
- Default port is `9621`.
- Default `WORKING_DIR` is `./rag_storage`.
- Default `INPUT_DIR` is `./inputs`.
- Default `MAX_UPLOAD_SIZE` in the sample env is 100MB.

## Query modes

- Upstream query modes include `local`, `global`, `hybrid`, `naive`, and `mix`.
- `mix` combines knowledge graph and vector retrieval.
- Upstream treats reranking as especially relevant for `mix`.

## API and authentication

- Upstream distinguishes provider credentials from LightRAG server security.
- `LLM_BINDING_API_KEY` is for the model provider.
- `LIGHTRAG_API_KEY` is for protecting the LightRAG Server API itself.
- Web UI account auth uses `AUTH_ACCOUNTS`, `TOKEN_SECRET`, and JWT settings.
- The API key is passed through the `X-API-Key` header.
- Upstream warns that using only the API key without account auth can still leave insecure access paths.

## Deployment notes

- Docker deployment is documented officially.
- When LightRAG runs in Docker and needs to reach a local backend, upstream notes that `host.docker.internal` may be required instead of `localhost`.
- The API README includes Nginx notes for file upload size and streaming endpoints.
- Upload reverse proxies should allow large request bodies for `/documents/upload`.

## LightRAG Core notes

- Upstream recommends the REST API of LightRAG Server for normal integrations.
- LightRAG Core is positioned more for embedded applications, research, and evaluation.
- Core usage requires explicit async initialization of storages.

## LeanToken-specific conclusion for the published page

- LeanToken is a strong fit for the LLM side because LightRAG officially supports OpenAI-compatible LLM bindings.
- The page should not assume LeanToken embeddings or rerankers are available unless explicitly verified.
- The safest documented LeanToken path is therefore LeanToken for LLM plus separately validated embedding configuration.

## Upstream pages used

- https://github.com/HKUDS/LightRAG
- https://github.com/HKUDS/LightRAG/blob/main/README.md
- https://github.com/HKUDS/LightRAG/blob/main/lightrag/api/README.md
- https://raw.githubusercontent.com/HKUDS/LightRAG/main/env.example
- https://raw.githubusercontent.com/HKUDS/LightRAG/main/examples/lightrag_openai_compatible_demo.py
- https://arxiv.org/abs/2410.05779
