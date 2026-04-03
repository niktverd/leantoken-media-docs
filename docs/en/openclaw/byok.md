# OpenClaw

![Saved OpenClaw landing page with the product logo and tagline](../../images/openclaw/openclaw.png)

## About the Service

OpenClaw is a personal AI assistant that you run on your own devices. In the saved materials, the service is described as a self-hosted assistant with Gateway acting as the control plane: through it, OpenClaw connects models, communication channels, tools, skills, sessions, and WebChat.

OpenClaw can work in the channels you already use: WhatsApp, Telegram, Slack, Discord, Google Chat, Signal, iMessage, BlueBubbles, Microsoft Teams, Matrix, WeChat, WebChat, and others. The materials also mention voice scenarios on macOS, iOS, and Android, live Canvas, browser control, skills, and multi-agent routing.

The recommended setup path is consistent across the saved OpenClaw materials: first install the CLI, then run `openclaw onboard`, and inside onboarding configure Gateway, workspace, model access, channels, and skills.

## What You Will Need

- Node.js 24 is recommended; Node.js 22.16+ is supported
- macOS or Linux; the OpenClaw materials recommend WSL2 for Windows
- an account on `https://leantoken.tech`
- a LeanToken API key and, if needed, an OpenAI-compatible base URL
- a machine where Gateway will run: a local computer or a remote Linux host

## AI Use Cases

Based on the saved materials, OpenClaw is especially useful in these scenarios:

- a personal AI assistant in messengers and WebChat that replies in your usual channels instead of a separate web chat
- development help: codebase review, test runs, debugging, GitHub work, and pull request preparation
- DevOps and operational tasks: log inspection, service monitoring, alerts, and recurring runbook processes
- research work: information search and synthesis, note-taking, and knowledge workflows
- voice scenarios and device-local actions through companion apps and nodes
- browser automation, Canvas workflows, and other tool-driven processes

If you use BYOK through `leantoken.tech`, one gateway and one set of credentials can be reused across all of these scenarios instead of relying only on a bundled provider OAuth subscription.

## BYOK on leantoken.tech

In the OpenClaw context, BYOK means connecting your own API key for the model or gateway instead of relying only on the provider's OAuth subscription. In the saved GitHub material, OpenClaw explicitly separates two authentication paths:

- subscriptions through OAuth
- API keys

The same materials also mention:

- support for many providers and models
- auth profile rotation for OAuth and API keys
- model failover
- a recommendation to use the strongest current model available to you

For `leantoken.tech`, this is useful when you want to:

- keep model access separate from your personal OAuth subscription
- manage the key and endpoint centrally
- switch models without changing the overall integration pattern
- use one gateway for OpenClaw, channels, skills, and automation flows

The saved OpenClaw materials recommend building this setup around `openclaw onboard`, so the practical flow below is based on onboarding rather than undocumented manual edits to internal files.

## How to Get an API Key on leantoken.tech

Before configuring OpenClaw, get your key and endpoint in LeanToken.

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
8. Keep both the endpoint and the key private. If the key is exposed, rotate it and replace it in OpenClaw.

## How to Add BYOK

The flow below matches the saved OpenClaw materials best.

### 1. Install OpenClaw

```bash
npm install -g openclaw@latest
```

If you want to install the daemon right away and follow the recommended onboarding flow, use:

```bash
openclaw onboard --install-daemon
```

In the source materials, this is described as the preferred setup path: onboarding guides you step by step through Gateway, workspace, channels, and skills.

### 2. Choose the API-key path during model/auth setup

During onboarding, reach the model access stage. If you want BYOK through `leantoken.tech`, use the API-key route rather than an OAuth subscription.

In practice, that means:

1. Prepare your LeanToken API key in advance.
2. If the setup wizard asks for an endpoint or base URL for an OpenAI-compatible provider, use `https://api.leantoken.tech/v1`.
3. Paste your LeanToken API key at the provider authentication step.
4. Choose a model that is available through your LeanToken endpoint.

The saved OpenClaw materials do not show the exact names of every onboarding screen in every version, so the wording in the wizard may differ slightly. But the overall logic in the source materials is stable: first auth, then model selection, then channels and skills.

### 3. Finish configuring Gateway and channels

After model/auth setup, finish onboarding:

- install the Gateway daemon if it is not installed yet
- configure the workspace
- connect the channels you need, such as Telegram, Slack, Discord, WhatsApp, or WebChat
- add only the skills you actually plan to use

The saved OpenClaw materials explicitly state that Gateway is the control plane, while the assistant itself works across the connected channels, apps, and tools.

### 4. Verify that OpenClaw responds through your BYOK

For the first check, use the dashboard and the local Gateway.

The saved materials mention the local dashboard at:

```text
http://localhost:18789/
```

After onboarding, verify:

1. that Gateway is running
2. that model access works without an authentication error
3. that the assistant responds in the CLI, WebChat, or a connected channel

For a manual CLI check, you can use:

```bash
openclaw agent --message "Run a short configuration self-check and confirm that the model is responding" --thinking high
```

If you need a foreground debug run, the saved materials also include:

```bash
openclaw gateway --port 18789 --verbose
```

## Recommended BYOK Scenarios

BYOK through LeanToken is especially useful for OpenClaw in these tasks:

- one assistant serves several channels and model access is centralized behind one key and endpoint
- you want to control billing instead of depending only on an OAuth subscription
- you need to switch models quickly for coding, research, summaries, and background automation
- you want to keep the overall Gateway workflow in OpenClaw while using your own model access path

## Practical Recommendations

The saved OpenClaw materials separately emphasize security and operational hygiene. That matters even more in a BYOK setup.

- Do not expose Gateway directly to the public internet. The materials recommend keeping it on loopback and using Tailscale or SSH tunnels for remote access.
- Do not store your API key in a repository or paste it into public configs. Keep it as a secret and rotate it if it is leaked.
- Treat skills as code with access to your data and tools. Install only the skills you trust.
- Use stricter isolation for group and channel scenarios if the assistant has shell or automation tool access.
- Run `openclaw doctor` after setup if you want a quick way to surface risky or broken configuration.

## What to Check After Setup

- OpenClaw starts without errors on your Node.js version
- Gateway is available locally and opens the dashboard at `localhost:18789`
- the selected model actually responds through your LeanToken key
- the assistant is visible in the channel you connected
- the key and endpoint are stored as secrets and did not end up in git, shared notes, or shell history
