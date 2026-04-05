# SourceCraft Code Assistant (Yandex Code Assistant)

## About the Service

SourceCraft Code Assistant is an AI developer assistant available inside the SourceCraft platform and as an IDE plugin. On the current official product page, it is described as an assistant with agent mode, built-in chat, code review, context-aware suggestions, and support for popular editors including VS Code and JetBrains IDEs.

If you are used to the old name, this is the same product: SourceCraft's FAQ explicitly says that `Yandex Code Assistant` became part of the SourceCraft platform and was renamed to `SourceCraft Code Assistant` without a loss of functionality.

For LeanToken, the important part is that the current SourceCraft documentation already describes model profiles and support for external AI providers. That makes BYOK through `https://leantoken.tech` a natural setup path rather than a workaround.

## Where SourceCraft Code Assistant Is Especially Useful

- agent-style work on natural-language tasks directly inside the IDE
- exploring a repository and understanding project structure through built-in chat
- code review and fast AI suggestions without switching to a separate service
- `Next Edit Action` and inline completion while editing code
- generating documentation, tests, and repetitive code changes from project context
- connecting MCP servers and using different LLMs for different working modes

## What Matters Before You Configure BYOK

Before connecting LeanToken, it helps to understand two basic SourceCraft Code Assistant systems.

### Model Profiles

According to the official Visual Studio Code documentation, the assistant uses `configuration profiles`. A profile can store:

- an AI provider
- an API key
- a model
- temperature and other model settings
- a rate limit
- additional provider-specific options

That is useful for BYOK because you do not need to keep replacing one global setup. You can create a separate profile such as `LeanToken`.

### Working Modes

In the SourceCraft agent documentation, the built-in modes are:

- `Code`
- `Ask`
- `Architect`
- `Debug`
- `Orchestrator`

Each mode can remember its own profile and model. In practice, that means LeanToken can be used to:

- keep a stronger model for `Code` or `Architect`
- use a faster or cheaper model for `Ask`
- test different profiles without breaking the default configuration

## What You Need

- Visual Studio Code with the SourceCraft Code Assistant plugin installed
- an account on `https://leantoken.tech`
- a LeanToken API key
- the OpenAI-compatible endpoint `https://api.leantoken.tech/v1`
- a model ID available in your LeanToken account

The setup flow below is written for VS Code. That is the most reliable starting point for this page because the official model-profile documentation is currently published for Visual Studio Code, and the saved screenshot also comes from that interface.

## BYOK on leantoken.tech

In SourceCraft Code Assistant, Bring Your Own Key means that you do not rely only on the built-in Yandex model profile. Instead, you create your own profile for an external OpenAI-compatible provider.

For LeanToken, that looks like this:

1. create your own API key in LeanToken
2. create a separate profile in Code Assistant
3. choose `OpenAI Compatible`
4. set `https://api.leantoken.tech/v1`
5. paste your LeanToken API key
6. choose the model you want
7. optionally link the profile to the working modes you use most

This is useful when you want to control:

- the model used for a specific workflow
- costs and quotas
- switching between several profiles
- the boundary between bundled and external models

## How to Get an API Key on leantoken.tech

First get your key and endpoint in LeanToken.

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
8. Return to SourceCraft Code Assistant settings and paste the key into the `OpenAI API Key` field.
9. Treat the key as a secret. If it appears in git, logs, screenshots, or shell history, rotate it.

## How to Connect LeanToken in SourceCraft Code Assistant

The saved screenshot below still uses the old `Yandex Code Assistant Settings` title, but the setup path remains relevant: the product has been renamed to SourceCraft Code Assistant, while the model-profile screen is still suitable for the BYOK flow.

![SourceCraft Code Assistant settings screen with a Custom profile, OpenAI Compatible provider, LeanToken Base URL, and a selected model](../../images/yandex-code-assistant/yandex-code-assistant.png)

### Step-by-Step Setup

1. Open the SourceCraft Code Assistant chat in Visual Studio Code.
2. In the top chat panel, open `Settings`.
3. In the `Models` section, click `+` next to the `Profile` selector.
4. Create a new profile. A practical name is `LeanToken`.
5. In `API Provider`, choose `OpenAI Compatible`.
6. In `Base URL`, enter `https://api.leantoken.tech/v1`.
7. In `OpenAI API Key`, paste your LeanToken API key.
8. In `Model`, choose a model available in your LeanToken account.
9. If you are not sure about advanced options yet, leave the default values for `temperature`, `rate limit`, and other provider-specific settings.
10. Save the profile and return to chat.

The minimum working configuration looks like this:

```text
Profile: LeanToken
API Provider: OpenAI Compatible
Base URL: https://api.leantoken.tech/v1
OpenAI API Key: <your_LeanToken_API_key>
Model: <your_model_id>
```

According to the official documentation, API keys in this flow are stored in `VS Code Secret Storage` rather than exposed as plain text in the interface.

## How to Use Profiles Together with Modes

If you already use agent mode, it is useful to link your LeanToken profile to specific workflows:

- `Code` for implementation and file changes
- `Ask` for explanations without editing the project
- `Architect` for planning and system design
- `Debug` for diagnosing problems

A practical first setup is:

- one LeanToken profile for all modes at the start
- then a stronger profile for `Code`
- and a faster profile for `Ask`

## First Smoke Test After Setup

Once the profile is saved, do not start with a large task immediately. First verify the basic path.

1. Send a simple prompt such as `Read README and briefly explain how this project starts`.
2. Ask the assistant to show relevant files or a short implementation plan.
3. If you already use modes, confirm that the correct profile is selected in `Code` or `Ask`.
4. Then move to a small practical task such as updating text, generating a test, or explaining one module.

## What to Check If Responses Do Not Arrive

- Make sure `Base URL` is exactly `https://api.leantoken.tech/v1`.
- Check that the API key was pasted fully and without extra spaces.
- Verify that the selected model ID is available in your LeanToken account.
- If the profile was created a while ago, reopen settings and refresh the model selection.
- If you need stricter data-handling rules, review the current SourceCraft settings related to Yandex-hosted models and external providers.

## What Can Be Expanded Later

This page already covers the basic LeanToken BYOK flow. If needed, the next iteration can add:

- a separate JetBrains-specific setup
- examples of profiles for different modes
- a dedicated MCP servers section
- a comparison between the built-in Yandex profile and an external LeanToken profile
