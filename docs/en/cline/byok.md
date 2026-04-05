# Cline

## About the Service

Cline is an open source AI coding agent that works directly inside your editor or terminal. According to the official documentation, Cline can read and modify files, run commands, use a browser, connect external tools through MCP servers, and help you develop software through a normal conversation flow.

That makes it a strong fit for BYOK because Cline does not lock you into one bundled provider. You can choose your own API provider, configure your own endpoint, paste your own API key, and use the model that best fits your coding workflow.

This page focuses on the path where Cline connects to `https://leantoken.tech` through the `OpenAI Compatible` provider. That is the most direct match for the current Cline settings UI and for the official OpenAI-compatible setup flow.

## When Cline Is Especially Useful

Cline is most useful when you need more than a simple editor chat panel.

- exploring an unfamiliar repository and understanding architecture through file reads, search, and follow-up questions
- implementing new features where it helps to discuss a plan first and then move into code changes
- debugging bugs through a combination of terminal commands, logs, tests, and iterative fixes
- refactoring where you want to experiment faster and recover safely through checkpoints
- working with web applications when browser tooling helps validate UI behavior
- extending your workflow with MCP servers for GitHub, documentation, databases, ticket systems, and internal tools

## How Work in Cline Is Organized

The official Cline docs describe several core systems that matter before you choose a model.

### Tasks

All work in Cline happens inside tasks. A task keeps:

- the original prompt
- the full conversation
- file changes
- executed commands
- decisions and intermediate context
- usage, cost, and execution time

This is useful for longer engineering work because you can come back later, review what changed, and continue with the same context instead of starting from scratch.

### Plan and Act

One of Cline's most useful workflows is the separation between `Plan` and `Act`.

- `Plan` is for exploring the codebase, discussing strategy, finding affected files, and shaping the implementation without editing files or running commands
- `Act` is for file changes, commands, tests, and actual execution once the plan is clear

In practice, this gives you a more predictable workflow. For medium and large tasks, it is usually better to start in `Plan` and switch to `Act` only after the approach makes sense.

### Checkpoints

Cline creates checkpoints by default. These are internal snapshots of project state that do not pollute your normal Git history. If the agent takes the wrong path, you can restore files, restore the task state, or restore both.

That matters for BYOK because it lets you work with more confidence:

- you can allow more autonomy on well-defined tasks
- you can test different approaches without losing control of the project state

### Other Systems Worth Knowing Early

Beyond provider configuration, Cline includes several systems that improve real-world development work:

- `Rules` for persistent project conventions and constraints
- `Skills` for specialized expertise loaded on demand
- `Workflows` for repeatable multi-step procedures
- `Hooks` for automatic validation and safety checks
- `.clineignore` for excluding noisy files and directories from context
- `Memory Bank` for preserving structured project context across sessions
- `MCP servers` for adding external tools and resources

In other words, Cline becomes much more powerful when you tailor it to your codebase instead of treating it as a one-off prompt box.

## What You Will Need

To configure Cline through BYOK, you usually need:

- VS Code, Cursor, or another compatible editor with Cline installed
- an account on `https://leantoken.tech`
- a LeanToken API key
- the OpenAI-compatible endpoint `https://api.leantoken.tech/v1`
- a model that performs well on coding tasks and behaves reliably in tool-calling workflows
- a repository or working folder opened in your editor

The steps below are written mainly for the VS Code-style interface shown in your screenshot.

## BYOK on leantoken.tech

In the official Cline documentation, BYOK means using your own keys and your own provider instead of the built-in `Cline Provider`.

For `leantoken.tech`, that means:

1. create your own LeanToken API key
2. choose `OpenAI Compatible` in Cline
3. set `https://api.leantoken.tech/v1` as the `Base URL`
4. paste your LeanToken key
5. select a model ID available through your LeanToken endpoint

This is useful when you want to:

- control your own billing and keys
- use one endpoint across several AI tools instead of only Cline
- switch models without changing the overall IDE workflow
- keep the provider layer separate from any bundled editor subscription

The official Cline docs also note that API keys are stored in the system credential manager and sent only to the selected provider. For LeanToken, that is the cleanest and most direct integration path.

## How to Get an API Key on leantoken.tech

Before configuring Cline, get your key and endpoint in LeanToken.

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
8. Keep both the endpoint and the key secret. If the key appears in logs, screenshots, git history, or shared notes, rotate it and replace it in Cline.

## How to Install Cline

According to the current official docs, Cline supports several environments, including VS Code, Cursor, JetBrains IDEs, CLI, Zed, Neovim, VSCodium, and Windsurf. For the screenshot you provided and for the most direct LeanToken flow, the VS Code-style extension path is the best reference.

The basic install flow is:

1. Open your editor.
2. Go to `Extensions`.
3. Search for `Cline`.
4. Install the extension.
5. Open Cline from the activity bar or command palette.

If the icon does not appear after installation:

- allow extension execution if the editor asks for it
- restart the editor
- open Cline through the command palette

## How to Configure LeanToken in Cline

The screenshot below shows the exact Cline settings screen needed for LeanToken as an OpenAI-compatible provider.

![Cline Settings screen with OpenAI Compatible selected, the LeanToken Base URL, the OpenAI Compatible API Key field, and qwen3-235b as the model](../../images/cline/cline-openai-compatible-settings.png)

The key fields visible in the screenshot are:

- `API Provider`
- `Base URL`
- `OpenAI Compatible API Key`
- `Model ID`
- `Custom Headers`
- the `MODEL CONFIGURATION` and `ADVANCED` sections below

### Step-by-Step Setup

1. Open Cline in the editor sidebar.
2. Click the gear icon in the top-right corner of the Cline panel.
3. Open the `API Configuration` section.
4. In `API Provider`, choose `OpenAI Compatible`.
5. In `Base URL`, enter `https://api.leantoken.tech/v1`.
6. In `OpenAI Compatible API Key`, paste your LeanToken API key.
7. In `Model ID`, enter the model you want to use. In your screenshot, the example value is `qwen3-235b`.
8. Leave `Custom Headers` empty unless your LeanToken setup explicitly requires additional headers.
9. Leave `Set Azure API version` and `Use Azure Identity Authentication` off if you are using the normal OpenAI-compatible LeanToken endpoint.
10. Click `Done` or close settings if the interface has already saved the state.
11. Return to the Cline chat and send a simple message to verify the connection.

The minimum working configuration looks like this:

```text
API Provider: OpenAI Compatible
Base URL: https://api.leantoken.tech/v1
OpenAI Compatible API Key: <your_LeanToken_API_key>
Model ID: <your_model_id>
```

### What the Additional Fields Mean

The official Cline docs for OpenAI-compatible providers say that the settings panel may also expose additional model configuration fields such as:

- `Max Output Tokens`
- `Context Window`
- `Image Support`
- `Computer Use`
- `Input Price`
- `Output Price`

These are not required for the first successful connection. To get started, the essential part is a correct provider, base URL, API key, and model ID.

Your screenshot also shows the option `Use different models for Plan and Act modes`. That is useful if you want:

- a stronger reasoning model for `Plan`
- a faster or cheaper implementation model for `Act`

For the first setup, however, it is usually better to make one model work reliably before splitting the workflow across multiple models.

## How to Choose a Model ID for Cline

There is no single best model ID for every Cline workflow. A few practical rules matter more than the name of the model itself.

- Choose a model that is strong at coding tasks, not just ordinary chat.
- For Cline, reliability in tool-driven workflows matters as much as answer quality.
- If you work on large repositories and long tasks, context window size matters.
- If you use `Plan` and `Act` heavily, speed and cost become important over long sessions.

Your screenshot also shows a warning inside Cline saying that Cline uses complex prompts and tends to work best with stronger models. In practical terms, that means:

- start with a model you already trust for agentic coding work
- if a model can answer but struggles with long multi-step work, move to a stronger coding-oriented option

## First Verification After Setup

Once LeanToken is connected, do not start with a large task immediately. First verify the basic path.

1. Send a simple message and confirm that Cline responds through the new provider.
2. Ask Cline to read a few files in the current repository and explain the project structure.
3. Use `Plan` first if you want to test analysis safely without file modifications.
4. Then give it a very small `Act` task, such as changing text, adding a small guard clause, or updating a comment.
5. Only move on to larger work after that basic loop behaves correctly.

Good first prompts include:

- `Read package.json and explain how this project starts`
- `Find the main entry point and summarize the initialization flow`
- `Create a short plan for adding a simple validation check to this module`

## A Practical Daily Workflow

After the initial setup, a strong Cline + LeanToken workflow usually looks like this:

1. Open the repository and start a task with a clearly scoped objective.
2. For medium and large work, begin in `Plan`.
3. Ask Cline to find relevant files and propose an implementation path.
4. Clarify constraints such as coding style, tests, architecture rules, and what must not change.
5. Switch to `Act` once the plan is acceptable.
6. Review the outcome and restore from checkpoints if you want to try a different implementation path.

For larger tasks, it also helps to use:

- `Memory Bank` for long-lived project context
- `Rules` for persistent conventions and constraints
- `Workflows` for repeatable procedures
- `Hooks` for automatic validation and guardrails
- `MCP servers` when you want Cline to interact with external systems

## How to Reduce Cost and Context Noise

One of the most common Cline problems is not the model itself, but irrelevant context. The official docs specifically recommend using `.clineignore` so Cline does not waste context on files that do not help solve the task.

In `.clineignore`, it usually makes sense to exclude:

- `node_modules/`
- `dist/`
- `build/`
- `.next/`
- `coverage/`
- `.env` and similar secret files
- large binary assets, data dumps, and generated files

This helps for three reasons:

- lower request cost
- faster responses
- less risk of useful context getting pushed out by irrelevant files

In a large project, a good `.clineignore` often has a bigger practical effect than changing models.

## Safety and Control

Cline is powerful enough that permission strategy matters.

- Do not enable aggressive auto-approve on an unfamiliar codebase.
- Do not turn on `YOLO mode` unless you are comfortable with fully automatic approval.
- For serious work, start by manually approving edits and commands.
- If you want more speed later, enable auto-approve gradually and only for actions whose risk you understand.
- Keep checkpoints on if you actively experiment with implementations.
- Treat MCP servers and Hooks with the same caution you would apply to any external execution path.

For LeanToken, combine that with standard key hygiene:

- do not store the API key in the repository
- do not paste it into `.env.example`, README files, or issue trackers
- do not expose it in screenshots or shared notes
- rotate it immediately if it was leaked

## Common Issues and What to Check

### Cline Does Not Respond After Setup

Check whether:

- `API Provider = OpenAI Compatible`
- the `Base URL` is correct
- `/v1` is included where required
- the API key is valid
- the chosen model ID is actually available in your LeanToken account

### `Invalid API Key` or Similar Authorization Errors

- copy the key again from LeanToken
- make sure you pasted an API key and not some other token
- if the key may have leaked or the configuration looks corrupted, create a new key and replace the old one

### `Model Not Found`

- verify the exact model ID in your LeanToken account
- make sure the model is really available through your endpoint
- if in doubt, start with a model you already know works and only then experiment further

### Sessions Feel Too Expensive or Too Long

- add `.clineignore`
- start new tasks instead of endlessly extending one conversation
- use `Plan` before `Act` so expensive tool usage happens only after the approach is clear
- if needed, configure different models for `Plan` and `Act`

### The Model Answers but Performs Poorly on Engineering Work

This is common. For Cline, what matters is not only response quality but also agentic behavior. If the model:

- loses track after several steps
- fails to follow a plan
- behaves weakly with commands and file context
- produces unstable edits

move to a stronger coding-oriented model in LeanToken.

## Where Cline and LeanToken Work Especially Well Together

The Cline + LeanToken combination is especially useful when:

- one OpenAI-compatible endpoint is reused across multiple team tools
- you want to swap model IDs centrally without changing the IDE workflow
- you want a clear separation between editor workflow and provider-side model access
- you want BYOK not only for Cline, but for a broader developer-tooling stack

In that setup, LeanToken becomes more than just a place to store a key. It becomes a shared control layer for model access across your developer tools.
