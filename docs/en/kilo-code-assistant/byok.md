# Kilo Code

## About the Service

Kilo Code is an open source AI coding assistant that helps you write code, understand codebases, fix bugs, refactor, and automate engineering work. In the current official documentation, Kilo is positioned as a tool that works where developers already work: in the IDE, CLI, browser, mobile apps, and Slack.

That matters for practical development for two reasons:

- Kilo Code is not limited to one built-in way of accessing models
- the same workflow can be carried across VS Code, JetBrains, and CLI without changing the core mental model

This page focuses on the most useful LeanToken scenario: connecting `https://leantoken.tech` to Kilo Code as an `OpenAI Compatible` provider with your own API key and your own model ID.

## When Kilo Code Is Especially Useful

Kilo Code is useful not just as a model chat, but as a real development tool.

- quickly exploring an unfamiliar repository through file reads and architectural explanations
- implementing small and medium-sized tasks directly from chat
- doing careful refactors where you first understand the code and then apply targeted changes
- debugging through a combination of diagnostics, terminal output, logs, and iterative fixes
- updating documentation, changelogs, and supporting files next to the code
- multi-step engineering work where it helps to switch between planning, coding, and debugging

## How Work Is Organized in Kilo Code

To configure BYOK well, it helps to understand a few core Kilo systems first.

### Agents

In the current Kilo documentation, the main working personas are called `agents`. They are specialized modes with different tool access and different focus.

- `code` is the main agent for implementation, file edits, and general development
- `ask` is for questions and explanations without modifying the codebase
- `plan` is for design, planning, and shaping an implementation
- `debug` is for diagnostics and step-by-step investigation of failures

In practice, the simple rule is:

- use `ask` and `plan` for research and decision-making
- use `code` and `debug` for actual execution work

### Context and Mentions

Kilo can discover relevant files on its own through built-in tools such as `read`, `grep`, and `glob`, but you can also steer it directly. The official docs describe `@` mentions such as:

- `@/path/to/file` for a specific file
- `@problems` for issues from the Problems panel
- `@terminal` for terminal output
- `@git-changes` for uncommitted changes

The extension also includes active editor context and open tabs automatically. That makes Kilo useful for workflows where you want to move quickly from a question to a precise change.

### Approvals and Auto-Approve

According to the current quickstart, Kilo Code works iteratively: the agent proposes an action, you approve or reject it, and then the work continues.

The docs also note that many tools are auto-approved by default, while approval becomes especially important for:

- shell commands
- access to external directories
- sensitive file reads

If you need a stricter setup, `Settings -> Auto-Approve` lets you choose one of these states per tool:

- `Allow`
- `Ask`
- `Deny`

This matters for BYOK because a strong model routed through LeanToken can work quite autonomously. The stronger the model and the more trust you place in it, the more carefully you should review permissions for `bash`, file edits, and external tools.

### Settings and Config Files

Current Kilo documentation says the VS Code extension and CLI use the same underlying settings system. Changes made in the UI are reflected in configuration files.

The main configuration locations are:

- global config: `~/.config/kilo/kilo.jsonc`
- project config: `kilo.jsonc` in the project root
- alternate project path: `.kilo/kilo.jsonc`

This is useful for team workflows and CLI usage, but it also creates risk: if you commit config files to git, you must not leave secrets there.

## Why Kilo Code Is a Good Fit for BYOK

Kilo Code can be used through the built-in Kilo provider, but that is not the only path. The current official docs explicitly say the product supports dozens of external providers, including `OpenAI Compatible`. That is exactly what makes it a good fit for LeanToken.

## BYOK on leantoken.tech: What It Means in Kilo Code

In Kilo Code, Bring Your Own Key with `leantoken.tech` means that you:

1. create your own LeanToken API key
2. connect LeanToken in Kilo as a separate provider
3. choose `OpenAI Compatible`
4. set the endpoint to `https://api.leantoken.tech/v1`
5. paste your own key
6. select a model ID that matches your workflow

According to the official OpenAI-compatible provider docs, Kilo Code only needs four core settings for the initial setup:

- `API Provider`
- `Base URL`
- `API Key`
- `Model`

That is enough for the first successful connection. Additional fields such as `Max Output Tokens`, `Context Window`, or `Image Support` can be tuned later once the base connection is stable.

## What You Need Before Setup

Before you start, prepare:

- Kilo Code installed in VS Code, JetBrains, or another supported environment
- an account on `https://leantoken.tech`
- a LeanToken API key
- the endpoint `https://api.leantoken.tech/v1`
- at least one model ID you want to use through LeanToken

The main setup flow below is written for the extension UI, because that is what your screenshot shows.

## How to Get an API Key on leantoken.tech

Before configuring Kilo Code, get your API key and endpoint in LeanToken.

![LeanToken landing page with Sign up, Log in, and Dashboard](../../images/leantoken/leantoken.tech.1.landing.png)

1. Open `https://leantoken.tech`.
2. Click `Sign up` if you do not have an account yet, `Log in` if you already have one, or `Dashboard` if you are already signed in.

![LeanToken dashboard overview with API endpoint and quick links](../../images/leantoken/leantoken.tech.2.dashboard.png)

3. After signing in, open the dashboard or API overview page.
4. Copy the API endpoint. In the current screenshots, it is `https://api.leantoken.tech/v1`.
5. Go to key management through `Manage keys` or `API Keys`.

![LeanToken API Keys page with Create key action](../../images/leantoken/leantoken.tech.3.key-managment.png)

6. Click `Create key`.
7. Copy the generated API key.
8. Treat the key as a secret. If it appears in git, logs, shell history, exports, or screenshots, rotate it.

![Current Kilo Settings -> Providers screen with the Custom provider dialog used to connect LeanToken through an OpenAI-compatible Base URL](../../images/kilo/kilo.png)

## How to Install Kilo Code

The official Kilo docs support multiple environments: VS Code, VS Code Preview, JetBrains, CLI, Slack, and other IDEs. For LeanToken documentation, the VS Code flow is the clearest reference because it matches the current settings UI shown in your screenshot.

### Install in VS Code

1. Open VS Code.
2. Go to `Extensions`.
3. Search for `Kilo Code`.
4. Click `Install`.
5. Confirm that the Kilo Code icon appears in the sidebar after installation.

The official docs also include this command-line install path:

```bash
code --install-extension kilocode.kilo-code
```

### What Happens After Installation

If you want to use the built-in Kilo provider, the extension will prompt you to sign in with a Kilo account. For LeanToken, that is not required: you can connect your own provider instead through settings.

## How to Add LeanToken as a Custom Provider in Kilo Code

Your screenshot shows the most useful LeanToken path: the `Providers` tab and the `Custom provider` dialog.

According to the official docs, Kilo settings are opened through the gear icon in the extension panel.

### Step-by-Step Setup

1. Open the Kilo Code panel in the editor sidebar.
2. Click the gear icon to open `Settings`.
3. Go to the `Providers` tab.
4. If the provider is not visible right away, open the extended provider list and choose the path for adding a custom or OpenAI-compatible provider.
5. Open the `Custom provider` dialog shown in the screenshot.
6. In `Provider ID`, enter a stable identifier without spaces or capital letters. A practical value is `lean-token`.
7. In `Display name`, enter a readable name such as `Lean Token`.
8. In `Base URL`, enter `https://api.leantoken.tech/v1`.
9. In `API key`, paste your LeanToken API key.
10. Confirm the provider connection.
11. After the provider is connected, choose the model you want in the Kilo model selector.
12. Send a small test prompt to verify that the provider works.

### What Each Field Means in the `Custom provider` Dialog

It helps to understand the purpose of each field so you do not make the setup fragile.

#### `Provider ID`

This is the internal technical identifier for the provider inside Kilo. It should usually be:

- lowercase
- without spaces
- made of letters, numbers, hyphens, or underscores

For LeanToken, a safe value is:

```text
lean-token
```

#### `Display name`

This is the user-facing label you will see in the UI. A readable value is:

```text
Lean Token
```

#### `Base URL`

This is the key field for the OpenAI-compatible setup. For LeanToken, use:

```text
https://api.leantoken.tech/v1
```

Kilo also documents support for full endpoint URLs. That is useful for custom proxies and enterprise setups, but for a normal LeanToken integration the standard base URL is enough.

#### `API key`

This is where you paste the LeanToken API key. The screenshot notes that the field can be left empty if auth is handled through headers, but that does not apply to the normal LeanToken setup. In the standard flow, the API key should be filled in directly here.

### Minimum Working Configuration

For most cases, this configuration is enough:

```text
API Provider: OpenAI Compatible
Base URL: https://api.leantoken.tech/v1
API Key: <your_LeanToken_API_key>
Model: <your_model_id>
```

## How to Choose a Model ID for Kilo Code Through LeanToken

The most common mistake in AI coding assistants is picking a model that sounds good in chat but performs poorly in a real multi-step engineering workflow. Kilo's documentation has a useful practical point: instead of memorizing a static winner list, follow stable model-selection principles.

### Practical Selection Logic

- for complex coding tasks, premium models in the Claude Sonnet/Opus, GPT-5, or strong Gemini-like class usually work better
- for everyday development, strong mid-tier models often offer the best balance of speed, cost, and quality
- for budget-sensitive work, modern Qwen-, DeepSeek-, or similar families can be good if they are stable in your actual tasks

### Why Context Window Matters

Kilo's model guide notes that context size has a major effect on usability:

- `32K-64K` is often enough for small projects and localized tasks
- `128K` works well for typical applications and several related files
- `256K+` becomes useful for large repositories and broader cross-file reasoning

If you work in long sessions, avoid setting `Max Tokens` too high without a reason. The docs explicitly warn that a large output budget reduces the room available for conversation history.

### A Practical Recommendation for the First Run

1. Start with a model you already trust for coding work.
2. Validate it not only on questions, but on real file work.
3. If it loses the thread in long tasks, move to a stronger model or more context.
4. If quality is good but the workflow is too slow or expensive, try a faster mid-tier option.

## How to Work in Kilo After Connecting LeanToken

Once the provider is connected, do not start with a half-day task right away. First validate the basic loop.

### First Verification

1. Open Kilo Code.
2. Select the LeanToken-backed provider and the model you want.
3. Send a short prompt such as:

```text
Explain which files in this project are responsible for app startup.
```

4. Then give Kilo a small real task:

```text
Look at @problems and propose the smallest reasonable fix plan.
```

5. After that, test the editing flow:

```text
Fix one specific issue from @problems and show the diff before applying the change.
```

### Which Agent to Use

- `ask` when you want an answer without changes
- `plan` when you want strategy or an implementation plan
- `code` when it is time to edit files
- `debug` when you are already working through a bug, logs, or terminal output

That split workflow often works better than trying to do everything in one mode.

## Useful Kilo Features to Know After Setup

### 1. Custom Agents

Kilo lets you create your own agents through:

- `Settings -> Agent Behaviour -> Agents`
- Markdown files in `.kilo/agents/`
- config entries

This is especially useful if you want a separate:

- `docs-writer`
- `reviewer`
- `test-engineer`
- `security-checker`

For LeanToken, that is helpful because the same provider setup can serve several specialized agent roles.

### 2. Shared Settings Between UI and CLI

The docs explicitly say the extension and CLI share the same configuration foundation. In practice, that means:

- you can configure the provider in the UI
- then use the same settings through the CLI
- or do the reverse

That is useful for developers who move back and forth between the editor and terminal.

### 3. Context Mentions

`@` mentions make Kilo more precise:

- `@/src/app.ts` for a specific file
- `@terminal` for the latest command output
- `@problems` for editor diagnostics
- `@git-changes` for uncommitted changes

If you want more accurate results from a LeanToken-backed model inside Kilo, this is one of the most practical tools to use.

### 4. Autocomplete

Kilo has a separate autocomplete feature. In the current docs it is described as Fill-in-the-Middle completion powered by Codestral.

That is not the same thing as chat or provider configuration. So the best path is to first make the main LeanToken-backed agent workflow stable, and only then evaluate autocomplete behavior separately.

### 5. Auto-Approve

Auto-Approve speeds up work, but Kilo also warns about risk. If you are working:

- in an important production repository
- around secrets
- with shell commands
- with multi-file automated edits

do not grant overly broad permissions unless you really mean to.

## Security and Operational Guidance

For Kilo + LeanToken, a few rules matter in practice.

### Do Not Store Keys in Git

Kilo uses config files such as `kilo.jsonc`, and the docs explicitly warn not to commit API keys or other secrets.

### Be Careful with Export and Import

Kilo documentation also warns that exported settings contain API provider profiles and API keys in plaintext. Treat those export files as sensitive secrets.

### Separate Experimental and Production Setups

If you work with multiple models or multiple teams, it is worth separating test providers and working providers so experiments do not affect the main workflow.

### Rotate the Key After Exposure

If the LeanToken key appears in:

- git history
- an issue tracker
- a screenshot
- an exported settings file sent to the wrong place
- shell history

it should be rotated and replaced in all Kilo configurations.

## Common Problems and How to Check Them

The official Kilo OpenAI-compatible provider page lists several common failure cases. LeanToken troubleshooting follows the same pattern.

### `Invalid API Key`

- make sure you pasted the current LeanToken key without extra spaces
- confirm that the key came from the `API Keys` page you actually intend to use

### `Model Not Found`

- verify the model ID
- confirm that the selected model is available through your LeanToken endpoint

### Connection Errors

- re-check the `Base URL`
- for the standard LeanToken setup, use `https://api.leantoken.tech/v1`
- only move to a full endpoint URL if your environment truly requires it

### Responses Work but Quality Is Poor

- the issue may be the chosen model rather than LeanToken itself
- try a stronger coding-oriented model
- check whether your current context window is too small for the task

### Context Fills Too Quickly

- reduce `Max Tokens`
- use shorter sessions
- switch to a model with more context
- start a new task after finishing one large block of work

## A Practical Kilo + LeanToken Workflow

Here is a simple workflow that usually proves the setup is real and usable:

1. Connect LeanToken as a custom provider.
2. Choose a model for coding work.
3. In `ask`, have Kilo summarize the current project structure.
4. In `plan`, ask for an implementation plan.
5. In `code`, allow one small change.
6. In `debug`, investigate one issue through `@terminal` or `@problems`.
7. Then evaluate whether the quality, speed, and cost are acceptable on your chosen model ID.

## What to Verify After Setup

- Kilo Code responds without authentication errors
- the selected model is actually available through your LeanToken key
- responses are coming through the connected custom or OpenAI-compatible provider, not the built-in Kilo provider
- the key is not exposed in `kilo.jsonc`, exported settings, public files, or team notes
- the selected agent and selected model match the type of task you are doing
- `@problems`, `@terminal`, and the normal file-context flow behave predictably

## Summary

Kilo Code is a strong fit for BYOK through LeanToken because the official product already has a direct and well-documented `OpenAI Compatible` provider path. For the initial setup, four things matter most: `Base URL`, `API key`, `Model`, and careful action permissions.

The most practical rollout looks like this:

1. first connect LeanToken as a custom provider
2. then choose a model that fits your coding workflow
3. only after that tune agents, auto-approve rules, and more advanced automation
