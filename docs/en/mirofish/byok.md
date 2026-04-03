# MiroFish

![MiroFish logo](../../images/mirofish/mirofish.png)

## About the Service

MiroFish is an open-source multi-agent AI prediction engine. According to the saved materials, the project builds a digital model of society from input data such as news, draft laws, analytical reports, or financial signals, and then runs a simulation of thousands of autonomous agents inside that model.

Each agent gets a role, behavioral logic, long-term memory, and relationships with other entities. As a result, MiroFish is not just a single-prompt AI tool. It runs a full workflow: builds a knowledge graph, generates the environment, simulates social interactions, produces a report, and lets you talk to agents and to ReportAgent after the simulation is finished.

Based on the saved sources, the service combines:

- a frontend built with Vue.js 3, Vite, D3.js, and Tailwind CSS
- a backend built with Python 3.11+ and Flask
- the OASIS simulation engine from CAMEL-AI
- memory and GraphRAG through Zep Cloud
- OpenAI-compatible LLM orchestration, which can work with BYOK through `leantoken.tech`

![Saved MiroFish interface screenshot from the project materials](../../images/mirofish/mirofish-overview.webp)

From an end-user perspective, MiroFish is closer to a digital sandbox for modeling than to a regular AI chat: you provide source materials and a prediction goal, and then observe how the digital society reacts to the event and what outcomes may follow.

## How MiroFish Works

In the saved documentation, MiroFish is described as a five-stage workflow:

1. `Graph Building` — the service extracts entities and relationships from the source materials, builds GraphRAG, and injects collective memory.
2. `Environment Setup` — it generates agent personas, roles, and simulated platform settings.
3. `Simulation Execution` — it runs parallel agent interactions and updates their memory between rounds.
4. `Report Generation` — ReportAgent analyzes the simulation logs and produces a structured predictive report.
5. `Deep Interaction` — after the run, you can talk directly with specific agents or with ReportAgent to explore the scenario in more detail.

According to the saved materials, the MiroFish interface is built as a step-by-step wizard. The frontend typically runs at `http://localhost:3000`, and the backend API at `http://localhost:5001`.

## AI Use Cases

Based on the README, DeepWiki, and the saved article, MiroFish is especially useful in these scenarios:

- modeling public reaction to news, political initiatives, regulatory changes, and crisis-driven information events
- testing PR and communication strategies before real-world launch, when it is important to see coalitions, conflicts, and pressure points in advance
- working with financial and market signals where you need more than a short summary and want to simulate downstream participant behavior
- exploring social platforms and debates through a layered agent model rather than a single generic answer
- creative and narrative scenarios: alternate storylines, continuations, and "what if" explorations

Important: in the saved materials, MiroFish is positioned as a prediction engine and digital sandbox, but those same materials also explicitly note that this is an early version with no documented backtesting. It is better treated as a scenario-analysis and rehearsal tool than as a guarantee of accurate real-world prediction.

## What You Will Need

To run MiroFish with BYOK according to the saved materials, you need:

- Node.js 18+
- Python 3.11+; the saved README also references the 3.11-3.12 range
- `uv` for Python dependencies
- `npm`
- an account on `https://leantoken.tech`
- a LeanToken API key
- a LeanToken OpenAI-compatible endpoint if you connect MiroFish through an external provider
- a model identifier available through your endpoint
- a separate `ZEP_API_KEY` for Zep Cloud, because memory and GraphRAG are handled by a separate service in MiroFish

If you prefer a container-based setup, the saved materials also document a Docker path through `docker compose`.

## BYOK on leantoken.tech

For MiroFish, BYOK is not a UI field inside the app. It is LLM provider configuration in the root `.env` file of the project. The saved materials explicitly state that the service supports any LLM API in OpenAI-compatible format and uses these variables:

- `LLM_API_KEY`
- `LLM_BASE_URL`
- `LLM_MODEL_NAME`

This is where you can supply the key, endpoint, and model from `leantoken.tech`.

In practice, that means:

- `LLM_API_KEY` — your LeanToken API key
- `LLM_BASE_URL` — an OpenAI-compatible endpoint such as `https://api.leantoken.tech/v1`
- `LLM_MODEL_NAME` — a model available on your LeanToken endpoint

At the same time, BYOK through LeanToken only covers the LLM side of MiroFish. According to the saved materials, the service still requires a separate `ZEP_API_KEY`, because GraphRAG and long-term agent memory are handled through Zep Cloud.

BYOK through `leantoken.tech` is convenient for MiroFish when you want to:

- connect an OpenAI-compatible model without hardwiring yourself to the originally recommended provider
- manage the key and endpoint centrally
- switch models without rewriting the overall MiroFish logic
- reuse the same key in other OpenAI-compatible tools

## How to Get an API Key on leantoken.tech

Before configuring MiroFish, get your key and endpoint in LeanToken.

![LeanToken landing page with Sign up, Log in, and Dashboard](../../images/leantoken/leantoken.tech.1.landing.png)

1. Open `https://leantoken.tech`.
2. Click `Sign up` if you do not have an account yet, `Log in` if you already have one, or `Dashboard` if you are already signed in.

![LeanToken dashboard overview with API endpoint and quick links](../../images/leantoken/leantoken.tech.2.dashboard.png)

3. After signing in, open the dashboard or API overview page.
4. Copy the API endpoint. In the screenshots, it is `https://api.leantoken.tech/v1`.
5. Go to `Manage keys` or `API Keys`.

![LeanToken API Keys page with Create key action](../../images/leantoken/leantoken.tech.3.key-managment.png)

6. Click `Create key`.
7. Copy the generated API key.
8. Also prepare the exact model identifier you plan to use in MiroFish through `LLM_MODEL_NAME`.
9. Store the key and endpoint as secrets. If the key is exposed, rotate it and replace it in `.env`.

## How to Add BYOK

The flow below matches the saved MiroFish materials best.

### 1. Prepare the project and `.env`

The saved README recommends this starting point:

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
```

Then open `.env` and fill the LLM block with values from LeanToken:

```env
LLM_API_KEY=your_leantoken_key
LLM_BASE_URL=https://api.leantoken.tech/v1
LLM_MODEL_NAME=your_model_identifier
ZEP_API_KEY=your_zep_key
```

What matters here:

- `LLM_API_KEY` and `LLM_BASE_URL` come from LeanToken
- `LLM_MODEL_NAME` must match a model actually served by your LeanToken endpoint
- `ZEP_API_KEY` must be obtained separately, because LeanToken does not replace the Zep Cloud memory layer

The saved configuration also includes an optional `Boost` block for accelerating some parallel operations. In the `.env` snippet you provided, it uses these variables:

```env
LLM_BOOST_API_KEY=your_api_key_here
LLM_BOOST_BASE_URL=your_base_url_here
LLM_BOOST_MODEL_NAME=your_model_name_here
```

If you want to use a separate acceleration configuration through LeanToken, you can fill this block with the same endpoint and model or dedicate a different model to Boost. If you do not need this acceleration setup, the provided snippet explicitly recommends leaving those variables out of `.env`.

### 2. Install dependencies

The saved materials recommend a single installation path:

```bash
npm run setup:all
```

If you prefer to install things in separate steps:

```bash
npm run setup
npm run setup:backend
```

According to the saved documentation, this installs root and frontend dependencies through `npm`, and backend dependencies through `uv`.

### 3. Start MiroFish

For local development, use:

```bash
npm run dev
```

After startup, the saved materials list the services here:

- frontend: `http://localhost:3000`
- backend API: `http://localhost:5001`

If you need to start parts separately:

```bash
npm run backend
npm run frontend
```

### 4. Go through the step-by-step workflow in the interface

After startup, open the frontend and go through the standard MiroFish steps:

1. At `Graph Construction`, upload seed materials such as news, analytical reports, policy drafts, or other source documents.
2. At `Environment Setup`, confirm that the environment and agent personas are generated without LLM authentication errors.
3. At `Simulation Execution`, start the simulation.
4. At `Report`, wait for the final analytical report.
5. At `Interaction`, ask follow-up questions to specific agents or to ReportAgent.

An important MiroFish detail: you do not paste the LeanToken key into a dedicated field inside the wizard. According to the saved materials, LLM configuration is read from `.env`, so the main sign of a correct BYOK setup is that the model-backed steps work successfully.

![Saved MiroFish demo-world screenshot from the project materials](../../images/mirofish/mirofish-demo-world.webp)

### 5. Start with a short and inexpensive simulation

The saved README explicitly notes that simulations can consume LLM credits quickly. It also gives a practical recommendation to start with runs under 40 rounds.

For an initial BYOK check, it is better to:

- use a small set of source materials
- run a short simulation
- confirm that graph building and persona generation complete without authentication errors
- only then increase scenario complexity and the number of rounds

## Alternative: Docker

The saved MiroFish materials also describe a container-based setup:

```bash
cp .env.example .env
docker compose up -d
```

According to DeepWiki and the README:

- the image used is `ghcr.io/666ghj/mirofish:latest`
- `docker compose` reads `.env` from the project root
- ports `3000` and `5001` are exposed
- `./backend/uploads` is mounted into the container so uploaded files and simulation logs persist

For BYOK, the Docker path is the same as the source deployment path: the key point is that `.env` already contains your LeanToken key, endpoint, and model.

## Practical Notes

- According to the saved article, MiroFish is an early `0.1.0` release, and the materials do not document confirmed backtesting.
- The article also states that the project had not yet been fully validated on Windows.
- The MiroFish documentation recommends an OpenAI-compatible LLM configuration and separately depends on Zep Cloud, so a LeanToken key alone does not cover the full service stack.
- Do not store `.env` in git or expose `LLM_API_KEY`, `ZEP_API_KEY`, and the endpoint in logs or public notes.
- If you want to understand the product before setting up a local install, the saved materials also include an [online demo](https://666ghj.github.io/mirofish-demo/).

## What to Check After Setup

- `.env` contains valid `LLM_API_KEY`, `LLM_BASE_URL`, `LLM_MODEL_NAME`, and `ZEP_API_KEY`
- MiroFish starts without errors through `npm run dev` or `docker compose up -d`
- the frontend is available on `localhost:3000`, and the backend on `localhost:5001`
- graph building and environment setup do not fail on LLM authentication
- the simulation starts and returns a report
- you can open post-simulation interaction and ask a question to an agent or to ReportAgent
- LeanToken and Zep keys did not end up in git, shell history, or shared configs
