# LangChain

## About the Service

LangChain is an open source framework for building AI applications and agents. In the official LangChain documentation, it is positioned as a framework with a prebuilt agent architecture and integrations for models, tools, and external systems, so you can ship an application quickly and still keep the option to swap providers later.

LangChain is especially useful when a plain model call is not enough: you need tool calling, structured output, external APIs, agent workflows, or tracing through LangSmith. LangChain agents are built on top of LangGraph, so you can start with a simple setup and grow the architecture over time.

For BYOK, LangChain is a practical fit because it already supports OpenAI-compatible APIs. If your model access goes through `https://api.leantoken.tech/v1`, the integration usually comes down to passing your own API key, the base URL, and the model ID you are allowed to use.

## What You Will Need

- a Python or Node.js project that already uses LangChain
- an account on `https://leantoken.tech`
- a LeanToken API key
- the OpenAI-compatible endpoint `https://api.leantoken.tech/v1`
- the model `qwen3-235b`
- optionally, a LangSmith account if you want tracing and debugging

## AI Use Cases

LangChain is especially useful in scenarios like these:

- an internal AI assistant that calls your own APIs and tools
- a chat interface or internal assistant that runs through one stable model ID
- extracting structured data from emails, contracts, forms, product cards, or other text-heavy inputs
- agent workflows where the model chooses tools, executes steps, and assembles the final answer
- moving the same application across providers through LangChain's standard model interfaces
- tracing, debugging, and comparing runs through LangSmith without rewriting the whole app

## BYOK on leantoken.tech

In the LangChain context, BYOK on `leantoken.tech` means you do not rely on a bundled provider credential. Instead, you pass your own key and your own endpoint into LangChain.

In practice, that gives you a few concrete advantages:

- the same key can be reused in local development, staging, and production
- the same model ID `qwen3-235b` can be reused across your chat scenarios
- the same integration pattern works in both Python and TypeScript
- you keep the secret in `.env`, Docker secrets, CI/CD variables, or a cloud secret manager
- switching provider backends later is easier because LangChain is built around standard interfaces

One important limitation from the official LangChain documentation still applies: `ChatOpenAI` targets the official OpenAI API specification. If a provider adds non-standard response fields, LangChain may not preserve them. For a standard OpenAI-compatible BYOK flow with key, model, and base URL, this is usually fine.

## How To Get An API Key On leantoken.tech

Before configuring LangChain, get your key and endpoint in LeanToken.

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
8. Return to your LangChain project and store the key in `.env` or another secret store. If your integration uses the OpenAI-compatible route, store `https://api.leantoken.tech/v1` next to it as the base URL.
9. Do not commit the key to git. If it is exposed in logs, screenshots, tickets, or a repository, rotate it and update your application configuration.

## How To Add BYOK

LangChain does not expose a dedicated product settings screen for this. In practice, you add the key to `.env` and then pass it explicitly into your model initialization code.

### 1. Prepare `.env`

Use a simple setup like this:

```bash
LEANTOKEN_API_KEY=lt_your_key_here
LEANTOKEN_BASE_URL=https://api.leantoken.tech/v1
LEANTOKEN_MODEL=qwen3-235b
```

The variable names can differ in your own project. In this guide, the model ID is fixed as `qwen3-235b` because this integration should use only that model. In the code examples below, the model is also hardcoded explicitly.

### 2. Python: connect the chat model through `ChatOpenAI`

If you want the most direct OpenAI-compatible path for chat completions, use `langchain-openai`.

```bash
pip install -U langchain-openai
```

```python
import os
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="qwen3-235b",
    api_key=os.environ["LEANTOKEN_API_KEY"],
    base_url=os.environ.get("LEANTOKEN_BASE_URL", "https://api.leantoken.tech/v1"),
    temperature=0,
)

response = llm.invoke(
    [
        ("system", "You help a developer and answer briefly."),
        ("human", "Explain what LangChain is in one paragraph."),
    ]
)

print(response.content)
```

This is the most direct approach when your application already builds chains, tools, or agents around a concrete chat model object.

### 3. Python: use `init_chat_model` for a more generic entry point

LangChain also documents a more generic path through `init_chat_model`. It is useful when you want to stay closer to provider-agnostic LangChain APIs.

```bash
pip install -U "langchain[openai]"
```

```python
import os
from langchain.chat_models import init_chat_model

model = init_chat_model(
    model="qwen3-235b",
    model_provider="openai",
    api_key=os.environ["LEANTOKEN_API_KEY"],
    base_url=os.environ.get("LEANTOKEN_BASE_URL", "https://api.leantoken.tech/v1"),
    temperature=0,
)

result = model.invoke("List the main strengths of LangChain for AI applications.")
print(result.text)
```

If you expect to swap providers later, this entry point is usually easier to scale than a hard dependency on one specific wrapper class.

### 4. Embeddings and RAG

This page does not include embedding examples. Under your current restriction, the LangChain setup should use only `qwen3-235b`, and this guide documents the chat integration path only.

If LeanToken exposes a dedicated embedding model later, it should be documented separately instead of being mixed into the current `qwen3-235b` flow.

### 5. JavaScript / TypeScript: connect `ChatOpenAI`

In LangChain JS, a custom OpenAI-compatible URL is passed through `configuration.baseURL`.

```bash
npm install @langchain/openai @langchain/core
```

```ts
import "dotenv/config";
import { ChatOpenAI } from "@langchain/openai";

const llm = new ChatOpenAI({
  model: "qwen3-235b",
  apiKey: process.env.LEANTOKEN_API_KEY!,
  temperature: 0,
  configuration: {
    baseURL:
      process.env.LEANTOKEN_BASE_URL ?? "https://api.leantoken.tech/v1",
  },
});

const response = await llm.invoke(
  "Explain briefly how LangChain helps with agent workflows."
);

console.log(response.content);
```

The official JS docs also note that some proxies or third-party providers do not support `stream_options`. If you hit that exact issue, try `streamUsage: false`.

```ts
const llm = new ChatOpenAI({
  model: "qwen3-235b",
  apiKey: process.env.LEANTOKEN_API_KEY!,
  streamUsage: false,
  configuration: {
    baseURL:
      process.env.LEANTOKEN_BASE_URL ?? "https://api.leantoken.tech/v1",
  },
});
```

### 6. JavaScript / TypeScript: use `initChatModel`

If you want the more generic LangChain initialization path, use `initChatModel`.

```bash
npm install langchain @langchain/openai
```

```ts
import "dotenv/config";
import { initChatModel } from "langchain";

const model = await initChatModel("qwen3-235b", {
  modelProvider: "openai",
  apiKey: process.env.LEANTOKEN_API_KEY!,
  baseUrl:
    process.env.LEANTOKEN_BASE_URL ?? "https://api.leantoken.tech/v1",
  temperature: 0,
});

const response = await model.invoke(
  "Give a short list of reasons to use LangChain."
);

console.log(response.content);
```

This path works well when your application is organized around generic LangChain interfaces and you want a cleaner provider swap later.

### 7. Reuse the model object in your app

After that, you do not need a second authentication path. The same model object can be reused across LangChain features:

- in `create_agent(...)`
- in prompt-template based chains
- in tool-calling workflows
- in LangSmith tracing and evaluation flows

The basic idea is simple: configure BYOK once at the model object level, then reuse that object throughout the application.

## LangSmith And Tracing

The saved LangChain source material also mentions LangSmith tracing. This is useful when you want to inspect prompts, tool calls, latency, and runtime errors after moving to BYOK.

Minimal setup:

```bash
LANGSMITH_TRACING=true
LANGSMITH_API_KEY=your_langsmith_key
LANGSMITH_PROJECT=langchain-leantoken
```

After that, LangChain and LangSmith can log your model calls if your code already runs through LangChain and you have a valid LangSmith key.

## Practical Recommendations

- Keep the LeanToken key in `.env`, a secret manager, or CI/CD variables, not in source code.
- Do not replace `qwen3-235b` with an arbitrary model ID if your integration is restricted to this model only.
- If you want a provider-agnostic architecture, prefer `init_chat_model` or `initChatModel`.
- If you already use `ChatOpenAI`, you usually only need to pass the LeanToken key and base URL explicitly.
- If JS errors mention streaming usage metadata, try `streamUsage: false`.
- If you want better observability before production, add LangSmith early.

## What To Check After Setup

- the application no longer calls the default provider endpoint and uses `https://api.leantoken.tech/v1`
- `ChatOpenAI`, `init_chat_model`, or `initChatModel` receives a valid API key and does not fail with `401 Unauthorized`
- every model example uses `qwen3-235b`
- the key did not leak into git, logs, tickets, or screenshots
- if LangSmith is enabled, you can see a trace after a test request

## Final Setup Pattern

For LangChain BYOK through `leantoken.tech`, the practical flow is:

1. get a LeanToken API key and endpoint
2. store them in `.env` or a secret manager
3. pass them into `ChatOpenAI`, `init_chat_model`, or `initChatModel` while using `qwen3-235b`
4. reuse that model object across your chain, agent, and tool-calling workflows

If your project already uses OpenAI-compatible LangChain abstractions, moving it to LeanToken usually requires only minimal code changes.
