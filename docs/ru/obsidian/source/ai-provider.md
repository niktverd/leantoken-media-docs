AI Providers

By Pavel Frankov

37,338 downloads
A hub for setting AI providers (OpenAI-like, Ollama and more) in one place.


Obsidian AI Providers

⚠️ Important Note: This plugin is a configuration tool - it helps you manage your AI settings in one place.

Think of it like a control panel where you can:

    Store your API keys and settings for AI services
    Share these settings with other Obsidian plugins
    Avoid entering the same AI settings multiple times

The plugin itself doesn't do any AI processing - it just helps other plugins connect to AI services more easily.
image
Required by plugins

    Local GPT

Supported providers

    OpenAI
    OpenRouter
    Anthropic
    Google Gemini
    Mistral AI
    Together AI
    Fireworks AI
    Perplexity AI
    DeepSeek
    xAI (Grok)
    Cerebras
    Z.AI
    Groq
    302.AI
    Novita AI
    DeepInfra
    SambaNova
    LM Studio
    Ollama (and Open WebUI)
    OpenAI compatible API

Features

    Fully encapsulated API for working with AI providers
    Develop AI plugins faster without dealing directly with provider-specific APIs
    Easily extend support for additional AI providers in your plugin
    Available in 11 languages: English, Spanish, French, Italian, Portuguese, German, Russian, Chinese, Japanese, Korean, and Dutch

Installation
Obsidian plugin store (recommended)

This plugin is available in the Obsidian community plugin store https://obsidian.md/plugins?id=ai-providers
BRAT

You can install this plugin via BRAT: pfrankov/obsidian-ai-providers
Create AI provider
Ollama

    Install Ollama.
    Install Gemma 2 ollama pull gemma2 or any preferred model from the library.
    Select Ollama in Provider type
    Click refresh button and select the model that suits your needs (e.g. gemma2)

Additional: if you have issues with streaming completion with Ollama try to set environment variable OLLAMA_ORIGINS to *:

    For MacOS run launchctl setenv OLLAMA_ORIGINS "*".
    For Linux and Windows check the docs.

OpenAI

    Select OpenAI in Provider type
    Set Provider URL to https://api.openai.com/v1
    Retrieve and paste your API key from the API keys page
    Click refresh button and select the model that suits your needs (e.g. gpt-4o)

OpenAI compatible server

There are several options to run local OpenAI-like server:

    Open WebUI
    llama.cpp
    llama-cpp-python
    LocalAI
    Obabooga Text generation web UI
    LM Studio
    ...maybe more

OpenRouter

    Select OpenRouter in Provider type
    Set Provider URL to https://openrouter.ai/api/v1
    Retrieve and paste your API key from the API keys page
    Click refresh button and select the model that suits your needs (e.g. anthropic/claude-3.7-sonnet)

Google Gemini

    Select Google Gemini in Provider type
    Set Provider URL to https://generativelanguage.googleapis.com/v1beta/openai
    Retrieve and paste your API key from the API keys page
    Click refresh button and select the model that suits your needs (e.g. gemini-1.5-flash)

LM Studio

    Select LM Studio in Provider type
    Set Provider URL to http://localhost:1234/v1
    Click refresh button and select the model that suits your needs (e.g. gemma2)

Groq

    Select Groq in Provider type
    Set Provider URL to https://api.groq.com/openai/v1
    Retrieve and paste your API key from the API keys page
    Click refresh button and select the model that suits your needs (e.g. llama3-70b-8192)

For plugin developers

Docs: How to integrate AI Providers in your plugin.

Quick reference (details in SDK docs):

try {
	const finalText = await aiProviders.execute({
		provider,
		prompt: "Hello",
		onProgress: (chunk, full) => {/* stream UI update */},
		abortController
	});
	// use finalText
} catch (e) {
	// handle error / abort
}

Removed callbacks: onEnd / onError — promise resolve/reject covers them (only onProgress remains for streaming). Legacy chainable handler also deprecated.
Roadmap

    Docs for devs
    Ollama context optimizations
    German translations
    Chinese translations
    Update to latest OpenAI version and embedding models
    Russian translations
    Groq Provider support
    Passing messages instead of one prompt
    Anthropic Provider support
    Shared embeddings to avoid re-embedding the same documents multiple times
    Spanish, Italian, French, Dutch, Portuguese, Japanese, Korean translations
    Incapsulated basic RAG search with optional BM25 search

My other Obsidian plugins

    Local GPT that assists with local AI for maximum privacy and offline access.
    Colored Tags that colorizes tags in distinguishable colors.

# Obsidian AI Providers research notes

Primary sources used for the published page:

- Docs home: https://pfrankov-obsidian-ai-providers.mintlify.app/
- GitHub repo: https://github.com/pfrankov/obsidian-ai-providers
- Plugin store: https://obsidian.md/plugins?id=ai-providers

Key points confirmed from official docs and README:

- AI Providers is a configuration hub, not an AI chat tool by itself.
- It stores provider settings once and exposes them to compatible Obsidian plugins.
- Official docs describe support for 20+ providers.
- GitHub README explicitly lists `OpenAI compatible API` among supported providers.
- The plugin UI is used to create provider entries with fields such as provider URL, API key, and model selection.
- The README includes setup examples for OpenAI, OpenRouter, Google Gemini, Groq, LM Studio, and Ollama.
- The README explicitly names `Local GPT` as a plugin that requires AI Providers.

LeanToken-specific mapping used in the published page:

- Provider type should correspond to an OpenAI-compatible provider preset.
- Provider URL: `https://api.leantoken.tech/v1`
- Credential field: LeanToken API key
- Model: select one of the models available in the user account after refreshing the list

Local screenshot source:

- Raw capture: `docs/ru/obsidian/source/415326379-09b6313d-726c-440b-9201-1b2f2e839fa7.gif`
- Published still image: `images/obsidian/obsidian-ai-providers-settings.png`

Notes:

- The exact preset names may differ between plugin versions.
- The screenshot shows AI Providers as a settings screen with a list of configured providers, model badges, provider URLs, and add/settings/copy/delete controls.
