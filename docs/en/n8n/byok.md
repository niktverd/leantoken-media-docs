# n8n

## About the Service

n8n is a workflow automation platform for connecting services through APIs and assembling multi-step processes from nodes. In the official n8n documentation, it is positioned as a workflow automation tool with AI capabilities, where you can connect apps, transform data, launch chat interfaces, add models, memory, tools, and routing logic.

For AI scenarios, this is especially useful because n8n is not limited to a single chat window. The model becomes one step inside a real process:

- it receives an incoming event
- reads data from a CRM, email, spreadsheets, or a webhook
- generates an answer, summary, or structure
- passes the result to the next node
- saves logs, notifies the team, or calls an external API

In the `leantoken.tech` context, n8n is a strong fit for BYOK because the standard AI stack in n8n already works with an OpenAI-compatible credential. That lets you use LeanToken as a single endpoint at `https://api.leantoken.tech/v1` and reuse one credential across multiple workflows.

## What You Will Need

- access to `n8n Cloud` or a self-hosted `n8n` instance
- permission to create workflows and credentials
- an account on `https://leantoken.tech`
- a LeanToken API key
- the OpenAI-compatible base URL `https://api.leantoken.tech/v1`
- a model available in your LeanToken account
- a clear idea of which setup you want:
  - a chat or agent workflow through `Chat Trigger`, `AI Agent`, and `OpenAI Chat Model`
  - a regular automation through the `OpenAI` node without an agent

## AI Use Cases

n8n is especially useful when AI is not a standalone destination, but a step inside automation.

Practical use cases:

- classifying leads, support tickets, and incoming emails before routing them into a CRM or helpdesk
- summarizing conversations, meeting notes, and long emails, then sending the result to Slack, Telegram, or Notion
- extracting structured data from forms, documents, questionnaires, or webhook payloads
- building an AI reply flow through `Chat Trigger` and `AI Agent`
- enriching customer, product, or application records before writing them to a table or database
- routing requests by intent such as support, sales, onboarding, or finance
- building internal AI assistants that use tools and external APIs
- generating answer drafts, FAQ entries, summaries, metadata, and JSON payloads for downstream nodes

## Why BYOK Is Especially Useful In n8n

For n8n, BYOK through `leantoken.tech` is useful for a few practical reasons:

- the same key can be reused across multiple workflows
- you control the model, endpoint, and key lifecycle yourself
- one OpenAI-compatible credential can be reused in both `OpenAI Chat Model` and the `OpenAI` node
- it is easier to split dev, staging, and production into separate credentials
- if you run self-hosted `n8n`, AI calls can fit into your existing internal workflows without changing the overall architecture

Another advantage is that credentials in n8n are first-class platform objects. This is better than pasting a key manually into every node. According to the official n8n docs, credentials are stored encrypted in the database, and Enterprise plans can also pull them from external secrets vaults.

## How To Get An API Key On leantoken.tech

Before configuring n8n, get your key and endpoint in LeanToken.

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
8. Return to n8n and prepare the credential where you will paste:
   - `API key`
   - `Base URL`: `https://api.leantoken.tech/v1`
9. Do not expose the key in workflow exports, issue trackers, screenshots, or git. If it leaks, rotate it and update the credential.

## Minimal BYOK Architecture In n8n

The most visual starter setup in n8n looks like this: a user sends a message in chat, the workflow receives the event, `AI Agent` processes the request, and `OpenAI Chat Model` calls LeanToken through an OpenAI-compatible credential.

![Minimal AI workflow in n8n: Chat Trigger -> AI Agent -> OpenAI Chat Model](../../images/n8n/01-simple-n8n-setup.png)

The screenshot shows the basic chain:

1. `When chat message received` accepts the user message
2. `AI Agent` controls the answer logic
3. `OpenAI Chat Model` calls the model through the credential
4. the lower panel shows input, output, and token usage

This is a good smoke-test skeleton. After that, you can extend the workflow with memory, tools, retrievers, external APIs, tables, and other n8n nodes.

One detail from the official n8n documentation matters here:

- `Chat Trigger` is used for AI workflows and chat interfaces
- every message sent into `Chat Trigger` creates a separate workflow execution
- in the current documentation, `AI Agent` is described as a `Tools Agent`, and in some versions it expects at least one tool sub-node

If your `n8n` instance requires a mandatory tool sub-node, there are two practical paths:

1. add a compatible tool and keep using `AI Agent`
2. use the `OpenAI` node directly for a simple prompt-response workflow

## How To Create An OpenAI-Compatible Credential For LeanToken

In n8n, the cleanest way to start is with a dedicated `OpenAI` or `OpenAI account` credential and then reuse it across the nodes that need it.

![OpenAI account credential screen in n8n with API Key, Organization ID, and Base URL fields](../../images/n8n/02-model-configuration.png)

Typical flow:

1. Open `Credentials` in the n8n UI, or create a credential directly from `OpenAI Chat Model` or the `OpenAI` node.
2. Choose the credential type for `OpenAI`.
3. Paste your LeanToken key into `API Key`.
4. If the form includes `Organization ID`, you normally leave it blank for LeanToken. That field belongs to the standard OpenAI account flow and is not required for a typical LeanToken setup.
5. In `Base URL`, enter `https://api.leantoken.tech/v1`.
6. Give the credential a clear name such as `LeanToken OpenAI`.
7. Save the credential.

Practical notes:

- if the model list does not load later, the first thing to check is `Base URL`
- if you have multiple environments, create separate credentials such as `LeanToken Dev` and `LeanToken Prod`
- if you work in a team, verify who can access the credential and in which project it is shared

## How To Store The Key Safely In n8n

According to the official n8n documentation, credentials are stored encrypted in the database. For many teams, that is enough, but if you are on Enterprise Self-hosted or Enterprise Cloud, you can go further and connect external secrets.

This is useful when:

- the same secret is reused across multiple environments
- keys must live outside the n8n database
- you want centralized rotation and tighter access control

The n8n external secrets documentation describes providers such as `1Password`, `AWS Secrets Manager`, `Azure Key Vault`, `GCP Secrets Manager`, and `HashiCorp Vault`. After connecting a vault, secrets can be referenced in credentials with an expression such as `{{ $secrets.<vault-name>.<secret-name> }}`.

## Option 1. Setup Through AI Agent And OpenAI Chat Model

This path is the best fit when you are building a chatbot, AI assistant, or a workflow where the model is part of agent-style logic.

### Step 1. Add Chat Trigger

1. Create a new workflow.
2. Add `Chat Trigger`.
3. While you are still configuring the flow, keep public access disabled.
4. When you are ready to expose the chat, choose one of these modes:
   - `Hosted Chat` if you want to use the built-in n8n chat UI
   - `Embedded Chat` if you will use your own interface or widget
5. If the chat must be restricted, configure authentication:
   - `None`
   - `Basic Auth`
   - `n8n User Auth`

This matters operationally because every incoming chat message creates a separate execution.

### Step 2. Add AI Agent

1. Connect `AI Agent` to `Chat Trigger`.
2. Use it as the central node that receives the user request and decides how the answer should be assembled.
3. As the workflow grows, you can attach:
   - `Chat Model`
   - memory
   - tools
   - retrievers
   - sub-workflows

If you are building only the simplest answer flow without tools, account for version differences across n8n. In the current official docs, `AI Agent` is described as a `Tools Agent`, but your saved canvas screenshot already shows a minimal version of this setup, so the real behavior depends on your instance version.

### Step 3. Connect OpenAI Chat Model

![OpenAI Chat Model node in n8n with a selected credential and the qwen3-235b model](../../images/n8n/03-ai-agent-config.png)

1. Add `OpenAI Chat Model` as a sub-node under `AI Agent`.
2. In `Credential to connect with`, select the LeanToken credential you created.
3. In `Model`, choose one of the models available in your LeanToken account.
4. Your screenshot shows `qwen3-235b` as an example, but the actual list depends on which models your credential can see through the endpoint.

The official n8n docs describe two operating modes for this node:

- `Chat Completions`
- `Responses API`

In practice, that means:

- if you want the most compatible path, start with the basic chat completion flow
- if you are building a new workflow and your endpoint supports it, you can turn on `Use Responses API`
- when `Responses API` is enabled, OpenAI built-in tools become available
- in the n8n docs, built-in tools are described for the `OpenAI Chat Model` plus `AI Agent` combination

If compatibility errors appear after enabling `Use Responses API`, go back to the basic mode without that option. For an OpenAI-compatible gateway, this is a normal troubleshooting branch: some endpoints support only the core chat flow, not the full Responses feature set.

### Step 4. Configure Model Parameters

For most setups, it is best to start by filling only the credential and model fields, then add more options only when needed.

Useful settings:

- `Maximum Number of Tokens` if you want to cap output length
- `Sampling Temperature` if you need more predictable or more creative answers
- `Timeout` if the workflow should not wait too long
- `Max Retries` if you want to survive short network failures

Do not overload the first pass with too many options. For a smoke test, validate `credential + base URL + model` first.

### Step 5. Test The Response

1. Run the workflow manually.
2. Send a short message in chat such as `Hello` or `Summarize this text briefly`.
3. Check that a model response appears in the node output.
4. Open the execution details and confirm that:
   - the request goes through without an authorization error
   - the model actually answers
   - token usage is shown as expected

## Option 2. Setup Through The OpenAI Node Without An Agent

If you do not need an agent workflow and only want to call a model once inside automation, the regular `OpenAI` node is usually simpler.

This path is especially useful for:

- summarizing text from a previous node
- classifying incoming data
- generating JSON structures
- extracting fields from a document
- running an enrich, rewrite, or normalize step in the middle of a workflow

According to the current n8n docs, the `OpenAI` node for text currently offers at least two useful operations:

- `Generate a Chat Completion`
- `Generate a Model Response`

The practical difference is:

- `Generate a Chat Completion` uses the Chat Completions API
- `Generate a Model Response` uses the Responses API

Basic flow:

1. Add an `OpenAI` node.
2. Select the same LeanToken credential.
3. In `Resource`, choose `Text`.
4. In `Operation`, choose:
   - `Generate a Chat Completion` if you want the most direct prompt-response path
   - `Generate a Model Response` if you intentionally want the Responses API path
5. Choose the model.
6. Add the messages with the roles you need:
   - `System` if you want to define behavior
   - `User` for the actual request
7. If needed, enable:
   - `Simplify Output`
   - `Output Content as JSON`

This is often simpler than `AI Agent` when you do not need memory, tool calling, or a chat product surface.

## When To Choose AI Agent Versus The OpenAI Node

Use `AI Agent` when:

- you are building a chatbot or assistant
- the model needs to choose tools
- you want memory, tools, and more agent-like logic
- you already have a `Chat Trigger` driven conversation

Use the `OpenAI` node when:

- you need a single model call inside a larger workflow
- you are summarizing, classifying, rewriting, or extracting data
- the model output goes directly into the next standard node
- you do not want extra agent complexity yet

In practice, many n8n workflows start with the `OpenAI` node and only move to `AI Agent` later when tools and more advanced reasoning loops are actually needed.

## Recommended Working Patterns

### 1. Email triage

Flow:

1. `Gmail Trigger` or `IMAP Email Trigger`
2. `OpenAI` node to classify the email
3. `Switch` or `If`
4. route the result into CRM, Slack, Notion, or a helpdesk

### 2. Lead enrichment

Flow:

1. `Webhook` or `Form Trigger`
2. clean and normalize the input fields
3. `OpenAI` node for a short lead profile and intent classification
4. write the result to the CRM

### 3. Internal AI assistant

Flow:

1. `Chat Trigger`
2. `AI Agent`
3. `OpenAI Chat Model`
4. tools for reading tables, calling APIs, or launching sub-workflows

### 4. Structured extraction from text

Flow:

1. `Webhook` receives raw text or a document
2. `OpenAI` node returns JSON
3. `Edit Fields`, `Set`, `Code`, or `IF` validates the result
4. the data is sent to a database, spreadsheet, or downstream API

## What To Check After Setup

Minimal smoke test:

1. Confirm that the credential is saved with `Base URL` set to `https://api.leantoken.tech/v1`.
2. Check that the model list loads and shows at least one model available to your account.
3. Run the shortest possible request without extra parameters.
4. Confirm that the answer returns without `401`, `403`, or `404`.
5. Review execution output and token usage.
6. If you use `Chat Trigger`, test the chat in non-public mode first.
7. If you later expose the chat publicly, configure the right authentication mode.

## Common Problems

### The model does not appear in the list

This usually means one of three things:

- `Base URL` is wrong
- the API key is invalid or does not have access
- the target model is not available in your LeanToken account

### Authorization error

Check that:

- the LeanToken key is current
- there are no extra spaces in the key
- the credential really uses `https://api.leantoken.tech/v1`

### Errors after enabling Responses API

If the basic chat flow works but `Use Responses API` fails, the issue is usually a mismatch between the endpoint or model capabilities and the extra Responses features. In that case:

1. disable `Use Responses API`
2. verify the simple chat completion path
3. only then return to more advanced features

### AI Agent requires a tool sub-node

This follows from the current n8n documentation, which describes `AI Agent` as a `Tools Agent`. If that happens in your version:

1. add a compatible tool sub-node
2. or move the test to the regular `OpenAI` node

### Public chat behaves differently than expected

Check:

- whether `Make Chat Publicly Available` is enabled
- whether the mode is `Hosted Chat` or `Embedded Chat`
- whether `Basic Auth`, `n8n User Auth`, or `CORS` settings are interfering

### Executions get consumed too quickly

This matters for `Chat Trigger`: every user message creates a new workflow execution. If a user keeps a long conversation going, executions are consumed quickly, and you need to account for that in your operating model.

## Limitations And Important Notes

- Exact field names and button positions can vary between n8n versions.
- In the n8n docs, `OpenAI Chat Model` loads the model list dynamically from the account credential, so the final list depends on the LeanToken endpoint and your account access.
- `Responses API` and built-in tools should only be enabled after the basic path is already confirmed to work.
- If you use public chat, think through authentication, CORS, and execution cost separately.
- If the workflow handles sensitive data, decide in advance which fields can be sent to the model and which should be masked or excluded.

## Useful Links

- [n8n Docs](https://docs.n8n.io/)
- [OpenAI credentials in n8n](https://docs.n8n.io/integrations/builtin/credentials/openai/)
- [Chat Trigger node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-langchain.chattrigger/)
- [AI Agent node](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)
- [OpenAI Chat Model node](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatopenai/)
- [OpenAI Text operations](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-langchain.openai/text-operations/)
- [External secrets in n8n](https://docs.n8n.io/external-secrets/)
- [LeanToken](https://leantoken.tech)
