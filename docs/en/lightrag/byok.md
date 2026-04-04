# LightRAG

## About the Service

LightRAG is an open source retrieval-augmented generation framework that combines standard vector search with entity extraction, relation extraction, and a knowledge graph layer. In the official repository, the project is described as `Simple and Fast Retrieval-Augmented Generation`, and the main operational surface for most users is LightRAG Server: a local server with Web UI, API, and an Ollama-compatible interface.

In practice, this means LightRAG is not limited to a basic "split documents into chunks and run similarity search" workflow. During indexing, it builds several knowledge layers at once:

1. chunks for standard RAG retrieval
2. entities and relations extracted from the corpus
3. a graph for graph-aware retrieval
4. dedicated storages for documents, statuses, graph data, and vectors

Because of that, LightRAG can answer not only simple factual questions about documents, but also more structural questions where roles, dependencies, events, and relationships across the corpus matter.

## How LightRAG Works

A typical LightRAG workflow looks like this:

1. You upload files or text into the server.
2. LightRAG splits the content into chunks.
3. The LLM extracts entities and relations from the text.
4. The embedding model encodes chunks, entities, and relations into vectors.
5. The service stores the results in its storage backends.
6. At query time, LightRAG combines vector retrieval, knowledge graph retrieval, and optionally a reranker.

This is also why LightRAG puts higher demands on model configuration than a basic RAG stack. According to the official README:

- the LLM should be at least around the `32B` class
- context length should be at least `32KB`, ideally `64KB`
- reasoning models are not recommended for document indexing
- embeddings should be chosen before the first indexing run and not changed without reindexing
- a reranker significantly improves mixed-query scenarios

## AI Use Cases

LightRAG is especially useful in scenarios like these:

- internal AI search over wiki pages, runbooks, engineering docs, and operational notes
- RAG over API documentation and technical specifications where relationships between entities matter
- analytical assistants for research, legal review, due diligence, and knowledge management
- chat over your own knowledge base where you need better retrieval quality than a basic file chat
- knowledge graphs built from articles, PDFs, presentations, policy documents, and project documentation
- a multimodal RAG pipeline if you later expand the setup toward RAG-Anything-style workflows

Put simply, LightRAG is useful when a standard "chat with files" setup is too shallow and you need the system to understand entities, relationships, and the overall structure of the corpus.

## What You Will Need

To run LightRAG with BYOK, you will usually need:

- a local LightRAG Server deployment or the `HKUDS/LightRAG` source tree
- `uv` or `pip` for the Python side
- `bun` if you build the Web UI from source
- an account on `https://leantoken.tech`
- a LeanToken API key
- the OpenAI-compatible endpoint `https://api.leantoken.tech/v1`
- a strong chat model available through your LeanToken endpoint
- an embedding backend, because LightRAG requires embeddings separately from the LLM
- optionally, a reranker if you want stronger retrieval in `mix` mode

## What Matters Before BYOK

For LightRAG, BYOK is not just one API key variable. The service has three separate configuration layers:

1. LLM
2. Embeddings
3. Reranker

The first two are mandatory. Without embeddings, LightRAG cannot index documents or retrieve context correctly.

In `.env`, this is reflected through:

- `LLM_BINDING`, `LLM_MODEL`, `LLM_BINDING_HOST`, `LLM_BINDING_API_KEY`
- `EMBEDDING_BINDING`, `EMBEDDING_MODEL`, `EMBEDDING_DIM`, `EMBEDDING_BINDING_HOST`, `EMBEDDING_BINDING_API_KEY`
- `RERANK_BINDING`, `RERANK_MODEL`, `RERANK_BINDING_HOST`, `RERANK_BINDING_API_KEY`

The main LeanToken-specific conclusion is:

- LeanToken is a good fit for the LLM side through an OpenAI-compatible API
- embedding compatibility must be validated separately
- reranking can be disabled at first, but it is worth planning for production retrieval

## BYOK on leantoken.tech

In the LightRAG context, BYOK on `leantoken.tech` usually means that you:

1. get your own LeanToken API key
2. set `https://api.leantoken.tech/v1` as `LLM_BINDING_HOST`
3. put the key into `LLM_BINDING_API_KEY`
4. choose a model that is actually available in your LeanToken account

This is a practical fit for LightRAG because the project officially supports `openai` and OpenAI-compatible backends for the LLM layer.

There is one important distinction to keep clear: `LLM_BINDING_API_KEY` and `LIGHTRAG_API_KEY` are not the same thing.

- `LLM_BINDING_API_KEY` lets LightRAG call the model provider
- `LIGHTRAG_API_KEY` protects your own LightRAG Server API

Do not mix them up. The first one is for model access. The second one is for securing your LightRAG instance.

## How to Get an API Key on leantoken.tech

Before configuring LightRAG, get your key and endpoint in LeanToken.

![LeanToken landing page with Sign up, Log in, and Dashboard](../../images/leantoken/leantoken.tech.1.landing.png)

1. Open `https://leantoken.tech`.
2. Click `Sign up` if you do not have an account yet, `Log in` if you already have one, or `Dashboard` if you are already signed in.

![LeanToken dashboard overview with API endpoint and quick links](../../images/leantoken/leantoken.tech.2.dashboard.png)

3. After signing in, open the dashboard or API overview page.
4. Copy the API endpoint. In the screenshots, it is `https://api.leantoken.tech/v1`.
5. Go to key management through `Manage keys` or `API Keys`.

![LeanToken API Keys page with Create key action](../../images/leantoken/leantoken.tech.3.key-managment.png)

6. Click `Create key`.
7. Copy the generated API key.
8. Return to the LightRAG project and paste the key into `.env` as `LLM_BINDING_API_KEY`.
9. In the same `.env`, set `LLM_BINDING_HOST=https://api.leantoken.tech/v1`.
10. Keep the key secret. If it appears in git, logs, tickets, screenshots, or shell history, rotate it.

## How to Choose Models for LightRAG

Before editing `.env`, it helps to think about three distinct roles.

### 1. LLM for indexing and answering

According to LightRAG guidance:

- use a strong non-reasoning chat model
- target at least the `32B` class
- prefer context length of `32KB` or more, ideally `64KB`

If your LeanToken account exposes several models, a stable general-purpose chat model without extra reasoning behavior is usually the safer first choice for document indexing.

### 2. Embedding model

The official materials directly mention strong options such as:

- `BAAI/bge-m3`
- `text-embedding-3-large`

For LightRAG, the embedding model is critical for two reasons:

- it directly affects retrieval quality
- its vector size must match `EMBEDDING_DIM`

If you later change the embedding model or its dimension, the existing index usually needs to be rebuilt and the documents reindexed.

### 3. Reranker

Reranking is optional in LightRAG, but the official README explicitly recommends it for mixed-query scenarios. Upstream examples mention:

- `BAAI/bge-reranker-v2-m3`
- Jina-based rerankers

For the first deployment, starting with `RERANK_BINDING=null` is fine. For production retrieval quality, it is often worth adding later.

## Recommended LeanToken Setup Pattern

The safest initial LeanToken setup for LightRAG is:

- use LeanToken for the LLM
- keep embeddings on a separate backend you already control
- enable reranking later after the base index is stable

That is safer than trying to force all three layers through one endpoint and then debugging whether the failure is in chat, embeddings, or reranking.

## Base `.env` Configuration

Here is a practical starter template.

```env
HOST=0.0.0.0
PORT=9621
SUMMARY_LANGUAGE=English
MAX_ASYNC=4
MAX_PARALLEL_INSERT=2

LLM_BINDING=openai
LLM_BINDING_HOST=https://api.leantoken.tech/v1
LLM_BINDING_API_KEY=lt_your_key_here
LLM_MODEL=<your_chat_model_from_LeanToken>

EMBEDDING_BINDING=ollama
EMBEDDING_BINDING_HOST=http://localhost:11434
EMBEDDING_MODEL=bge-m3:latest
EMBEDDING_DIM=1024
OLLAMA_EMBEDDING_NUM_CTX=8192

RERANK_BINDING=null

LIGHTRAG_KV_STORAGE=JsonKVStorage
LIGHTRAG_DOC_STATUS_STORAGE=JsonDocStatusStorage
LIGHTRAG_GRAPH_STORAGE=NetworkXStorage
LIGHTRAG_VECTOR_STORAGE=NanoVectorDBStorage
```

Important points:

- `SUMMARY_LANGUAGE=English` is a sensible default for English corpora
- `LLM_BINDING=openai` works for OpenAI-compatible backends, including LeanToken
- the default `JsonKVStorage`, `JsonDocStatusStorage`, `NetworkXStorage`, and `NanoVectorDBStorage` combination is appropriate for a test setup
- this is a good local proof-of-concept shape, not necessarily the final production architecture

## OpenAI-Compatible Embedding Variant

If you already have an OpenAI-compatible embedding backend, the configuration can look like this:

```env
EMBEDDING_BINDING=openai
EMBEDDING_BINDING_HOST=<your_embedding_endpoint>
EMBEDDING_BINDING_API_KEY=<your_embedding_key>
EMBEDDING_MODEL=<your_embedding_model_name>
EMBEDDING_DIM=<exact_embedding_dimension>
EMBEDDING_SEND_DIM=false
```

For this variant, check three things carefully:

1. the endpoint really exposes embeddings in an OpenAI-compatible format
2. `EMBEDDING_MODEL` matches the actual model ID
3. `EMBEDDING_DIM` matches the model's real vector size

If any of those are wrong, indexing and retrieval will become unstable or fail outright.

## How to Install LightRAG Server

LightRAG has several installation paths. For most users, the most practical ones are source install and Docker Compose.

### Option 1. Install from source

The official workflow looks like this:

```bash
git clone https://github.com/HKUDS/LightRAG.git
cd LightRAG
make dev
source .venv/bin/activate
cd lightrag_webui
bun install --frozen-lockfile
bun run build
cd ..
cp env.example .env
```

If you do not use `make dev`, upstream also documents the equivalent `uv` path:

```bash
uv sync --extra test --extra offline
source .venv/bin/activate
```

After that, fill in `.env` and start the server.

### Option 2. Docker Compose

If you prefer a containerized setup:

```bash
git clone https://github.com/HKUDS/LightRAG.git
cd LightRAG
cp env.example .env
docker compose up
```

The BYOK logic is the same in Docker: LLM, embeddings, and optionally the reranker are still configured through `.env`.

### Option 3. Setup wizard

Recent upstream versions also document a setup wizard that generates `.env` and related compose files:

```bash
make env-base
make env-storage
make env-server
make env-security-check
```

This is useful if you do not want to edit the long `env.example` file by hand.

## How to Add BYOK Step by Step

The flow below is the most practical starting point for LeanToken.

### 1. Prepare `.env`

Copy `env.example`:

```bash
cp env.example .env
```

Then fill in at least these fields:

```env
SUMMARY_LANGUAGE=English

LLM_BINDING=openai
LLM_BINDING_HOST=https://api.leantoken.tech/v1
LLM_BINDING_API_KEY=lt_your_key_here
LLM_MODEL=<your_chat_model_from_LeanToken>

EMBEDDING_BINDING=<your_embedding_binding>
EMBEDDING_BINDING_HOST=<your_embedding_endpoint>
EMBEDDING_BINDING_API_KEY=<your_embedding_key_if_needed>
EMBEDDING_MODEL=<your_embedding_model>
EMBEDDING_DIM=<your_embedding_dimension>
```

If you are not using a reranker yet:

```env
RERANK_BINDING=null
```

### 2. Remember that `.env` is loaded from the startup directory

The official API README explicitly notes:

- `.env` must be present in the startup directory
- LightRAG loads `.env` into the process environment when it starts
- system environment variables override `.env`
- after editing `.env`, opening a new terminal session is recommended

That matters if you run multiple LightRAG instances with different configurations.

### 3. Start the server

For the normal local mode:

```bash
lightrag-server
```

For the production multiprocess mode on Linux:

```bash
lightrag-gunicorn --workers 4
```

According to the official docs, the default port is `9621`.

### 4. Open the Web UI

After startup, the usual local URL is:

```text
http://localhost:9621
```

In the Web UI, verify:

1. the server opens without startup errors
2. document upload works
3. the indexing pipeline finishes without LLM or embedding errors
4. queries work after indexing completes

### 5. Start with a small document corpus

For the first BYOK validation, do not start with dozens of large PDFs.

A better first pass is:

- upload 3 to 5 documents
- wait for indexing to finish fully
- test several query types
- only then increase corpus size and storage complexity

## How to Work with the Web UI and API

LightRAG Server is practical because it covers three surfaces at once:

- Web UI for interactive use
- REST API for application integration
- an Ollama-compatible interface for external AI chat clients

A typical work loop looks like this:

1. upload documents
2. wait for indexing
3. open the knowledge graph or query interface
4. compare several query modes
5. tune reranking, storage, and context settings if needed

If you want to connect LightRAG into another product, upstream generally recommends using the REST API rather than embedding LightRAG Core directly unless you have a specific research or experimental need.

## Query Modes

LightRAG supports several query modes. In practice, the most useful ones to know are:

- `local`, which focuses more on local contextual retrieval
- `global`, which works with broader corpus-level knowledge
- `hybrid`, which combines local and global retrieval
- `naive`, which is a simpler baseline retrieval mode
- `mix`, which combines knowledge graph and vector retrieval and benefits the most from reranking

For an initial setup, comparing at least `hybrid` and `mix` is worth it. If your corpus is rich in entities and relationships, the difference can be noticeable.

## Reranker: When It Matters

You do not need a reranker to get started, but it becomes especially useful if:

- your queries are complex mixed queries
- your documents are long and semantically overlapping
- you want to reduce noise in the retrieved context
- `mix` becomes your main query mode

If the base deployment already works and you want to improve retrieval quality without changing the whole architecture, the reranker is often the next logical layer to add.

## Storage Backends and Working Directories

LightRAG uses four storage classes:

- KV storage
- vector storage
- graph storage
- document status storage

For a test environment, upstream recommends simple defaults:

- `JsonKVStorage`
- `JsonDocStatusStorage`
- `NetworkXStorage`
- `NanoVectorDBStorage`

As load grows, you can evaluate heavier backends such as PostgreSQL, Neo4j, MongoDB, Redis, Milvus, Qdrant, Memgraph, or OpenSearch, but that is an operational scaling step, not a requirement for the first BYOK deployment.

By default:

- `WORKING_DIR` is `./rag_storage`
- `INPUT_DIR` is `./inputs`

## How to Secure LightRAG Server Itself

This section matters because it is easy to confuse the provider key with the server key.

### 1. API key for the server

To protect the LightRAG API, use:

```env
LIGHTRAG_API_KEY=your-secure-api-key-here
```

This key is then passed in requests through the header:

```text
X-API-Key
```

### 2. Web UI login

If you want to restrict the Web UI, upstream recommends configuring both accounts and JWT settings:

```env
AUTH_ACCOUNTS='admin:{bcrypt}<hash>'
TOKEN_SECRET='your-secret'
TOKEN_EXPIRE_HOURS=4
```

The official docs explicitly note that `LIGHTRAG_API_KEY` alone is not enough if guest access through the Web UI and related API paths is still open.

The practical summary is:

- `LLM_BINDING_API_KEY` gives LightRAG access to LeanToken
- `LIGHTRAG_API_KEY` protects your LightRAG API
- `AUTH_ACCOUNTS` and `TOKEN_SECRET` protect Web UI access and user auth

## Production Notes

For production deployments, LightRAG deserves a few extra checks up front.

### Reverse proxy and large files

The official API README includes an Nginx-specific section:

- `/documents/upload` may require a larger `client_max_body_size`
- streaming endpoints work better with gzip disabled
- upload and long-running generation timeouts may need adjustment

This matters especially when you upload PDFs, presentations, or large technical documents.

### Multiple instances

LightRAG supports multiple instances with different `.env` files from different startup directories. For logical data isolation, you can also use `WORKSPACE`.

### Docker and localhost

If LightRAG runs in Docker and needs to reach a local backend, upstream notes that `host.docker.internal` may be required instead of `localhost`.

### Non-English corpora

If your main corpus is not in English, update `SUMMARY_LANGUAGE` accordingly so summaries and related internal processing do not default to English.

## Common Problems

These are the most common issues during a first LightRAG setup.

### The server starts, but indexing fails

Usually this means one of three things:

- the LLM is working, but embeddings are not configured
- `EMBEDDING_DIM` does not match the real model dimension
- the embedding endpoint is not compatible with the chosen binding

### Everything breaks after changing the embedding model

That is expected. LightRAG explicitly warns that after changing the embedding model, the old vector-related storage should be cleared and the documents reindexed.

### Indexing quality is poor

A common reason is using a weak model or a reasoning model for extraction. LightRAG recommends a strong non-reasoning model for document indexing.

### The server API looks protected, but access still exists

Check whether you configured only `LIGHTRAG_API_KEY` without `AUTH_ACCOUNTS`. Upstream explicitly warns that for real protection both layers are better enabled together.

### Upload fails with `413 Request Entity Too Large`

If LightRAG is behind Nginx or another reverse proxy, increase the request body limit at the proxy layer, not only inside `.env`.

## What to Check After Setup

- `LLM_BINDING_HOST` points to `https://api.leantoken.tech/v1`
- `LLM_BINDING_API_KEY` contains a valid LeanToken key
- `LLM_MODEL` matches a real model available in your LeanToken account
- the embedding backend answers correctly and uses the right `EMBEDDING_MODEL` and `EMBEDDING_DIM`
- the server starts on `localhost:9621` without errors
- documents index completely
- `hybrid` and `mix` queries return meaningful answers
- after changing the embedding model, you perform a full reindex
- `LIGHTRAG_API_KEY`, `AUTH_ACCOUNTS`, and `TOKEN_SECRET` are configured if the instance is reachable beyond local-only use
- the keys did not leak into git, Docker logs, shared notes, or screenshots

## Final Setup Pattern

If you compress the LightRAG plus LeanToken setup into one short pattern, it looks like this:

1. get your LeanToken API key and endpoint
2. configure LeanToken as `LLM_BINDING=openai`
3. configure embeddings separately
4. optionally add a reranker later
5. run `lightrag-server`
6. upload a small test corpus
7. validate indexing, `hybrid`, and `mix`
8. only then move on to storage scaling, auth hardening, and production deployment

For LightRAG, that is much safer than trying to start with a fully maximized configuration from day one. First make the `LLM + embeddings + indexing` chain stable, then improve retrieval, storage, and security.
