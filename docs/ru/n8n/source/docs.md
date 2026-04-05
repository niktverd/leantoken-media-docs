# n8n BYOK research notes

Date reviewed: 2026-04-05

## Source folder status

- `README.md` existed and described the intended research contents.
- `links.md` contained two official n8n links:
  - docs root
  - AI Agent node
- Published screenshots for the final page are stored in `/Users/niktverd/code/leantoken-tech-media-docs/images/n8n`.

## Official sources used

- https://docs.n8n.io/
- https://docs.n8n.io/integrations/builtin/credentials/openai/
- https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-langchain.chattrigger/
- https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/
- https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatopenai/
- https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-langchain.openai/text-operations/
- https://docs.n8n.io/external-secrets/

## Notes extracted from official docs

- `OpenAI credentials` officially support API key authentication and mention optional `Organization ID`.
- The current saved n8n UI screenshot also shows a `Base URL` field in the OpenAI credential form, which is the key field for LeanToken BYOK.
- `Chat Trigger` is the official entry point for AI chat workflows.
- Every message sent to `Chat Trigger` executes the workflow once.
- `Chat Trigger` supports `Hosted Chat` and `Embedded Chat`, plus `None`, `Basic Auth`, and `n8n User Auth`.
- The current AI Agent documentation states that all AI Agent nodes now work as `Tools Agent` and that at least one tool sub-node must be connected.
- `OpenAI Chat Model` dynamically loads models from the connected account.
- The node supports both Chat Completions and the OpenAI `Responses API`.
- Built-in tools are documented for the `Responses API` path and only in combination with `AI Agent`.
- `OpenAI` node text operations currently expose both `Generate a Chat Completion` and `Generate a Model Response`.
- n8n stores credentials encrypted in its database by default.
- External secrets are documented as an Enterprise feature and support expressions like `{{ $secrets.<vault-name>.<secret-name> }}` inside credentials.

## Notes extracted from local screenshots

- `01-simple-n8n-setup.png` shows a minimal chat workflow:
  - `When chat message received`
  - `AI Agent`
  - `OpenAI Chat Model`
- `02-model-configuration.png` shows the OpenAI credential form with:
  - `API Key`
  - `Organization ID (optional)`
  - `Base URL`
  - LeanToken endpoint entered as `https://api.leantoken.tech/v1`
- `03-ai-agent-config.png` shows:
  - credential selected in `OpenAI Chat Model`
  - model selected from list
  - example model `qwen3-235b`
  - execution output and token usage visible in the n8n UI

## Editorial decisions for the published page

- The published page should focus on LeanToken as an OpenAI-compatible gateway for n8n.
- The page should document two practical setups:
  - `Chat Trigger` + `AI Agent` + `OpenAI Chat Model`
  - `OpenAI` node without agent logic
- The page should reuse the three canonical LeanToken key screenshots and all three provided n8n screenshots.
- The page should avoid promising that every OpenAI `Responses API` feature works through every compatible gateway; advise starting with the basic chat flow and enabling advanced features only after the base setup works.
