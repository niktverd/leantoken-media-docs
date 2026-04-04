> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Trace DeepSeek applications

[DeepSeek](https://deepseek.com/) provides high-performance, OpenAI-compatible language models including `deepseek-chat` (for general conversations) and `deepseek-reasoner` (for advanced reasoning tasks). Using LangSmith allows you to debug, monitor, and evaluate your LLM applications by capturing structured traces of inputs, outputs, and metadata.

This guide shows you how to integrate DeepSeek with LangSmith in both Python and TypeScript, using LangSmith's [`@traceable`](https://reference.langchain.com/python/langsmith/run_helpers/traceable) (Python) and [`traceable(...)`](https://reference.langchain.com/javascript/modules/langsmith.html) (TypeScript) utilities to log LLM calls automatically.

## Installation

Install [OpenAI](https://platform.openai.com/docs/libraries) and LangSmith:

<CodeGroup>
  ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  pip install openai langsmith
  ```

  ```bash uv theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  uv add openai langsmith
  ```

  ```bash npm theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  npm install openai langsmith dotenv
  ```
</CodeGroup>

DeepSeek provides an [OpenAI-compatible API](https://api-docs.deepseek.com/), which means you can use the OpenAI SDK to interact with DeepSeek models. The only difference is that you configure the client to point to DeepSeek's base URL (`https://api.deepseek.com/v1`) instead of OpenAI's endpoint.

## Setup

Set your [API keys](/langsmith/create-account-api-key) and project name:

```bash  theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
export LANGSMITH_API_KEY="your-langsmith-api-key"
export LANGSMITH_TRACING="true"
export LANGSMITH_PROJECT="deepseek-integration"
export DEEPSEEK_API_KEY="your-deepseek-api-key"
```

* Ensure you have a DeepSeek API key from your [DeepSeek account](https://platform.deepseek.com/).
* Set `LANGSMITH_TRACING=true` and provide your LangSmith API key (`LANGSMITH_API_KEY`) activates automatic logging of traces.
* Specify a [`LANGSMITH_PROJECT`](/langsmith/log-traces-to-project) name to organize traces by project; if not set, traces go to the default project (named "default").
* The `LANGSMITH_TRACING` flag must be true for any traces to be recorded.

## Configure tracing

1. Instrument the DeepSeek API call with LangSmith. In your script, create an OpenAI client configured to use DeepSeek's API endpoint and wrap a call in a traced function:

   <CodeGroup>
     ```python Python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
     import os
     from openai import OpenAI
     from langsmith import traceable

     # Create a client pointing to DeepSeek
     client = OpenAI(
         api_key=os.environ["DEEPSEEK_API_KEY"],
         base_url="https://api.deepseek.com/v1"
     )

     @traceable(
         run_type="llm",
         name="DeepSeek Chat Completion",
         metadata={"ls_provider": "deepseek", "ls_model_name": "deepseek-chat"},
     )
     def call_deepseek(messages: list[dict]):
         response = client.chat.completions.create(
             model="deepseek-chat",
             messages=messages
         )
         return response.choices[0].message

     if __name__ == "__main__":
         messages = [
             {"role": "system", "content": "You are a helpful assistant that translates English to French."},
             {"role": "user", "content": "I love programming."}
         ]
         result = call_deepseek(messages=messages)
         print("Model reply:", result.content)
     ```

     ```typescript TypeScript theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
     import { config } from "dotenv";
     import OpenAI from "openai";
     import { traceable } from "langsmith/traceable";

     config(); // Load env vars from .env

     const openai = new OpenAI({
     apiKey: process.env.DEEPSEEK_API_KEY,
     baseURL: "https://api.deepseek.com/v1"
     });

     type ChatMessage = {
     role: "system" | "user" | "assistant";
     content: string;
     };

     const callDeepSeek = traceable(
     async (messages: ChatMessage[]) => {
         const response = await openai.chat.completions.create({
         model: "deepseek-chat",
         messages
         });

         return response.choices[0].message;
     },
     {
         name: "DeepSeek Chat Completion",
         run_type: "llm",
         metadata: {
         ls_provider: "deepseek",
         ls_model_name: "deepseek-chat"
         }
     }
     );

     (async () => {
     const messages: ChatMessage[] = [
         {
         role: "system",
         content: "You are a helpful assistant that translates English to French."
         },
         {
         role: "user",
         content: "I love programming."
         }
     ];

     const result = await callDeepSeek(messages);
     console.log("Model reply:", result.content);
     })();

     ```
   </CodeGroup>

   In this example, you use the OpenAI SDK to interact with [DeepSeek's API](https://api-docs.deepseek.com/). The OpenAI client is configured with `base_url="https://api.deepseek.com/v1"` to route requests to DeepSeek's endpoint while maintaining OpenAI-compatible syntax.

   The `@traceable` decorator (Python) or `traceable` function (TypeScript) wraps your function so that each invocation is logged as a trace run of type `"llm"`. The `metadata` parameter tags the trace with:

   * `ls_provider`: Identifies the provider (DeepSeek) for filtering traces.
   * `ls_model_name`: Specifies the model used for cost tracking and analytics.

   The function returns the full message object (`response.choices[0].message`), which includes the response content along with metadata like the role and any additional fields. LangSmith automatically captures:

   * Input messages sent to the model.
   * The model's complete response (content, role, etc.).
   * Model name and token usage statistics.
   * Execution timing and any errors.

2. Execute your script to generate a trace:

   <CodeGroup>
     ```bash Python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
     python deepseek_trace.py
     ```

     ```bash TypeScript theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
     node deepseek_trace.js
     ```
   </CodeGroup>

   The function call will reach out to DeepSeek's API, and because of the `@traceable`/`traceable` wrapper, LangSmith will log this call's inputs and outputs as a new trace. You'll find the model's response printed to the console, and a corresponding run appear in the [LangSmith UI](https://smith.langchain.com).

## View traces in LangSmith

After running the example, you can inspect the recorded traces in the [LangSmith UI](https://smith.langchain.com):

1. Open the LangSmith UI and log in to your account.
2. Select the project you used for this integration (for example, the name set in `LANGSMITH_PROJECT`, or "default" if you didn't set one).
3. Find the trace corresponding to your DeepSeek API call. It will be identified by the function name (`DeepSeek Chat Completion`).
4. Click on the trace to open it. You'll be able to inspect the model input and output, including the prompt messages you sent and the response from DeepSeek, as well as timing information (latency) and token usage.

With LangSmith's tracing, you have full visibility into your DeepSeek calls—allowing you to debug the behavior of DeepSeek's models, monitor performance (response time and token usage), and compare runs with different parameters.

## Cost tracking

Although DeepSeek models are open-weight, using the hosted DeepSeek API may incur usage-based costs depending on your plan.

LangSmith can automatically associate costs with traced LLM calls by estimating token usage and applying model-specific pricing. When tracing DeepSeek API calls, LangSmith uses the recorded prompt and response messages to calculate token counts and attach cost information to each run.

To enable automatic cost tracking for LLM calls, refer to [Automatically track costs based on token counts](/langsmith/cost-tracking#llm-calls:-automatically-track-costs-based-on-token-counts).

Once enabled, costs appear directly in the LangSmith UI alongside each traced DeepSeek run, allowing you to monitor usage and compare experiments over time.

***

<div className="source-links">
  <Callout icon="edit">
    [Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/trace-deepseek.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
  </Callout>

  <Callout icon="terminal-2">
    [Connect these docs](/use-these-docs) to Claude, VSCode, and more via MCP for real-time answers.
  </Callout>
</div>



> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Trace LangChain applications (Python and JS/TS)

LangSmith integrates seamlessly with LangChain (Python and JavaScript), the popular open-source framework for building LLM applications.

## Installation

Install the following for Python or JS (the code snippets use the OpenAI integration).

For a full list of packages available, see the [LangChain docs](/oss/python/integrations/providers/overview).

<CodeGroup>
  ```bash pip theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  pip install langchain_openai
  ```

  ```bash yarn theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  yarn add @langchain/openai @langchain/core
  ```

  ```bash npm theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  npm install @langchain/openai @langchain/core
  ```

  ```bash pnpm theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  pnpm add @langchain/openai @langchain/core
  ```
</CodeGroup>

## Quick start

### 1. Configure your environment

```bash wrap theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
export LANGSMITH_TRACING=true
export LANGSMITH_API_KEY=<your-api-key>
# This example uses OpenAI, but you can use any LLM provider of choice
export OPENAI_API_KEY=<your-openai-api-key>
# For LangSmith API keys linked to multiple workspaces, set the LANGSMITH_WORKSPACE_ID environment variable to specify which workspace to use.
export LANGSMITH_WORKSPACE_ID=<your-workspace-id>
```

<Info>
  If you are using LangChain.js with LangSmith and are not in a serverless environment, we also recommend setting the following explicitly to reduce latency:

  `export LANGCHAIN_CALLBACKS_BACKGROUND=true`

  If you are in a serverless environment, we recommend setting the reverse to allow tracing to finish before your function ends:

  `export LANGCHAIN_CALLBACKS_BACKGROUND=false`
</Info>

### 2. Log a trace

No extra code is needed to log a trace to LangSmith. Just run your LangChain code as you normally would.

<CodeGroup>
  ```python Python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  from langchain_openai import ChatOpenAI
  from langchain_core.prompts import ChatPromptTemplate
  from langchain_core.output_parsers import StrOutputParser

  prompt = ChatPromptTemplate.from_messages([
      ("system", "You are a helpful assistant. Please respond to the user's request only based on the given context."),
      ("user", "Question: {question}\nContext: {context}")
  ])

  model = ChatOpenAI(model="gpt-4.1-mini")
  output_parser = StrOutputParser()
  chain = prompt | model | output_parser

  question = "Can you summarize this morning's meetings?"
  context = "During this morning's meeting, we solved all world conflict."

  chain.invoke({"question": question, "context": context})
  ```

  ```typescript TypeScript theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  import { ChatOpenAI } from "@langchain/openai";
  import { ChatPromptTemplate } from "@langchain/core/prompts";
  import { StringOutputParser } from "@langchain/core/output_parsers";

  const prompt = ChatPromptTemplate.fromMessages([
    ["system", "You are a helpful assistant. Please respond to the user's request only based on the given context."],
    ["user", "Question: {question}\nContext: {context}"],
  ]);

  const model = new ChatOpenAI({ modelName: "gpt-4.1-mini" });
  const outputParser = new StringOutputParser();
  const chain = prompt.pipe(model).pipe(outputParser);

  const question = "Can you summarize this morning's meetings?"
  const context = "During this morning's meeting, we solved all world conflict."

  await chain.invoke({ question: question, context: context });
  ```
</CodeGroup>

### 3. View your trace

By default, the trace will be logged to the project with the name `default`. You can view an example of a trace logged using the above code [publicly in LangSmith](https://smith.langchain.com/public/e6a46eb2-d785-4804-a1e3-23f167a04300/r).

<img src="https://mintcdn.com/langchain-5e9cc07a/4kN8yiLrZX_amfFn/langsmith/images/langchain-trace.png?fit=max&auto=format&n=4kN8yiLrZX_amfFn&q=85&s=a116791e1efb842afdde7b478309b79f" alt="Langchain trace" width="1382" height="557" data-path="langsmith/images/langchain-trace.png" />

## Trace selectively

The [previous section](#quick-start) showed how to trace all invocations of a LangChain runnables within your applications by setting a single environment variable. While this is a convenient way to get started, you may want to trace only specific invocations or parts of your application.

There are two ways to do this in Python: by manually passing in a `LangChainTracer` instance as a [callback](https://reference.langchain.com/python/langchain_core/callbacks/), or by using the [`tracing_context` context manager](https://reference.langchain.com/python/langsmith/observability/sdk/run_helpers/#langsmith.run_helpers.tracing_context).

In JS/TS, you can pass a [`LangChainTracer`](https://reference.langchain.com/javascript/classes/_langchain_core.tracers_tracer_langchain.LangChainTracer.html) instance as a callback.

<CodeGroup>
  ```python Python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  # You can opt-in to specific invocations..
  import langsmith as ls

  with ls.tracing_context(enabled=True):
      chain.invoke({"question": "Am I using a callback?", "context": "I'm using a callback"})

  # This will NOT be traced (assuming LANGSMITH_TRACING is not set)
  chain.invoke({"question": "Am I being traced?", "context": "I'm not being traced"})

  # This would not be traced, even if LANGSMITH_TRACING=true
  with ls.tracing_context(enabled=False):
      chain.invoke({"question": "Am I being traced?", "context": "I'm not being traced"})
  ```

  ```typescript TypeScript theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  // You can configure a LangChainTracer instance to trace a specific invocation.
  import { LangChainTracer } from "@langchain/core/tracers/tracer_langchain";

  const tracer = new LangChainTracer();
  await chain.invoke(
    {
      question: "Am I using a callback?",
      context: "I'm using a callback"
    },
    { callbacks: [tracer] }
  );
  ```
</CodeGroup>

## Log to a specific project

### Statically

As mentioned in the [tracing conceptual guide](/langsmith/observability-concepts) LangSmith uses the concept of a Project to group traces. If left unspecified, the tracer project is set to default. You can set the `LANGSMITH_PROJECT` environment variable to configure a custom project name for an entire application run. This should be done before executing your application.

```bash  theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
export LANGSMITH_PROJECT=my-project
```

<Warning>
  The `LANGSMITH_PROJECT` flag is only supported in JS SDK versions >= 0.2.16, use `LANGCHAIN_PROJECT` instead if you are using an older version.
</Warning>

### Dynamically

This largely builds off of the [previous section](#trace-selectively) and allows you to set the project name for a specific `LangChainTracer` instance or as parameters to the `tracing_context` context manager in Python.

<CodeGroup>
  ```python Python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  # You can set the project name using the project_name parameter.
  import langsmith as ls

  with ls.tracing_context(project_name="My Project", enabled=True):
      chain.invoke({"question": "Am I using a context manager?", "context": "I'm using a context manager"})
  ```

  ```typescript TypeScript theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  // You can set the project name for a specific tracer instance:
  import { LangChainTracer } from "@langchain/core/tracers/tracer_langchain";

  const tracer = new LangChainTracer({ projectName: "My Project" });
  await chain.invoke(
    {
      question: "Am I using a callback?",
      context: "I'm using a callback"
    },
    { callbacks: [tracer] }
  );
  ```
</CodeGroup>

## Add metadata and tags to traces

You can annotate your traces with arbitrary metadata and tags by providing them in the [`RunnableConfig`](https://reference.langchain.com/python/langchain_core/runnables/?h=runnablecon#langchain_core.runnables.RunnableConfig). This is useful for associating additional information with a trace, such as the environment in which it was executed, or the user who initiated it. For information on how to query traces and runs by metadata and tags, see [Query traces (SDK)](/langsmith/export-traces)

<Note>
  When you attach metadata or tags to a runnable (either through the [`RunnableConfig`](https://reference.langchain.com/python/langchain-core/runnables/config/RunnableConfig) or at runtime with invocation params), they are inherited by all child runnables of that runnable.
</Note>

<CodeGroup>
  ```python Python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  from langchain_openai import ChatOpenAI
  from langchain_core.prompts import ChatPromptTemplate
  from langchain_core.output_parsers import StrOutputParser

  prompt = ChatPromptTemplate.from_messages([
      ("system", "You are a helpful AI."),
      ("user", "{input}")
  ])

  # The tag "model-tag" and metadata {"model-key": "model-value"} will be attached to the ChatOpenAI run only
  chat_model = ChatOpenAI().with_config({"tags": ["model-tag"], "metadata": {"model-key": "model-value"}})
  output_parser = StrOutputParser()

  # Tags and metadata can be configured with RunnableConfig
  chain = (prompt | chat_model | output_parser).with_config({"tags": ["config-tag"], "metadata": {"config-key": "config-value"}})

  # Tags and metadata can also be passed at runtime
  chain.invoke({"input": "What is the meaning of life?"}, {"tags": ["invoke-tag"], "metadata": {"invoke-key": "invoke-value"}})
  ```

  ```typescript TypeScript theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  import { ChatOpenAI } from "@langchain/openai";
  import { ChatPromptTemplate } from "@langchain/core/prompts";
  import { StringOutputParser } from "@langchain/core/output_parsers";

  const prompt = ChatPromptTemplate.fromMessages([
      ["system", "You are a helpful AI."],
      ["user", "{input}"]
  ])

  // The tag "model-tag" and metadata {"model-key": "model-value"} will be attached to the ChatOpenAI run only
  const model = new ChatOpenAI().withConfig({ tags: ["model-tag"], metadata: { "model-key": "model-value" } });
  const outputParser = new StringOutputParser();

  // Tags and metadata can be configured with RunnableConfig
  const chain = (prompt.pipe(model).pipe(outputParser)).withConfig({"tags": ["config-tag"], "metadata": {"config-key": "top-level-value"}});

  // Tags and metadata can also be passed at runtime
  await chain.invoke({input: "What is the meaning of life?"}, {tags: ["invoke-tag"], metadata: {"invoke-key": "invoke-value"}})
  ```
</CodeGroup>

## Customize run name

You can customize the name of a given run when invoking or streaming your LangChain code by providing it in the [Config](https://reference.langchain.com/python/langchain_core/runnables/?h=runnablecon#langchain_core.runnables.RunnableConfig). This name is used to identify the run in LangSmith and can be used to filter and group runs. The name is also used as the title of the run in the LangSmith UI. This can be done by setting a `run_name` in the [`RunnableConfig`](https://reference.langchain.com/python/langchain-core/runnables/config/RunnableConfig) object at construction or by passing a `run_name` in the invocation parameters in JS/TS.

<CodeGroup>
  ```python Python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  # When tracing within LangChain, run names default to the class name of the traced object (e.g., 'ChatOpenAI').
  configured_chain = chain.with_config({"run_name": "MyCustomChain"})
  configured_chain.invoke({"input": "What is the meaning of life?"})

  # You can also configure the run name at invocation time, like below
  chain.invoke({"input": "What is the meaning of life?"}, {"run_name": "MyCustomChain"})
  ```

  ```typescript TypeScript theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  // When tracing within LangChain, run names default to the class name of the traced object (e.g., 'ChatOpenAI').
  const configuredChain = chain.withConfig({ runName: "MyCustomChain" });
  await configuredChain.invoke({ input: "What is the meaning of life?" });

  // You can also configure the run name at invocation time, like below
  await chain.invoke({ input: "What is the meaning of life?" }, {runName: "MyCustomChain"})
  ```
</CodeGroup>

<Note>
  The `run_name` parameter only changes the name of the runnable you invoke (e.g., a chain, function). It does not rename the nested run automatically created when you invoke an LLM object like [`ChatOpenAI`](https://reference.langchain.com/python/langchain-openai/chat_models/base/ChatOpenAI) (`gpt-4.1-mini`). In the example, the enclosing run will appear in LangSmith as `MyCustomChain`, while the nested LLM run still shows the model’s default name.

  To give the LLM run a more meaningful name, you can either:

  * Wrap the model in another runnable and assign a `run_name` to that step.
  * Use a tracing decorator or helper (e.g., `@traceable` in Python, or `traceable` from `langsmith` in JS/TS) to create a custom run around the model call.
</Note>

## Override model name in traces

When tracing LangChain model calls, LangSmith automatically captures the model identifier used in the API call. However, you may want to display a different, more descriptive name in traces for organizational purposes or to distinguish between different model configurations. You can do this by passing the `ls_model_name` [metadata parameter](/langsmith/ls-metadata-parameters#ls-model-name) when constructing or configuring your LangChain model.

This is particularly useful when:

* Working with self-hosted or local models where the model ID might not be descriptive.
* Using the same model with different configurations and wanting to distinguish them in traces.
* Creating aliases for models to make traces more readable for your team.
* Standardizing model names across different deployment environments.

<CodeGroup>
  ```python Python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  from langchain_openai import ChatOpenAI
  from langchain_ollama import ChatOllama

  # Override model name for a local model
  llm = ChatOllama(
      model="llama2:13b-chat",  # Actual model ID
      metadata={"ls_model_name": "llama2-13b-production"}  # Name shown in LangSmith
  )

  # Or with OpenAI to distinguish configurations
  llm_creative = ChatOpenAI(
      model="gpt-4.1",
      temperature=0.9,
      metadata={"ls_model_name": "gpt-4.1-creative"}
  )

  llm_factual = ChatOpenAI(
      model="gpt-4.1",
      temperature=0.1,
      metadata={"ls_model_name": "gpt-4.1-factual"}
  )

  # The metadata is inherited when the model is used in a chain
  result = llm.invoke("What is the meaning of life?")
  ```

  ```typescript TypeScript theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  import { ChatOpenAI } from "@langchain/openai";
  import { ChatOllama } from "@langchain/ollama";

  // Override model name for a local model
  const llm = new ChatOllama({
    model: "llama2:13b-chat",  // Actual model ID
    metadata: { ls_model_name: "llama2-13b-production" }  // Name shown in LangSmith
  });

  // Or with OpenAI to distinguish configurations
  const llmCreative = new ChatOpenAI({
    modelName: "gpt-4.1",
    temperature: 0.9,
    metadata: { ls_model_name: "gpt-4.1-creative" }
  });

  const llmFactual = new ChatOpenAI({
    modelName: "gpt-4.1",
    temperature: 0.1,
    metadata: { ls_model_name: "gpt-4.1-factual" }
  });

  // The metadata is inherited when the model is used in a chain
  const result = await llm.invoke("What is the meaning of life?");
  ```
</CodeGroup>

When you pass `ls_model_name` in the model's metadata, this name will appear in the LangSmith UI for all traces involving that model instance. This works for any LangChain chat model or LLM and is inherited by all runs that use the model, including when it's part of a chain.

<Note>
  The `ls_model_name` metadata parameter is also used for [cost tracking](/langsmith/cost-tracking). When combined with the `ls_provider` parameter, LangSmith can automatically calculate costs for custom or self-hosted models. For more information about all available metadata parameters, see the [metadata parameters reference](/langsmith/ls-metadata-parameters).
</Note>

## Customize run ID

You can customize the ID of a given run when invoking or streaming your LangChain code by providing it in the [Config](https://reference.langchain.com/python/langchain_core/runnables/?h=runnablecon#langchain_core.runnables.RunnableConfig). This ID is used to uniquely identify the run in LangSmith and can be used to query specific runs. The ID can be useful for linking runs across different systems or for implementing custom tracking logic. This can be done by setting a `run_id` in the [`RunnableConfig`](https://reference.langchain.com/python/langchain-core/runnables/config/RunnableConfig) object at construction or by passing a `run_id` in the invocation parameters.

<Note>
  This feature is not currently supported directly for LLM objects.
</Note>

<CodeGroup>
  ```python Python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  import uuid

  my_uuid = uuid.uuid4()

  # You can configure the run ID at invocation time:
  chain.invoke({"input": "What is the meaning of life?"}, {"run_id": my_uuid})
  ```

  ```typescript TypeScript theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  import { v4 as uuidv4 } from 'uuid';

  const myUuid = uuidv4();

  // You can configure the run ID at invocation time, like below
  await chain.invoke({ input: "What is the meaning of life?" }, { runId: myUuid });
  ```
</CodeGroup>

Note that if you do this at the **root** of a trace (i.e., the top-level run, that run ID will be used as the `trace_id`).

## Access run (span) ID for LangChain invocations

When you invoke a LangChain object, you can manually specify the run ID of the invocation. This run ID can be used to query the run in LangSmith.

In JS/TS, you can use a `RunCollectorCallbackHandler` instance to access the run ID.

<CodeGroup>
  ```python Python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  import uuid

  from langchain_openai import ChatOpenAI
  from langchain_core.prompts import ChatPromptTemplate
  from langchain_core.output_parsers import StrOutputParser

  prompt = ChatPromptTemplate.from_messages([
      ("system", "You are a helpful assistant. Please respond to the user's request only based on the given context."),
      ("user", "Question: {question}\n\nContext: {context}")
  ])
  model = ChatOpenAI(model="gpt-4.1-mini")
  output_parser = StrOutputParser()

  chain = prompt | model | output_parser

  question = "Can you summarize this morning's meetings?"
  context = "During this morning's meeting, we solved all world conflict."
  my_uuid = uuid.uuid4()
  result = chain.invoke({"question": question, "context": context}, {"run_id": my_uuid})
  print(my_uuid)
  ```

  ```typescript TypeScript theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  import { ChatOpenAI } from "@langchain/openai";
  import { ChatPromptTemplate } from "@langchain/core/prompts";
  import { StringOutputParser } from "@langchain/core/output_parsers";
  import { RunCollectorCallbackHandler } from "@langchain/core/tracers/run_collector";

  const prompt = ChatPromptTemplate.fromMessages([
    ["system", "You are a helpful assistant. Please respond to the user's request only based on the given context."],
    ["user", "Question: {question}\n\nContext: {context}"],
  ]);
  const model = new ChatOpenAI({ modelName: "gpt-4.1-mini" });
  const outputParser = new StringOutputParser();

  const chain = prompt.pipe(model).pipe(outputParser);
  const runCollector = new RunCollectorCallbackHandler();

  const question = "Can you summarize this morning's meetings?"
  const context = "During this morning's meeting, we solved all world conflict."
  await chain.invoke(
      { question: question, context: context },
      { callbacks: [runCollector] }
  );
  const runId = runCollector.tracedRuns[0].id;
  console.log(runId);
  ```
</CodeGroup>

## Ensure all traces are submitted before exiting

In LangChain Python, LangSmith's tracing is done in a background thread to avoid obstructing your production application. This means that your process may end before all traces are successfully posted to LangSmith. This is especially prevalent in a serverless environment, where your VM may be terminated immediately once your chain or agent completes.

You can make callbacks synchronous by setting the `LANGCHAIN_CALLBACKS_BACKGROUND` environment variable to `"false"`.

For both languages, LangChain exposes methods to wait for traces to be submitted before exiting your application. Below is an example:

<CodeGroup>
  ```python Python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  from langchain_openai import ChatOpenAI
  from langchain_core.tracers.langchain import wait_for_all_tracers

  llm = ChatOpenAI()

  try:
    llm.invoke("Hello, World!")
  finally:
    wait_for_all_tracers()
  ```

  ```typescript TypeScript theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  import { awaitAllCallbacks } from "@langchain/core/callbacks/promises";

  try {
      const llm = new ChatOpenAI();
      const response = await llm.invoke("Hello, World!");
  } catch (e) {
      // handle error
  } finally {
      await awaitAllCallbacks();
  }
  ```
</CodeGroup>

## Trace without setting environment variables

As mentioned in other guides, the following environment variables allow you to configure tracing enabled, the api endpoint, the api key, and the tracing project:

* `LANGSMITH_TRACING`
* `LANGSMITH_API_KEY`
* `LANGSMITH_ENDPOINT`
* `LANGSMITH_PROJECT`

However, in some environments, it is not possible to set environment variables. In these cases, you can set the tracing configuration programmatically.

This largely builds off of the [previous section](#trace-selectively).

<CodeGroup>
  ```python Python theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  import langsmith as ls

  # You can create a client instance with an api key and api url
  client = ls.Client(
      api_key="YOUR_API_KEY",  # This can be retrieved from a secrets manager
      api_url="https://api.smith.langchain.com",  # Update appropriately for self-hosted installations or the EU region
  )

  # You can pass the client and project_name to the tracing_context
  with ls.tracing_context(client=client, project_name="test-no-env", enabled=True):
      chain.invoke({"question": "Am I using a callback?", "context": "I'm using a callback"})
  ```

  ```typescript TypeScript theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  import { LangChainTracer } from "@langchain/core/tracers/tracer_langchain";
  import { Client } from "langsmith";

  // You can create a client instance with an api key and api url
  const client = new Client(
      {
          apiKey: "YOUR_API_KEY",
          apiUrl: "https://api.smith.langchain.com", // Update appropriately for self-hosted installations or the EU region
      }
  );

  // You can pass the client and project_name to the LangChainTracer instance
  const tracer = new LangChainTracer({client, projectName: "test-no-env"});
  await chain.invoke(
    {
      question: "Am I using a callback?",
      context: "I'm using a callback",
    },
    { callbacks: [tracer] }
  );
  ```
</CodeGroup>

## Distributed tracing with LangChain (Python)

LangSmith supports distributed tracing with LangChain Python. This allows you to link runs (spans) across different services and applications. The principles are similar to the [distributed tracing guide](/langsmith/distributed-tracing) for the LangSmith SDK.

```python  theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
import langsmith
from langchain_core.runnables import chain
from langsmith.run_helpers import get_current_run_tree

# -- This code should be in a separate file or service --
@chain
def child_chain(inputs):
    return inputs["test"] + 1

def child_wrapper(x, headers):
    with langsmith.tracing_context(parent=headers):
        child_chain.invoke({"test": x})

# -- This code should be in a separate file or service --
@chain
def parent_chain(inputs):
    rt = get_current_run_tree()
    headers = rt.to_headers()
    # ... make a request to another service with the headers
    # The headers should be passed to the other service, eventually to the child_wrapper function

parent_chain.invoke({"test": 1})
```

## Interoperability between LangChain (Python) and LangSmith SDK

If you are using LangChain for part of your application and the LangSmith SDK (see [Custom instrumentation](/langsmith/annotate-code)) for other parts, you can still trace the entire application seamlessly.

LangChain objects will be traced when invoked within a `traceable` function and be bound as a child run of the `traceable` function.

```python  theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langsmith import traceable

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant. Please respond to the user's request only based on the given context."),
    ("user", "Question: {question}\nContext: {context}")
])

model = ChatOpenAI(model="gpt-4.1-mini")
output_parser = StrOutputParser()
chain = prompt | model | output_parser

# The above chain will be traced as a child run of the traceable function
@traceable(
    tags=["openai", "chat"],
    metadata={"foo": "bar"}
)
def invoke_runnnable(question, context):
    result = chain.invoke({"question": question, "context": context})
    return "The response is: " + result

invoke_runnnable("Can you summarize this morning's meetings?", "During this morning's meeting, we solved all world conflict.")
```

This will produce the following trace tree: <img src="https://mintcdn.com/langchain-5e9cc07a/ImHGLQW1HnQYwnJV/langsmith/images/trace-tree-python-interop.png?fit=max&auto=format&n=ImHGLQW1HnQYwnJV&q=85&s=52c64fd784522c4b2d75886ae76f8c18" alt="Trace tree python interop" width="1334" height="734" data-path="langsmith/images/trace-tree-python-interop.png" />

## Interoperability between LangChain.JS and LangSmith SDK

### Tracing LangChain objects inside `traceable` (JS only)

Starting with `langchain@0.2.x`, LangChain objects are traced automatically when used inside `@traceable` functions, inheriting the client, tags, metadata and project name of the traceable function.

For older versions of LangChain below `0.2.x`, you will need to manually pass an instance `LangChainTracer` created from the tracing context found in `@traceable`.

```typescript  theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
import { ChatOpenAI } from "@langchain/openai";
import { ChatPromptTemplate } from "@langchain/core/prompts";
import { StringOutputParser } from "@langchain/core/output_parsers";
import { getLangchainCallbacks } from "langsmith/langchain";

const prompt = ChatPromptTemplate.fromMessages([
  [
    "system",
    "You are a helpful assistant. Please respond to the user's request only based on the given context.",
  ],
  ["user", "Question: {question}\nContext: {context}"],
]);

const model = new ChatOpenAI({ modelName: "gpt-4.1-mini" });
const outputParser = new StringOutputParser();
const chain = prompt.pipe(model).pipe(outputParser);

const main = traceable(
  async (input: { question: string; context: string }) => {
    const callbacks = await getLangchainCallbacks();
    const response = await chain.invoke(input, { callbacks });
    return response;
  },
  { name: "main" }
);
```

### Tracing LangChain child runs via `traceable` / RunTree API (JS only)

<Note>
  We're working on improving the interoperability between `traceable` and LangChain. The following limitations are present when using combining LangChain with `traceable`:

  1. Mutating RunTree obtained from `getCurrentRunTree()` of the RunnableLambda context will result in a no-op.
  2. It's discouraged to traverse the RunTree obtained from RunnableLambda via `getCurrentRunTree()` as it may not contain all the RunTree nodes.
  3. Different child runs may have the same `execution_order` and `child_execution_order` value. Thus in extreme circumstances, some runs may end up in a different order, depending on the `start_time`.
</Note>

In some uses cases, you might want to run `traceable` functions as part of the RunnableSequence or trace child runs of LangChain run imperatively via the `RunTree` API. Starting with LangSmith 0.1.39 and @langchain/core 0.2.18, you can directly invoke `traceable`-wrapped functions within RunnableLambda.

```typescript  theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
import { traceable } from "langsmith/traceable";
import { RunnableLambda } from "@langchain/core/runnables";
import { RunnableConfig } from "@langchain/core/runnables";

const tracedChild = traceable((input: string) => `Child Run: ${input}`, {
  name: "Child Run",
});

const parrot = new RunnableLambda({
  func: async (input: { text: string }, config?: RunnableConfig) => {
    return await tracedChild(input.text);
  },
});
```

<img src="https://mintcdn.com/langchain-5e9cc07a/ImHGLQW1HnQYwnJV/langsmith/images/trace-tree-manual-tracing.png?fit=max&auto=format&n=ImHGLQW1HnQYwnJV&q=85&s=7b117d3aa9b419fe2a314ec6d9cc7c16" alt="Trace Tree" width="2564" height="1530" data-path="langsmith/images/trace-tree-manual-tracing.png" />

Alternatively, you can convert LangChain's [`RunnableConfig`](https://reference.langchain.com/python/langchain-core/runnables/config/RunnableConfig) to a equivalent RunTree object by using `RunTree.fromRunnableConfig` or pass the [`RunnableConfig`](https://reference.langchain.com/python/langchain-core/runnables/config/RunnableConfig) as the first argument of `traceable`-wrapped function.

<CodeGroup>
  ```typescript Traceable theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  import { traceable } from "langsmith/traceable";
  import { RunnableLambda } from "@langchain/core/runnables";
  import { RunnableConfig } from "@langchain/core/runnables";

  const tracedChild = traceable((input: string) => `Child Run: ${input}`, {
    name: "Child Run",
  });

  const parrot = new RunnableLambda({
    func: async (input: { text: string }, config?: RunnableConfig) => {
      // Pass the config to existing traceable function
      await tracedChild(config, input.text);
      return input.text;
    },
  });
  ```

  ```typescript Run Tree theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  import { RunTree } from "langsmith/run_trees";
  import { RunnableLambda } from "@langchain/core/runnables";
  import { RunnableConfig } from "@langchain/core/runnables";

  const parrot = new RunnableLambda({
    func: async (input: { text: string }, config?: RunnableConfig) => {
      // create the RunTree from the RunnableConfig of the RunnableLambda
      const childRunTree = RunTree.fromRunnableConfig(config, {
        name: "Child Run",
      });

      childRunTree.inputs = { input: input.text };
      await childRunTree.postRun();

      childRunTree.outputs = { output: `Child Run: ${input.text}` };
      await childRunTree.patchRun();

      return input.text;
    },
  });
  ```
</CodeGroup>

If you prefer a video tutorial, check out the [Alternative Ways to Trace video](https://academy.langchain.com/pages/intro-to-langsmith-preview) from the Introduction to LangSmith Course.

***

<div className="source-links">
  <Callout icon="edit">
    [Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/langsmith/trace-with-langchain.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
  </Callout>

  <Callout icon="terminal-2">
    [Connect these docs](/use-these-docs) to Claude, VSCode, and more via MCP for real-time answers.
  </Callout>
</div>



