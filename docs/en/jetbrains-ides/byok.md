# JetBrains IDEs

## Service Description

JetBrains IDEs are developer tools from JetBrains, including [CLion](https://blog.jetbrains.com/clion/), [DataGrip](https://blog.jetbrains.com/datagrip/), [DataSpell](https://blog.jetbrains.com/dataspell/), [GoLand](https://blog.jetbrains.com/go/), [IntelliJ IDEA](https://blog.jetbrains.com/idea/), [PhpStorm](https://blog.jetbrains.com/phpstorm/), [PyCharm](https://blog.jetbrains.com/pycharm/), [RustRover](https://blog.jetbrains.com/rust/), [Rider](https://blog.jetbrains.com/dotnet/tag/rider/), [RubyMine](https://blog.jetbrains.com/ruby/), and [WebStorm](https://blog.jetbrains.com/webstorm/).

JetBrains BYOK support lets you connect your own AI provider and models directly inside the IDE. Based on the saved JetBrains AI blog post, BYOK is available in AI chat and in AI agents, including Junie and Claude Agent, as long as your provider offers the required models.

## AI Use Cases

- explain unfamiliar code, stack traces, and project structure without leaving the IDE
- draft refactorings, tests, scripts, and repetitive code scaffolding faster
- use AI chat or agent workflows with your preferred model for code review, implementation, and debugging tasks
- connect local or self-hosted model endpoints through OpenAI-compatible APIs when privacy or cost control matters
- generate technical documentation, changelog notes, and review summaries while working in the repository

## BYOK on leantoken.tech

Bring Your Own Key on `leantoken.tech` means you use your own provider credentials instead of relying only on a bundled shared setup. For JetBrains IDEs, this is useful when you want to control the model, the provider, the endpoint, the billing, and the privacy boundary yourself.

According to the JetBrains announcement, BYOK supports API keys from Anthropic, OpenAI, and other OpenAI-compatible providers. JetBrains also highlights that keys are stored locally on your machine and are not shared with JetBrains.

JetBrains also embedded a short walkthrough video in the source article: [Bring Your Own Key in JetBrains IDEs](https://www.youtube.com/watch?v=JcJyoFyLEOs).

![JetBrains AI chat using third-party BYOK models](../../images/jetbrains/jetbrains-ides-byok-chat.webp)

The chat model picker can expose the models available through your connected provider, so you can switch between models directly in the IDE.

## How To Get An API Key On leantoken.tech

Use this flow before configuring JetBrains IDEs:

![LeanToken landing page with Sign up, Log in, and Dashboard](../../images/leantoken/leantoken.tech.1.landing.png)

1. Open `https://leantoken.tech`.
2. Click `Sign up` if you do not have an account yet, `Log in` if you already have one, or `Dashboard` if you are already signed in.

![LeanToken dashboard overview with API endpoint and quick links](../../images/leantoken/leantoken.tech.2.dashboard.png)

3. After you sign in, open the dashboard or overview page.
4. Copy the API endpoint shown there. In the screenshots, the endpoint is `https://api.leantoken.tech/v1`.
5. Use `Manage keys` or open `API Keys` from the left sidebar.

![LeanToken API Keys page with Create key action](../../images/leantoken/leantoken.tech.3.key-managment.png)

6. On the `API Keys` page, click `Create key`.
7. Copy the generated API key you want to use in JetBrains IDEs.
8. Keep both the key and endpoint private. If the key is exposed, revoke or rotate it and replace it in your IDE settings.

## How To Add BYOK

The saved JetBrains blog post shows the setup path as `Settings | Tools | AI Assistant | Models`.

![Older JetBrains BYOK setup under the Models page](../../images/jetbrains/jetbrains-ides-byok-models-settings.webp)

In the newer UI from your screenshot, the configuration appears under `Settings | Tools | AI Assistant | Providers & API keys`.

![Current JetBrains Providers & API keys screen for BYOK setup](../../images/jetbrains/jetbrains-ides-byok-providers-api-keys.jpg)

Typical flow in the current UI:

1. Get your LeanToken API key and, if needed, the matching API URL by following the section above.
2. In JetBrains IDEs, open `Settings | Tools | AI Assistant | Providers & API keys`.
3. In `Third-party AI providers`, choose the provider type.
4. If LeanToken is exposed as an OpenAI-compatible endpoint for your setup, enter the base URL `https://api.leantoken.tech/v1` and your API key.
5. Click `Test Connection`.
6. Enable `Tool calling` if your provider and workflow support it.
7. In `Model Assignment`, choose which connected models should be used for core features and helpers.
8. Open AI chat and confirm that the expected provider models appear in the picker.

## JetBrains AI And BYOK Together

JetBrains explicitly notes that not every AI feature is guaranteed to work with third-party provider models alone. For cloud code completion and some editor-integrated features, JetBrains recommends activating JetBrains AI as well.

The practical setup is:

- use your own key for the provider and models you want to control directly
- keep JetBrains AI active when you want better coverage for built-in IDE features
- choose the provider-model pair in chat when you need a specific capability, price point, or privacy profile

## Recommended BYOK Scenarios

- Use Anthropic or OpenAI models when you want strong coding assistance in chat and agent workflows.
- Use an OpenAI-compatible gateway when your team standardizes access to multiple providers behind one endpoint.
- Use a local endpoint when you want private experimentation inside the IDE without sending prompts to a hosted public API.
- Use `leantoken.tech` as the operational place where your own provider credentials are managed and reused across your AI setup.

## Notes

- JetBrains says more providers are expected, including Google Gemini, Azure, and Amazon Bedrock.
- The exact menu labels can differ between IDE versions. The saved blog article uses `Models`, while the screenshot you provided shows `Providers & API keys`.
- Agent support still depends on whether your chosen provider exposes the models required by Junie or Claude Agent.
