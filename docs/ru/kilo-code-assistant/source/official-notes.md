Kilo Code research notes

Checked on 2026-04-05.

Source: https://kilo.ai/docs/getting-started
- Kilo Code positions itself as an open-source AI coding assistant.
- Official docs say it works in IDEs, CLI, browser/mobile, and Slack.
- The overview page explicitly lists coding, debugging, reviews, automation, and deployment/security tasks.

Source: https://kilo.ai/docs/getting-started/installing
- Official installation docs include VS Code, VS Code Preview, JetBrains, CLI, Slack, and other IDE paths.
- The simplest published BYOK flow for this repo should still be the VS Code extension because the current LeanToken screenshot is from the Settings webview in the extension.
- The docs include Marketplace installation and the command `code --install-extension kilocode.kilo-code`.

Source: https://kilo.ai/docs/getting-started/setup-authentication
- Kilo Code can be used with the built-in Kilo provider, but the docs also explicitly support using another API provider.
- The setup page says Kilo supports over 30 providers.
- This is enough to justify positioning LeanToken as an external provider path rather than a workaround.

Source: https://kilo.ai/docs/ai-providers/openai-compatible
- OpenAI Compatible is a first-class provider path in Kilo Code docs.
- The required fields are API Provider, Base URL, API Key, and Model.
- Advanced model settings can include max output tokens, context window, image support, computer use, and pricing metadata.
- Kilo Code supports a full endpoint URL in the Base URL field when a provider needs a non-standard path.
- Official troubleshooting notes call out Invalid API Key, Model Not Found, connection errors, and trying a different model for poor results.

Source: https://kilo.ai/docs/getting-started/settings
- The gear icon in the Kilo Code chat view opens the settings page.
- The UI is organized into tabs including Providers, Auto-Approve, Models, and more.
- VS Code extension and CLI share the same underlying settings system.
- Primary config locations are `~/.config/kilo/kilo.jsonc`, `kilo.jsonc`, and `.kilo/kilo.jsonc`.
- The docs explicitly warn not to commit API keys and other secrets to version control.

Source: https://kilo.ai/docs/getting-started/settings/auto-approving-actions
- Kilo uses a granular per-tool permission system with Allow / Ask / Deny.
- Default fallback is `ask` when no rule matches.
- The docs strongly warn that broad auto-approval, especially for command execution, has real security risk.

Source: https://kilo.ai/docs/getting-started/quickstart
- Kilo works iteratively through a review/approve cycle.
- The current quickstart says most tools are auto-approved by default, while shell commands, external directory access, and sensitive file reads still prompt.
- The quickstart also states that settings are managed through `kilo.jsonc`, agents replace legacy modes terminology, and autocomplete uses FIM with Codestral.

Source: https://kilo.ai/docs/code-with-ai/agents/using-agents
- Current built-in agents are `code`, `ask`, `plan`, and `debug`.
- `orchestrator` is deprecated.
- `code` and `debug` have full tool access; `ask` is read-only; `plan` is planning-focused with restricted editing.

Source: https://kilo.ai/docs/code-with-ai/agents/context-mentions
- Kilo supports `@`-mentions for files and automatically includes editor context.
- The docs explicitly mention built-in discovery tools like `read`, `grep`, and `glob`.
- This justifies published usage examples around `@problems`, `@terminal`, and targeted file mentions.

Source: https://kilo.ai/docs/code-with-ai/features/autocomplete
- Autocomplete is documented separately from chat/provider configuration.
- The current docs say it uses Fill-in-the-Middle completion powered by Codestral (`mistralai/codestral-2508`).
- That means a LeanToken provider page should avoid overclaiming that chat BYOK automatically configures autocomplete behavior.

Source: https://kilo.ai/docs/code-with-ai/agents/model-selection
- Kilo explicitly avoids static model recommendations and points users to `kilo.ai/models`.
- The guide gives stable principles instead: stronger models for complex coding, mid-tier for everyday work, and larger context windows for large codebases.
- The guide also warns that aggressive Max Tokens settings reduce room left for conversation history.

Source: https://kilo.ai/docs/code-with-ai/agents/auto-model
- Kilo has its own auto-routing tiers such as `kilo-auto/frontier`, `kilo-auto/balanced`, and `kilo-auto/free`.
- These are part of the built-in Kilo model-routing story, not the same thing as documenting LeanToken as an external provider.

Source: https://kilo.ai/docs/customize/custom-modes
- Custom agents can be managed through the Settings UI or as Markdown files in `.kilo/agents/`.
- This is relevant for a more advanced LeanToken page because one connected provider can serve multiple specialized agents in the same workspace.

Local asset used in published page
- /Users/niktverd/code/leantoken-tech-media-docs/images/kilo/kilo.png
- Screenshot shows `Kilo Settings -> Providers` and a `Custom provider` modal with LeanToken values.
- The modal visibly contains `Provider ID`, `Display name`, `Base URL`, and `API key`.
- The help text under `Provider ID` indicates lowercase letters, numbers, hyphens, or underscores, so the published doc should recommend `lean-token` rather than a spaced or capitalized value.

Working inference for LeanToken
- The best initial LeanToken path is Kilo Code via the official OpenAI Compatible provider configuration.
- The published page can now use the saved settings screenshot as the central configuration image.
- The strongest practical distinctions to explain are:
  - built-in Kilo provider vs external LeanToken provider
  - chat/agent provider configuration vs autocomplete
  - shared extension/CLI config vs secret-handling risks
