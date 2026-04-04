# Obsidian

## About the Service

Obsidian is a local-first note-taking app and personal knowledge base built on plain Markdown files. Its strength is not one built-in AI feature, but the community plugin ecosystem: you keep your vault locally, choose your own workflow, and connect only the AI tools you actually need.

For a BYOK setup in Obsidian, it is useful to separate two roles:

- `AI Providers` stores and reuses AI provider settings in one place
- a consumer plugin such as `Local GPT` then uses that provider for text actions, RAG, vision, and related AI workflows

That distinction matters: `AI Providers` is not a chat tool by itself and does not edit notes on its own. It is a configuration layer that saves you from re-entering the same `API key`, `Provider URL`, and model in every plugin separately.

In the official materials, `AI Providers` is presented as a centralized hub for AI provider configurations with support for `20+` providers, including `OpenAI`, `Anthropic`, `OpenRouter`, `Ollama`, `Google Gemini`, `Groq`, `LM Studio`, and `OpenAI compatible API`. That makes it a practical fit for `leantoken.tech`, because LeanToken exposes an OpenAI-compatible endpoint at `https://api.leantoken.tech/v1`.

This guide uses the following setup as the main example:

1. `Obsidian`
2. `AI Providers`
3. `Local GPT`
4. `leantoken.tech` as the OpenAI-compatible BYOK endpoint

## What You Will Need

- Obsidian Desktop installed
- `Community plugins` enabled
- the `AI Providers` plugin
- the `Local GPT` plugin if you want AI actions directly inside notes
- an account on `https://leantoken.tech`
- a LeanToken API key
- the base URL `https://api.leantoken.tech/v1`
- a model available in your LeanToken account
- optionally, a separate embedding provider if you want RAG features in `Local GPT`

## AI Use Cases

Obsidian is especially useful when AI should work inside a living knowledge base instead of outside your notes.

Practical use cases:

- summarizing long notes, meeting notes, research material, and articles
- extracting action items, theses, checklists, and structured lists from text
- continuing drafts, rewriting paragraphs, and fixing style or grammar
- Q&A across linked notes, backlinks, and PDF files through RAG-capable plugins
- generating new notes, intermediate summaries, wiki pages, and research digests
- mixing local and cloud models in one vault, where some tasks stay offline and others go to an external endpoint
- storing generated outputs back in the vault so every new research pass strengthens your personal knowledge base

A strong example of this workflow comes from Andrej Karpathy's post about `LLM Knowledge Bases`: he collects raw material as Markdown and images, uses an LLM to gradually compile a wiki, asks complex questions against that knowledge base, and stores results back into Obsidian. Post link: [LLM Knowledge Bases by Andrej Karpathy](https://x.com/karpathy/status/2039805659525644595).

Applied to Obsidian, the workflow looks like this:

1. collect articles, notes, repositories, PDFs, and images in a vault
2. use AI to summarize, categorize, and generate internal wiki pages
3. ask questions against the accumulated corpus
4. save answers, diagrams, slides, and derived notes back into the vault

## Why BYOK Is Especially Useful In Obsidian

BYOK is useful in Obsidian for a few practical reasons:

- you keep one controlled key and one endpoint that can be reused across compatible plugins
- you choose the model yourself instead of depending only on a plugin's bundled provider
- you can mix cloud and local setups, for example `Ollama` for local tasks and `LeanToken` for stronger remote models
- provider logic becomes easier to reuse across vaults and plugins because it is moved into a dedicated hub
- cost control, secret rotation, and credential management become simpler

For `leantoken.tech`, the setup usually comes down to two values:

- `API key`
- `Provider URL` or `base URL`: `https://api.leantoken.tech/v1`

## How To Get An API Key On leantoken.tech

Before configuring Obsidian, get your key and endpoint in LeanToken.

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
8. Return to Obsidian and paste the key into the `API key` field of the selected provider.
9. Paste `https://api.leantoken.tech/v1` into `Provider URL` if the plugin expects a separate endpoint field.
10. Treat the key as a secret. If it leaks into git, notes, issues, screenshots, shell history, or logs, rotate it.

## Which Plugins To Use For This Setup

There are many AI plugins in Obsidian, but for a documented and repeatable BYOK setup it is easier to use a split-responsibility pair:

- `AI Providers` handles provider configuration
- `Local GPT` uses that provider for concrete AI actions

Why this pair:

- the `AI Providers` README explicitly lists `Local GPT` as a plugin that uses this hub
- the `Local GPT` README explicitly says you need `AI Providers` to configure providers
- `Local GPT` already supports common text actions, image support, an action palette, and RAG-related workflows

If you later add other AI plugins, the logic stays the same: first check whether the plugin can consume settings from `AI Providers`, then reuse the LeanToken provider you already created.

## How To Install AI Providers And Local GPT

### 1. Enable community plugins

1. Open `Settings`.
2. Go to `Community plugins`.
3. If `Restricted mode` is enabled, disable it so you can install third-party plugins.

### 2. Install AI Providers

1. In `Community plugins`, click `Browse`.
2. Search for `AI Providers`.
3. Install the plugin and enable it.
4. After installation, open its settings page in `Settings`.

### 3. Install Local GPT

1. Open `Browse` in `Community plugins` again.
2. Search for `Local GPT`.
3. Install the plugin and enable it.
4. Optionally configure hotkeys for `Local GPT: Show context menu` and `Local GPT: Action Palette` in `Hotkeys`.

## How To Add LeanToken In AI Providers

Below is the `AI Providers` settings screen in Obsidian with a list of already configured providers.

![AI Providers screen in Obsidian with configured providers and the add-provider button](../../images/obsidian/obsidian-ai-providers-settings.png)

Typical LeanToken flow:

1. Open `Settings | Community plugins | AI Providers`.
2. Click `+` to add a new provider.
3. Choose the provider type that matches an OpenAI-compatible API in your plugin version.
4. If your UI includes an explicit `OpenAI compatible API` option, use it.
5. If labels differ slightly, follow the core requirement: the provider must let you set `Provider URL` manually and work with an OpenAI-compatible endpoint.
6. Name the connection, for example `LeanToken`.
7. Paste `https://api.leantoken.tech/v1` into `Provider URL`.
8. Paste your LeanToken key into `API key`.
9. Click the model refresh button if your version exposes it.
10. Select one of the models available in your LeanToken account.
11. Save the settings.

After that, the LeanToken provider should appear in the shared AI Providers list alongside your other providers.

Practical notes:

- if you want to use multiple models, create multiple provider entries with different names
- if you need a separate embedding backend for RAG, store it as a separate entry instead of mixing it with the chat configuration
- if you need a separate vision-capable model, keep that as its own provider entry as well

## How To Use LeanToken In Local GPT

Once the provider is saved in `AI Providers`, you can move to note-level actions through `Local GPT`.

Basic flow:

1. Open `Settings | Community plugins | Local GPT`.
2. Find the main AI provider selector or the setting that consumes configurations from `AI Providers`.
3. Select the LeanToken provider you created earlier.
4. If you want vision features, point Local GPT to a separate provider entry with an image-capable model.
5. If you want RAG or enhanced actions, configure the embedding provider separately.
6. Open any note, highlight text, and call `Local GPT` through the context menu or `Action Palette`.

Useful starter actions in `Local GPT`:

- `Continue writing`
- `Summarize text`
- `Fix spelling and grammar`
- `Find action items`
- `General help`

That is enough for a fast smoke test of the LeanToken connection.

## Recommended Obsidian Workflow For Knowledge Bases

If you use Obsidian not as a plain notebook but as an AI-assisted knowledge base, a practical workflow looks like this:

1. collect source material in the vault: articles, notes, PDFs, images, and links
2. use Obsidian Web Clipper when you want to save web pages as Markdown
3. use `Local GPT` to summarize, rewrite, and extract structure from notes
4. keep derived material close to the source: summary notes, comparison notes, wiki pages, and slide outlines
5. gradually add more links between notes in larger vaults so AI plugins can use links and backlinks better
6. add a dedicated embedding provider only where RAG actually helps

This matches the Karpathy workflow closely:

- raw sources are collected into a local knowledge base
- an LLM helps compile a wiki out of Markdown files
- more complex questions are asked against the accumulated corpus
- outputs are stored back into the knowledge base instead of being lost in a chat window

## What To Check After Setup

Minimal smoke test:

1. open a note with a few paragraphs of text
2. highlight a fragment
3. call `Local GPT`
4. choose `Summarize text`
5. confirm that the response arrives without an auth error or provider error

If it does not work:

- make sure `Provider URL` is `https://api.leantoken.tech/v1`
- make sure the `API key` field contains a valid LeanToken key
- refresh the model list
- confirm that the selected model is actually available in your account
- if RAG fails specifically, check the embedding provider separately because chat models and embeddings are not the same thing

## Limitations And Important Notes

- `AI Providers` is not a standalone AI assistant. It only stores and exposes configuration to other plugins.
- Exact field names and presets can vary slightly by plugin version.
- `Local GPT` can combine local models through `Ollama` with cloud or OpenAI-compatible providers, so LeanToken works well in mixed setups.
- Vision scenarios and RAG often work better with separate provider entries for separate tasks.
- If your vault contains sensitive notes, decide in advance which data can be sent to an external AI endpoint and which data should stay on local models.

## Useful Links

- [AI Providers: documentation](https://pfrankov-obsidian-ai-providers.mintlify.app/)
- [AI Providers: GitHub repository](https://github.com/pfrankov/obsidian-ai-providers)
- [AI Providers: community plugin page](https://obsidian.md/plugins?id=ai-providers)
- [Local GPT: documentation](https://pfrankov-obsidian-local-gpt.mintlify.app/)
- [Local GPT: GitHub repository](https://github.com/pfrankov/obsidian-local-gpt)
- [Local GPT: community plugin page](https://obsidian.md/plugins?id=local-gpt)
- [Obsidian Web Clipper](https://obsidian.md/clipper)
- [Andrej Karpathy: LLM Knowledge Bases](https://x.com/karpathy/status/2039805659525644595)
