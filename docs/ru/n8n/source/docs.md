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
---
title: OpenAI Chat Model node documentation
description: Learn how to use the OpenAI Chat Model node in n8n. Follow technical documentation to integrate OpenAI Chat Model node into your workflows.
contentType: [integration, reference]
priority: high
---

# OpenAI Chat Model node

Use the OpenAI Chat Model node to use OpenAI's chat models with conversational [agents](/glossary.md#ai-agent).

On this page, you'll find the node parameters for the OpenAI Chat Model node and links to more resources.

/// note | Credentials
You can find authentication information for this node [here](/integrations/builtin/credentials/openai.md).
///

--8<-- "_snippets/integrations/builtin/cluster-nodes/sub-node-expression-resolution.md"

## Node parameters

### Model

Select the model to use to generate the completion.

n8n dynamically loads models from OpenAI, and you'll only see the models available to your account.

### Use Responses API
OpenAI provides two endpoints for generating output from a model:
- **Chat Completions**: The Chat Completions API endpoint generates a model response from a list of messages that comprise a conversation. The API requires the user to handle conversation state manually, for example by adding a [Simple Memory](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.memorybufferwindow/index.md) subnode. For new projects, OpenAI recommends to use the Responses API.
- **Responses**: The Responses API is an agentic loop, allowing the model to call multiple built-in tools within the span of one API request. It also supports persistent conversations by passing a `conversation_id`.

Toggle to **Use Responses API** if you want the model to generate output using the Responses API. Otherwise, the OpenAI Chat Model node will default to using the Chat Completions API.

Refer to the OpenAI documentation for a [comparison of the Chat Completions and Responses APIs](https://platform.openai.com/docs/guides/migrate-to-responses).


### Built-in Tools
The OpenAI Responses API provides a range of [built-in tools](https://platform.openai.com/docs/guides/tools) to enrich the model's response. Toggle to **Use Responses API** if you want the model to have access to the following built-in tools:

- **Web Search**: Allows models to search the web for the latest information before generating a response.
- **File Search**: Allow models to search your knowledgebase from previously uploaded files for relevant information before generating a response. Refer to the [OpenAI documentation](https://platform.openai.com/docs/guides/tools-file-search) for more information.
- **Code Interpreter**: Allows models to write and run Python code in a sandboxed environment.

/// note | Use with AI Agent node
Built-in tools are only supported when using the OpenAI Chat Model node in combination with the AI Agent node. Built-in tools aren't available when using the OpenAI Chat Model node in combination with a Basic LLM Chain node, for example.
///

## Node options

Use these options to further refine the node's behavior. The following options are available whether you use the Responses API to generate model output or not.

### Frequency Penalty

Use this option to control the chances of the model repeating itself. Higher values reduce the chance of the model repeating itself.

### Maximum Number of Tokens

Enter the maximum number of tokens used, which sets the completion length.

### Presence Penalty

Use this option to control the chances of the model talking about new topics. Higher values increase the chance of the model talking about new topics.

### Sampling Temperature

Use this option to control the randomness of the sampling process. A higher temperature creates more diverse sampling, but increases the risk of hallucinations.

### Timeout

Enter the maximum request time in milliseconds.

### Max Retries

Enter the maximum number of times to retry a request.

### Top P

Use this option to set the probability the completion should use. Use a lower value to ignore less probable options. 

## Additional node options (Responses API only)
The following, additional options are available when toggling to **Use Responses API**.

### Conversation ID
The conversation that this response belongs to. Input items and output items from this response are automatically added to this conversation after this response completes.

###  Prompt Cache Key
Use this key for caching similar requests to optimize cache hit rates.

### Safety Identifier
Apply an identifier to track users who may violate usage policies.

### Service Tier
Select the service tier that fits your needs: Auto, Flex, Default, or Priority.

### Metadata
A set of key-value pairs for storing structured information. You can attach up to 16 pairs to an object, which is useful for adding custom data that can be used for searching by the API or in the dashboard.

### Top Logprobs
Define an integer between 0 and 20 specifying the number of most likely tokens to return at each token position, each with an associated log probability.

### Output Format
Choose a response format: Text, JSON Schema, or JSON Object. Use of JSON Schema is recommended, if you want to receive data in JSON format.

### Prompt
Configure the prompt filled with a unique ID, its version, and substitutable variables. Prompts are configured through the OpenAI dashboard.

## Templates and examples

<!-- see https://www.notion.so/n8n/Pull-in-templates-for-the-integrations-pages-37c716837b804d30a33b47475f6e3780 -->
[[ templatesWidget(page.title, 'openai-chat-model') ]]

## Related resources

Refer to [LangChains's OpenAI documentation](https://js.langchain.com/docs/integrations/chat/openai/) for more information about the service.

Refer to [OpenAI documentation](https://platform.openai.com/docs/api-reference/responses/create) for more information about the parameters.

--8<-- "_snippets/integrations/builtin/cluster-nodes/langchain-overview-link.md"

## Common issues

For common questions or issues and suggested solutions, refer to [Common issues](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatopenai/common-issues.md).



