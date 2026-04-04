# LangChain official notes for BYOK page

These notes summarize the official pages used to update `docs/ru/langchain/byok.md`.

## Product positioning

- LangChain overview says LangChain is an open source framework with a prebuilt agent architecture and integrations for any model or tool.
- LangChain overview says LangChain is the easy way to build custom agents and applications quickly.
- LangChain overview says LangChain agents are built on top of LangGraph.
- LangChain overview recommends LangSmith for tracing and debugging.

## Python integration details

- `langchain-openai` is the package used for `ChatOpenAI` and `OpenAIEmbeddings`.
- `ChatOpenAI` accepts `api_key` and `base_url`.
- `OpenAIEmbeddings` accepts `api_key` and `base_url`.
- `init_chat_model(...)` can be used with `model_provider="openai"`, `base_url=...`, and `api_key=...`.
- Python docs mention `stream_usage=True` when streaming token usage metadata is needed.

## JavaScript / TypeScript integration details

- `@langchain/openai` is the package used for `ChatOpenAI` and `OpenAIEmbeddings`.
- JS `ChatOpenAI` uses `configuration.baseURL` for a custom OpenAI-compatible endpoint.
- JS docs explicitly note that some proxies or third-party providers may require `streamUsage: false`.
- `OpenAIEmbeddings` also uses `configuration.baseURL`.
- `initChatModel(...)` can be used with `modelProvider: "openai"`, `baseUrl`, and `apiKey`.

## Compatibility caveat

- Official LangChain docs state that `ChatOpenAI` targets official OpenAI API specifications only.
- Docs warn that non-standard provider-specific response fields may not be extracted or preserved.
- For the LeanToken page, this means the main supported path should stay focused on standard OpenAI-compatible usage: API key, model ID, and base URL.

## Official pages used

- https://www.langchain.com/langchain
- https://docs.langchain.com/oss/python/langchain/overview
- https://docs.langchain.com/oss/javascript/langchain/overview
- https://docs.langchain.com/oss/python/integrations/chat/openai
- https://docs.langchain.com/oss/javascript/integrations/chat/openai
- https://docs.langchain.com/oss/python/integrations/embeddings/openai
- https://docs.langchain.com/oss/javascript/integrations/embeddings/openai
- https://docs.langchain.com/oss/python/langchain/models
- https://docs.langchain.com/oss/javascript/langchain/models
