Roo Code research notes

Checked on 2026-04-05.

Source: https://roocode.com/
- Roo Code positions itself as an AI software engineering team.
- Official site highlights two form factors: local VS Code extension and Roo Code Cloud.
- Homepage explicitly says users can choose from curated providers or bring their own key.
- FAQ says Roo Code is open source, runs in VS Code, can read/write files, execute commands, and use external AI APIs.
- Homepage FAQ also positions Roo as more than autocomplete: multi-file edits, command execution, and customizable provider/model choice.

Source: https://docs.roocode.com/
- Docs describe Roo Code as an AI-coding suite with a local VS Code extension and cloud agents.
- Official usage list includes generating code, refactoring/debugging, updating docs, answering questions about the codebase, automating repetitive tasks, and using MCP servers.
- Docs emphasize that Roo is model-agnostic and needs an LLM provider.

Source: https://docs.roocode.com/getting-started/installing
- Roo Code is installed as a VS Code extension.
- Official installation paths: VS Code Marketplace, Open VSX, or VSIX.
- Troubleshooting notes mention VS Code 1.84.0+.

Source: https://docs.roocode.com/getting-started/connecting-api-provider
- Roo Code needs an inference provider.
- Provider configuration is done from the Roo panel in VS Code.
- Roo docs recommend starting with a model that works reliably for multi-step tool-driven tasks.

Source: https://docs.roocode.com/providers/
- OpenAI Compatible providers are supported in the VS Code extension.
- Roo Code Router is the easiest setup path, but it is not the BYOK path needed for LeanToken docs.

Source: https://docs.roocode.com/providers/openai
- Roo Code provider setup is done from the extension panel via the gear icon in Roo Code Settings.
- Official OpenAI flow includes API Provider, API Key, Model, and optional Base URL.

Source: https://docs.roocode.com/providers/openai-compatible
- OpenAI Compatible setup requires Base URL, API Key, and Model ID.
- Roo Code exposes additional model configuration fields such as max output tokens, context window, image support, computer use, and pricing metadata.
- Roo Code uses native tool calling exclusively. No XML fallback.
- If a model does not support OpenAI-compatible native tool calling, it cannot be used with Roo Code.

Source: https://docs.roocode.com/providers/litellm
- Roo Code documents an OpenAI Compatible provider flow with Base URL, API Key, and Model selection.
- This provider page is enough to justify LeanToken documentation through the OpenAI Compatible setup path.

Source: https://docs.roocode.com/features/api-configuration-profiles
- Profiles can store different providers, API keys, models, temperatures, rate limits, and provider-specific settings.
- Profiles are created from the Providers settings screen with a plus button near the selector.
- Tasks remember the profile they started with.
- API keys are stored in VS Code Secret Storage.

Source: https://docs.roocode.com/basic-usage/using-modes
- Core built-in modes: Code, Ask, Architect, Debug, Orchestrator.
- Different modes expose different tool groups and use cases.
- Roo remembers the last-used model per mode.

Source: https://docs.roocode.com/basic-usage/how-tools-work
- Roo works by proposing tools, waiting for approval, executing them, and continuing until completion.
- Core categories include read, edit, command, mcp, image, and workflow tools.

Source: https://docs.roocode.com/basic-usage/context-mentions
- Roo supports mentions for files, folders, images, problems, terminal output, git state, and URLs.
- Image mentions require a model with vision support.

Source: https://docs.roocode.com/tips-and-tricks
- Roo docs recommend careful use of max tokens and context budgets.
- Tips mention using separate models for different modes and being intentional about large-context work.

Source: https://docs.roocode.com/features/codebase-indexing
- Codebase indexing uses Tree-sitter, embeddings, Qdrant, and the `codebase_search` tool.
- OpenAI Compatible can be used as an embedding provider, but indexing is a separate setup from basic chat/provider configuration.

Local asset used in published page
- /Users/niktverd/code/leantoken-tech-media-docs/images/roocode/roocode.png
- Screenshot shows Roo Code Settings → Providers with a LeanToken OpenAI Compatible profile and model `qwen3-235b`.

Working inference for LeanToken
- LeanToken should be documented through the OpenAI Compatible setup path.
- The published page should focus on the VS Code extension flow first, because that path is directly documented in the official provider setup pages.
- The most important compatibility warning is native tool calling support, not just plain chat compatibility.
- Final production guidance should validate one or more LeanToken model IDs against Roo Code tool-driven workflows before making stronger compatibility claims.
