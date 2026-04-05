# Cline official notes

These notes summarize the current upstream materials used to write the published Russian page in `docs/ru/cline/byok.md`.

## Core product positioning

- The official docs describe Cline as an open-source AI coding agent that lives in your editor and terminal.
- Cline can read and write files, run terminal commands, use a browser, and connect external tools through MCP servers.
- The docs emphasize a transparent, approval-first workflow where the user stays in control.
- The official overview also states that Cline runs client-side with the user’s API keys.

## Supported environments and install surfaces

- The installation guide currently lists VS Code, Cursor, Antigravity, JetBrains IDEs, CLI, Zed, Neovim, VSCodium, and Windsurf.
- The basic IDE install path is: open the editor, open Extensions, search for `Cline`, install it, and open Cline from the Activity Bar or command palette.
- The install guide says JetBrains works almost identically to VS Code for core features such as diff editing, tools, providers, MCP, and rules/workflows.

## Authentication and BYOK

- The getting-started guide splits authentication into two paths:
  - Cline Provider for built-in billing and account login
  - Bring Your Own Key for third-party providers, API keys, and local models
- BYOK is positioned for enterprise, custom billing, higher rate limits, beta-model access, and local privacy.
- The docs explicitly say API keys are stored in the system credential manager and sent only to the selected provider.
- The settings flow in the official guide is:
  1. Open Settings
  2. Select a provider
  3. Authenticate
  4. Choose a model
  5. Verify by sending a message

## OpenAI-compatible provider mapping

- Cline has a dedicated `OpenAI Compatible` provider type.
- The official OpenAI-compatible page says the key fields are:
  - Base URL
  - API Key
  - Model ID
- The same page says advanced model configuration can include:
  - Max Output Tokens
  - Context Window
  - Image Support
  - Computer Use / tool-calling related capability
  - Input and Output price metadata
- The docs explicitly say available model IDs differ by provider and the user should use the provider’s own model names.
- LeanToken maps cleanly to this flow:
  - API Provider: `OpenAI Compatible`
  - Base URL: `https://api.leantoken.tech/v1`
  - API Key: LeanToken API key
  - Model ID: whichever model ID is available in the LeanToken account

## Core workflows relevant to the published page

- The Tasks guide says every interaction happens inside a task.
- Tasks capture the full conversation history, code changes, command executions, decisions, token usage, API costs, and execution time.
- Tasks can be interrupted and resumed across sessions.

- Plan & Act mode is a first-class workflow.
- Plan mode can read the codebase and discuss strategy but cannot modify files or execute commands.
- Act mode can modify files and run commands while keeping the full context from planning.
- The docs recommend:
  - small tasks: start directly in Act mode
  - medium tasks: Plan first, then Act
  - large tasks: use `/deep-planning`
- Cline can use different models for Plan and Act modes.

## Safety and recovery features

- Checkpoints are enabled by default.
- The checkpoints guide says Cline maintains a shadow Git repository separate from the user’s real Git history.
- Checkpoints can restore files, task history, or both.
- The docs present checkpoints as the main safety net for faster, more autonomous workflows.

- Auto Approve is configurable per capability: reads, edits, commands, browser, MCP, and notifications.
- The official guidance recommends starting conservatively, especially for serious development work.
- YOLO mode auto-approves everything and is documented as dangerous.

## Advanced workflow and customization

- The customization overview says Cline has five main customization systems:
  - Rules
  - Skills
  - Workflows
  - Hooks
  - `.clineignore`
- Rules define behavior and constraints.
- Skills load domain expertise on demand.
- Workflows automate repeatable multi-step processes.
- Hooks inject custom logic at key moments and can validate or block operations.
- `.clineignore` controls which files and folders Cline can access or skip.

## Context management

- The `.clineignore` guide says ignoring dependencies, build artifacts, generated files, and secrets can reduce starting context dramatically.
- The docs give concrete examples such as `node_modules/`, `dist/`, `build/`, `.env`, and large binary assets.
- The Memory Bank guide describes a structured markdown-based documentation system for preserving project context across sessions.
- The docs position Memory Bank and Checkpoints as complementary:
  - Memory Bank preserves knowledge and decisions
  - Checkpoints preserve file state

## MCP notes

- The MCP overview says Model Context Protocol standardizes how applications provide context and tools to LLMs.
- Cline does not ship with MCP servers preinstalled.
- MCP servers can expose tools, prompts, and resources.
- The docs highlight use cases such as GitHub automation, browser automation, databases, dashboards, Jira/task systems, and documentation generation.

## Local screenshot note

- The user-provided screenshot is now stored as:
  - `images/cline/cline-openai-compatible-settings.png`
- The screenshot shows the Cline `Settings` panel with:
  - `API Provider = OpenAI Compatible`
  - Base URL field filled for LeanToken
  - `OpenAI Compatible API Key`
  - `Model ID = qwen3-235b`
  - optional `Custom Headers`
  - model configuration and advanced settings below
- This screenshot is the best available local proof for the exact UI flow used in the published page.

## Upstream pages used

- https://docs.cline.bot/introduction/overview
- https://docs.cline.bot/getting-started/installing-cline
- https://docs.cline.bot/getting-started/authorizing-with-cline
- https://docs.cline.bot/provider-config/openai-compatible
- https://docs.cline.bot/features/tasks/understanding-tasks
- https://docs.cline.bot/features/plan-and-act
- https://docs.cline.bot/core-workflows/checkpoints
- https://docs.cline.bot/features/auto-approve
- https://docs.cline.bot/mcp/mcp-overview
- https://docs.cline.bot/customization/overview
- https://docs.cline.bot/customization/clineignore
- https://docs.cline.bot/prompting/cline-memory-bank
- https://docs.cline.bot/features/hooks/index
