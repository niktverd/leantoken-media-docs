SourceCraft Code Assistant research notes

Checked on 2026-04-05.

Source: https://sourcecraft.dev/portal/code-assistant/
- The product page positions SourceCraft Code Assistant as an AI developer assistant available on the SourceCraft platform and as plugins for VS Code and JetBrains IDEs.
- Core marketed capabilities include agent mode, built-in chat, code review, context-aware suggestions, Next Edit Action, MCP integration, custom agent modes, and LLM provider configuration (BYOM).
- The product page FAQ explicitly says that Yandex Code Assistant became part of SourceCraft and was renamed to SourceCraft Code Assistant without changing functionality.
- The same page lists supported smart autocompletion languages including C++, Go, Java, JavaScript, TypeScript, Kotlin, Python, Scala, SQL, and Swift.

Source: https://sourcecraft.dev/portal/docs/ru/code-assistant/concepts/
- The overview page describes Code Assistant as an AI assistant based on LLMs.
- The docs separately distinguish code autocompletion, chat/agent mode, and quick actions.
- The docs include a warning that SourceCraft uses Yandex-hosted AI capabilities and that some organization data is transferred to Yandex for that path.

Source: https://sourcecraft.dev/portal/docs/en/code-assistant/operations/agent/api-configuration-profiles
- The models page says model configuration profiles allow switching between different AI providers, models, API keys, temperature settings, budgets, and provider-specific options.
- The default profile includes a Yandex code model and is limited by quotas.
- Supported provider examples explicitly include Yandex Cloud, OpenAI, Anthropic, OpenRouter, and Glama.
- API keys are securely stored in VS Code Secret Storage and are not exposed in plain text.
- Profiles can be linked to modes, and the extension remembers the last profile used for each mode.
- The feature is explicitly documented for Visual Studio Code.

Source: https://sourcecraft.dev/portal/docs/ru/code-assistant/operations/chat-prompts
- Built-in modes currently documented are `Code`, `Ask`, `Architect`, `Debug`, and `Orchestrator`.
- Tool access differs by mode: `Code` and `Debug` have full `read`, `edit`, `browser`, `command`, and `mcp`; `Ask` is read-only; `Architect` has restricted editing for markdown.
- The docs say each mode remembers the last used model/profile, which is relevant for documenting separate LeanToken-backed profiles per workflow.

Source: https://sourcecraft.dev/portal/docs/ru/sourcecraft/release-notes
- In III quarter 2025 release notes, SourceCraft announced GA availability and a major plugin update with agent mode, tool configuration, various LLM models, MCP servers, and selectable modes.
- The same release notes say the JetBrains plugin gained the ability to inspect context and add files/directories to chat context.
- In IV quarter 2025 release notes, SourceCraft added Replace mode for autocompletion, AGENTS.md support in chat, and the ability to disable data transfer to Yandex-hosted models.

Local asset used in published page
- /Users/niktverd/code/leantoken-tech-media-docs/images/yandex-code-assistant/yandex-code-assistant.png
- The screenshot still uses the old `Yandex Code Assistant Settings` title, which matches the product's previous branding.
- The visible configuration fields are `Profile`, `API Provider`, `Base URL`, `OpenAI API Key`, and `Model`.
- The screenshot clearly shows an `OpenAI Compatible` provider path with `https://api.leantoken.tech/v1`, which is enough to scaffold a LeanToken BYOK setup flow.

Working inference for LeanToken
- The strongest initial LeanToken path is Visual Studio Code, because the current official docs for model profiles are published for VS Code and the saved screenshot also comes from that UI.
- The published page should explain the rename from Yandex Code Assistant to SourceCraft Code Assistant so the legacy screenshot and current website naming do not conflict.
- A useful first public page should focus on profile-based BYOK through `OpenAI Compatible`, while leaving deeper JetBrains-specific details for a future follow-up.
