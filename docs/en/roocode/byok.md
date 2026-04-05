# Roo Code

## About the Service

Roo Code is an open source AI coding suite for LLM-assisted software development. Officially, the product is available in two main formats:

- a local extension for VS Code and compatible editors
- Roo Code Cloud Agents for autonomous work through the web, Slack, GitHub, and other integrations

The main idea behind Roo Code is a model-agnostic setup. Roo itself is not a model: it needs an inference provider, and you choose the provider, model ID, API key, and related settings yourself. That matters for BYOK because Roo Code does not lock you into one cloud provider or one bundled subscription.

In practice, Roo Code does much more than a basic AI chat panel in an editor. According to the official documentation, it can:

- generate code from natural-language task descriptions
- explain the structure and behavior of an existing codebase
- refactor and fix bugs
- update documentation
- run commands
- work with MCP servers
- execute multi-step workflows through different modes

This page focuses on the local Roo Code extension because that is the most direct and predictable BYOK path through `leantoken.tech` and an OpenAI-compatible endpoint.

## When Roo Code Is Especially Useful

Roo Code is a strong fit for these scenarios:

- exploring an unfamiliar repository and quickly understanding architecture, entry points, and dependencies
- fixing bugs through the `@problems` and `@terminal` flow, plus file reads and command execution
- implementing small and medium-sized tasks from a spec without manually hopping across many files
- refactoring and preparing pull requests in a step-by-step workflow
- generating tests, changelogs, release notes, and technical documentation
- reviewing UI screenshots and images if the selected model supports vision
- multi-step work where it helps to separate planning, coding, debugging, and orchestration into different modes

Put simply, Roo Code is most useful when autocomplete is no longer enough and you need an agent-style workflow with access to files, commands, and real project context.

## Roo Code Work Formats

Before you configure BYOK, it helps to understand the difference between the two main product forms.

### 1. Roo Code VS Code Extension

This is the local scenario. Roo Code runs directly inside your editor, uses the current workspace, proposes tools, asks for approval on actions, and lets you control the process step by step.

This is the best fit for LeanToken BYOK because you directly configure:

- the provider
- the base URL
- the API key
- the model ID
- model and profile settings

### 2. Roo Code Cloud Agents

This is the cloud format for autonomous agent work through the web and external integrations. It is useful for parallel execution, Slack tasks, and GitHub-based teamwork, but this document does not cover the LeanToken setup flow for that path.

## What You Will Need

To configure Roo Code through BYOK, you usually need:

- VS Code `1.84.0` or newer, or a compatible editor
- the Roo Code extension installed
- an account on `https://leantoken.tech`
- a LeanToken API key
- the OpenAI-compatible endpoint `https://api.leantoken.tech/v1`
- a model available through your LeanToken endpoint that supports OpenAI-compatible native tool calling

The last point is critical. Roo Code explicitly states in its official documentation that it uses only native tool calling. There is no XML-based fallback. A model may respond to normal chat requests and still be a poor Roo Code choice if it does not handle tools correctly.

## BYOK on leantoken.tech

In the Roo Code context, BYOK on `leantoken.tech` means that you:

1. create your own API key in LeanToken
2. connect LeanToken as an `OpenAI Compatible` provider
3. set `https://api.leantoken.tech/v1` as the Base URL
4. choose a model available through your endpoint
5. use Roo Code as a normal coding agent inside the editor, but on top of your own key and your own model access path

This is useful for several reasons:

- you control the key and endpoint yourself
- you can switch model IDs without changing the overall workflow in the IDE
- the same LeanToken key can be reused in other OpenAI-compatible integrations
- you are not limited to Roo Code Router or a specific first-party subscription

If you want the easiest setup with no API key, Roo Code recommends Roo Code Router. But if your goal is BYOK and control over your own endpoint, LeanToken belongs in the `OpenAI Compatible` path.

## How to Get an API Key on leantoken.tech

Before configuring Roo Code, get your key and endpoint in LeanToken.

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
8. If you plan to run multiple workflows, decide in advance whether you want one shared key or separate keys for different IDEs and projects.
9. Keep the key private. If it ends up in git, shell history, logs, issue trackers, or screenshots, rotate it.

## How to Install Roo Code

The official Roo Code documentation recommends two main install paths.

### Through VS Code Marketplace

1. Open VS Code.
2. Go to `Extensions`.
3. Search for `Roo Code`.
4. Select the extension by `RooVeterinaryInc`.
5. Click `Install`.
6. If the editor asks for reload, confirm it.

After installation, the Roo Code icon appears in the activity bar.

### Through Open VSX

This path is useful for editors without Marketplace access, including some VS Code-compatible forks.

1. Open the extensions view in your editor.
2. Search for `Roo Code`.
3. Install the extension.
4. Reload the editor if needed.

### Installation from VSIX

If you need the manual path:

1. Download the `.vsix` file from the official Roo Code GitHub Releases page.
2. In VS Code, open the extensions menu.
3. Choose `Install from VSIX...`.
4. Select the downloaded file.

## How to Configure LeanToken in Roo Code

Below is the main working flow for Roo Code with LeanToken.

![Providers screen in Roo Code Settings with the default profile, OpenAI Compatible, the LeanToken base URL, and a selected model](../../images/roocode/roocode.png)

Your screenshot already shows the exact screen needed for BYOK:

- the `Providers` section
- the `default` profile
- `API Provider = OpenAI Compatible`
- `Base URL = https://api.leantoken.tech/v1`
- a filled `API Key` field
- the `qwen3-235b` model selected

### Step-by-Step Setup

1. Open the Roo Code panel in VS Code.
2. Click the gear icon to open `Settings`.
3. Go to the `Providers` tab.
4. In `Configuration Profile`, keep `default` or create a new profile with the `+` button.
5. In `API Provider`, choose `OpenAI Compatible`.
6. In `Base URL`, enter `https://api.leantoken.tech/v1`.
7. In `API Key`, paste your LeanToken key.
8. In `Model`, select the model ID you want to use.
9. Click `Save`.

The minimum working parameter set looks like this:

```text
API Provider: OpenAI Compatible
Base URL: https://api.leantoken.tech/v1
API Key: <your_LeanToken_API_key>
Model: <your_model_id>
```

### What the Additional Fields Mean

According to the Roo Code docs for OpenAI-compatible providers and based on your screenshot, the UI may show extra model settings and metadata such as:

- `Max Output Tokens`
- `Context Window`
- `Image Support`
- `Computer Use`
- `Input Price`
- `Output Price`

Your screenshot also shows service toggles and provider-specific options such as:

- `Enable streaming`
- `Use legacy OpenAI API format`
- `Include max output tokens`
- extra switches for specific model families

That means you should verify not only that the provider connects, but also how Roo interprets the capabilities of the specific model ID.

### Practical Advice on Model Selection

If this is your first LeanToken setup in Roo Code:

1. start with a model you have already tested in tool-driven tasks
2. do not judge a model only by the quality of its chat replies
3. separately verify file reads, tool use, and multi-step workflow stability

If a model answers basic prompts but behaves badly with tools, the real value of Roo Code drops quickly.

## Configuration Profiles

One of the most useful Roo Code features for BYOK is `API Configuration Profiles`.

Profiles let you keep several configuration sets at the same time. Each profile can have its own:

- provider settings
- API keys
- model selection
- temperature-like settings
- rate limit settings
- provider-specific options

This is especially useful when you want to:

- keep a dedicated profile for LeanToken
- use different models for different kinds of work
- switch quickly between providers without manually rewriting fields every time

### How to Use Profiles in Practice

For Roo Code with LeanToken, a practical structure looks like this:

- `default` or `leantoken-main` for the main coding workflow
- a separate profile for a cheaper or faster model
- a separate profile for vision tasks if you have a compatible model

The official Roo Code docs also note that:

- profiles can be created with the `+` button next to the selector
- profiles can be switched not only in `Settings`, but also from the dropdown in the chat interface
- frequently used profiles can be pinned to stay at the top of the list
- profiles can be renamed through the pencil icon
- profiles can be deleted through the trash icon
- tasks remember the profile they started with
- different modes can be attached to different profiles

This reduces the risk of switching models accidentally in the middle of a long task.

### Where Roo Stores Keys

According to the Roo Code docs, API keys are stored in `VSCode Secret Storage` and should not appear as plain text in standard settings. That does not remove the need for basic security hygiene, but it is better than placing the key directly in project files.

## Roo Code Modes

Roo Code is built around working modes. Each mode defines behavior, tool access, and the style of execution.

### Code

This is the default mode.

It is a good fit for:

- writing code
- refactoring
- fixing bugs
- most everyday development work

This mode has full access to `read`, `edit`, `command`, and `mcp`.

### Ask

This mode is for explanation and context exploration without permission to edit files or run commands.

It is useful for:

- explaining unfamiliar code
- discussing architecture
- checking ideas
- safely reading a project without accidental edits

### Architect

This mode is for design and planning work.

It is useful for:

- breaking a large task into steps
- system design
- planning changes before implementation

### Debug

This mode is for diagnosing and narrowing down problems.

It is useful for:

- test failures
- build errors
- log analysis
- tracking down unstable behavior

### Orchestrator

This mode is for decomposing larger tasks and delegating subtasks to other modes.

It is useful for:

- long feature tasks
- large refactors
- chains of work where planning, coding, and debugging happen in sequence

This mode does not have direct access to ordinary file and command tools. Instead, it delegates subtasks to other modes.

### How to Switch Modes

Roo Code documents four main switching paths:

1. the dropdown to the left of the chat input
2. slash commands such as `/code`, `/ask`, `/architect`, `/debug`, and `/orchestrator`
3. `Cmd + .` on macOS or `Ctrl + .` on Windows and Linux
4. accepting a mode-switch suggestion that Roo offers during the workflow

### Why Modes Matter for BYOK

Roo Code remembers the last model selection for each mode. That lets you do things like:

- keep a stronger reasoning model for `Architect`
- keep a faster model for `Code`
- keep a separate profile for `Debug`

For LeanToken, this is useful if you want to control cost and quality at the workflow level instead of forcing one universal model ID for everything.

## How Roo Code Uses Tools

The Roo Code docs describe the tool model very clearly: Roo does not just answer with text. It calls tools to work with code and the environment.

The main categories are:

- `read` for reading and searching files
- `edit` for creating and modifying files
- `command` for executing terminal commands
- `image` for AI image generation
- `mcp` for Model Context Protocol integrations
- workflow tools for follow-up questions, task completion, mode switching, and creating subtasks

### What the Workflow Looks Like

A typical workflow looks like this:

1. you describe the task in natural language
2. Roo selects the needed tool
3. it shows the tool parameters
4. it waits for your approval
5. it executes the action
6. it continues until the task is complete

By default, this is safer than a fully autonomous flow because you control:

- which files will be changed
- which commands will be run
- which proposed actions are actually executed

## Why Native Tool Calling Is Critical

This is one of the most important technical details for Roo Code.

In the `OpenAI Compatible` documentation, Roo explicitly states:

- Roo uses only native tool calling
- XML-based fallback is not supported
- if a model does not support OpenAI-compatible function or tool calling, it cannot be used with Roo Code

That leads to a practical LeanToken conclusion:

- compatibility with an OpenAI-compatible chat endpoint alone does not guarantee that Roo Code will work well
- you must verify compatibility with native tool calling separately
- if a model produces tool-calling errors, it is usually better to switch the model ID than to keep forcing the same setup

## Useful Mentions in Roo Code

One of Roo Code's strongest features is contextual mentions. They can pull in not only normal text, but also files, folders, images, diagnostics, and git context.

### The Most Useful Mentions in Practice

- `@/path/to/file.ts` to add a file to context
- `@/path/to/folder/` to add the contents of files in a directory
- `@/path/to/image.png` to add an image if the model supports vision
- `@problems` to include diagnostics from the VS Code Problems panel
- `@terminal` to include recent terminal output
- `@git-changes` to include uncommitted changes
- `@https://example.com` to import the contents of a web page

### Real Prompt Examples

```text
@problems Fix all build errors and explain the root cause.
```

```text
Explain what @/src/app.ts does and identify the main architecture risks here.
```

```text
Compare @git-changes with the current task and suggest a good commit message.
```

```text
What is wrong with this screen? @/screenshots/bug.png
```

For Roo Code with LeanToken, this is especially useful when you want answers grounded in the actual repository, terminal output, and current local changes instead of generic advice.

## Additional Capability: Codebase Indexing

Roo Code can build a semantic index of the codebase. According to the official docs, this mechanism:

1. parses code through Tree-sitter
2. creates embeddings for semantic blocks
3. stores vectors in Qdrant
4. gives Roo access to `codebase_search`

This is a separate capability, not required for the base BYOK setup. But it becomes useful for larger repositories where file-by-file reading is no longer enough.

It is important to understand the architecture of this feature:

- you need an embedding provider
- you need a vector database, usually Qdrant
- if you want an OpenAI-compatible embedder, you need a compatible endpoint and a compatible embedding model

For LeanToken, this means indexing should be configured separately from the main chat and profile path. First make the ordinary Roo workflow stable, then add embeddings and vector search.

## A Practical Starter Workflow

After configuring BYOK, do not start with a large feature task. It is safer to run a short verification sequence first.

### Step 1. Check Context Reading

Ask Roo to read one or two files:

```text
Explain the structure of this project and start with @/package.json and @/src/
```

### Step 2. Check Diagnostics

If the project has errors:

```text
@problems Analyze these errors and suggest the right fix order.
```

### Step 3. Check Tool Use

Give Roo a small and safe task:

```text
Add a short comment in this file and show the exact changes you plan to make.
```

### Step 4. Check Commands

If you are ready to approve a command:

```text
Run the tests for this package and briefly explain what failed.
```

This lets you confirm quickly:

- whether Roo can see the files
- whether the model calls tools correctly
- whether the provider path through LeanToken is stable

## Security and Operations

Roo Code is approval-based by default, and that is a good security model.

### What Should Stay Enabled by Default

- manual approval for edits
- manual approval for commands
- careful review of all proposed tool actions

### What to Understand About Auto-Approve

Roo Code includes a direct warning in its documentation: auto-approve speeds things up, but it significantly increases risk. For terminal commands and file changes, it can lead to data loss, corrupted files, or other unwanted side effects.

For a LeanToken setup, the practical recommendation is simple:

- first get a stable manual workflow
- only then enable selective auto-approve
- do not enable aggressive auto-approve on unfamiliar projects or production-sensitive setups

### Basic Security Rules

- do not store the LeanToken key in a repository
- do not paste the key into project config files unless you really need to
- do not publish screenshots with an unmasked key
- rotate the key immediately if it leaks
- use separate profiles for different kinds of work if you want to isolate access

## Common Problems and How to Check Them

### Roo Code Does Not Connect to LeanToken

Check:

- whether `Base URL` is correct
- whether the API key is correct
- whether `https://api.leantoken.tech/v1` is reachable
- whether the key was accidentally truncated while copying

### Roo Code Sees the Model but Works Poorly with Tools

This is usually a reason to check:

- whether the specific model ID supports native tool calling
- whether the provider limits tool or function-calling semantics
- whether the model is suitable for multi-step coding workflows rather than one-off chat replies

### `Model Not Found` Error

Check the exact model ID exposed by your LeanToken endpoint. This error is often caused by the model name, not by the key.

### Connection Errors

Check:

- whether the endpoint contains a typo
- whether the endpoint is reachable from your network
- whether a proxy or firewall is blocking IDE requests

### Context Fills Up Too Quickly

Roo Code separately recommends being deliberate with `Max Tokens` and context window settings. If you use very large thinking budgets, you consume task history space more quickly.

In practice, it helps to:

- keep higher thinking limits for `Architect` and `Debug`
- use more moderate settings for `Code`
- split work into several tasks instead of one endless session

## What to Check After Setup

- Roo Code opens without errors and is visible in the sidebar
- a profile with `OpenAI Compatible` is saved in `Providers`
- `Base URL` is set to `https://api.leantoken.tech/v1`
- the model responds to a short test prompt
- Roo can read files and propose tools
- tool-driven operations do not fail because of model incompatibility
- the key is stored as a secret and did not leak into git, logs, or notes

## Final Recommendation

For Roo Code with LeanToken, the most reliable starting path is:

1. install Roo Code in VS Code
2. create a dedicated profile for LeanToken
3. choose `OpenAI Compatible`
4. set `https://api.leantoken.tech/v1`
5. paste the LeanToken API key
6. select a model already verified for native tool calling
7. run a short test workflow with files, diagnostics, and commands

If that base setup is stable, you can grow the configuration further:

- separate profiles for different modes
- vision scenarios
- indexing
- selective auto-approve
- custom modes and more advanced workflows
