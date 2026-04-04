# LangChain

## О сервисе

LangChain — это open source framework для разработки AI-приложений и агентов. В официальной документации LangChain описан как framework с готовой архитектурой агентов и интеграциями для моделей, tools и внешних систем, чтобы приложение можно было быстро собрать и потом менять провайдеров без полной переписи кода.

LangChain особенно удобен там, где одной модели уже мало: нужно подключать tools, делать retrieval, вызывать внешние API, собирать structured output, строить agent workflow и наблюдать за вызовами через LangSmith. При этом сами агенты LangChain строятся поверх LangGraph, поэтому базовая интеграция может оставаться простой, а архитектура может постепенно усложняться.

Для BYOK-сценария это хороший вариант, потому что LangChain уже умеет работать с OpenAI-совместимым API. Если ваш доступ к модели идет через `https://api.leantoken.tech/v1`, то в коде обычно достаточно передать собственный API key, base URL и точный ID модели.

## Что понадобится

- проект на Python или Node.js, где вы используете LangChain
- аккаунт на `https://leantoken.tech`
- API key LeanToken
- OpenAI-compatible endpoint `https://api.leantoken.tech/v1`
- модель `qwen3-235b`
- опционально аккаунт LangSmith, если хотите трассировку и отладку вызовов

## Сценарии использования AI

LangChain особенно полезен в таких сценариях:

- AI-ассистент внутри продукта или внутреннего портала, который вызывает корпоративные API и tools
- chat-интерфейс или внутренний AI-ассистент, который работает через один стабильный model ID
- извлечение структурированных данных из писем, договоров, анкет, карточек товаров и других текстов
- агентные workflow, где модель должна сама выбирать tool, выполнять шаги и собирать итоговый ответ
- перенос одного и того же приложения между разными model providers через стандартный интерфейс LangChain
- трассировка, дебаг и сравнение прогонов через LangSmith без полной переписки основного кода

## BYOK на leantoken.tech

В контексте LangChain BYOK на `leantoken.tech` означает, что вы не привязываете приложение к встроенному ключу конкретного провайдера, а передаете в LangChain собственный ключ и собственный endpoint.

Практически это дает несколько плюсов:

- один и тот же ключ можно использовать в локальной разработке, staging и production
- один и тот же model ID `qwen3-235b` можно использовать во всех chat-сценариях приложения
- один и тот же код легче переиспользовать в Python и TypeScript
- вы сами контролируете хранение секрета в `.env`, Docker secrets, CI/CD variables или cloud secret manager
- стоимость смены провайдера ниже, потому что LangChain изначально строится вокруг стандартных интерфейсов

Важно учитывать ограничение из официальной документации LangChain: `ChatOpenAI` ориентирован на официальный OpenAI API spec. Если upstream-совместимость неполная или провайдер добавляет нестандартные поля ответа, LangChain может их не сохранять. Для обычного OpenAI-compatible BYOK-потока с key, model и base URL это обычно не проблема, но на provider-specific расширения лучше не опираться.

## Как получить API key на leantoken.tech

Перед настройкой LangChain получите key и endpoint в LeanToken.

![Публичная страница LeanToken с Sign up, Log in и Dashboard](../../images/leantoken/leantoken.tech.1.landing.png)

1. Откройте `https://leantoken.tech`.
2. Нажмите `Sign up`, если у вас еще нет аккаунта, `Log in`, если аккаунт уже есть, или `Dashboard`, если вы уже вошли в систему.

![Панель LeanToken с API endpoint и быстрыми ссылками](../../images/leantoken/leantoken.tech.2.dashboard.png)

3. После входа откройте dashboard или обзорную страницу API.
4. Скопируйте API endpoint. На скриншотах показан `https://api.leantoken.tech/v1`.
5. Перейдите в раздел управления ключами через `Manage keys` или `API Keys`.

![Страница API Keys в LeanToken с действием Create key](../../images/leantoken/leantoken.tech.3.key-managment.png)

6. Нажмите `Create key`.
7. Скопируйте созданный API key.
8. Вернитесь в проект LangChain и вставьте key в `.env` или другой secrets-store приложения. Если интеграция идет через OpenAI-compatible API, сохраните рядом и base URL `https://api.leantoken.tech/v1`.
9. Не коммитьте ключ в git. Если он попал в лог, скриншот, issue или репозиторий, перевыпустите его и обновите конфигурацию приложения.

## Как добавить BYOK

LangChain не настраивается через отдельный GUI-экран сервиса, как IDE или web app. Для LangChain ключ обычно добавляется в `.env`, а затем явно передается в код инициализации модели.

### 1. Подготовьте `.env`

Удобная базовая схема для LangChain-проекта:

```bash
LEANTOKEN_API_KEY=lt_your_key_here
LEANTOKEN_BASE_URL=https://api.leantoken.tech/v1
LEANTOKEN_MODEL=qwen3-235b
```

Имена переменных могут быть любыми. В этом руководстве model ID зафиксирован как `qwen3-235b`, потому что для этой интеграции вы используете только его. Ниже в кодовых примерах он также указан явно.

### 2. Python: подключите chat-модель через `ChatOpenAI`

Если вам нужен прямой OpenAI-compatible путь для chat completion, используйте пакет `langchain-openai`.

```bash
pip install -U langchain-openai
```

```python
import os
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="qwen3-235b",
    api_key=os.environ["LEANTOKEN_API_KEY"],
    base_url=os.environ.get("LEANTOKEN_BASE_URL", "https://api.leantoken.tech/v1"),
    temperature=0,
)

response = llm.invoke(
    [
        ("system", "Ты помогаешь разработчику и отвечаешь кратко."),
        ("human", "Объясни, что такое LangChain в одном абзаце."),
    ]
)

print(response.content)
```

Это самый прямой путь, если вы уже строите chain, tool-calling или agent logic поверх конкретного chat model object.

### 3. Python: используйте `init_chat_model`, если хотите более универсальный вход

В официальной документации LangChain есть и более общий путь через `init_chat_model`. Он особенно удобен, если вы хотите держать код ближе к provider-agnostic API LangChain.

```bash
pip install -U "langchain[openai]"
```

```python
import os
from langchain.chat_models import init_chat_model

model = init_chat_model(
    model="qwen3-235b",
    model_provider="openai",
    api_key=os.environ["LEANTOKEN_API_KEY"],
    base_url=os.environ.get("LEANTOKEN_BASE_URL", "https://api.leantoken.tech/v1"),
    temperature=0,
)

result = model.invoke("Кратко перечисли сильные стороны LangChain для AI-приложений.")
print(result.text)
```

Если вы хотите позже менять провайдера, такой вход обычно проще масштабируется, чем жесткая привязка к одному классу.

### 4. Embeddings и RAG

В этой версии страницы embedding-примеры не приводятся. По вашему ограничению для LangChain доступна только модель `qwen3-235b`, а этот гайд описывает именно chat-подключение через OpenAI-compatible endpoint.

Если позже для LeanToken появится отдельная embedding-модель, ее стоит задокументировать отдельно и не смешивать с текущей схемой для `qwen3-235b`.

### 5. JavaScript / TypeScript: подключите `ChatOpenAI`

В LangChain JS для OpenAI-compatible URL используется `configuration.baseURL`.

```bash
npm install @langchain/openai @langchain/core
```

```ts
import "dotenv/config";
import { ChatOpenAI } from "@langchain/openai";

const llm = new ChatOpenAI({
  model: "qwen3-235b",
  apiKey: process.env.LEANTOKEN_API_KEY!,
  temperature: 0,
  configuration: {
    baseURL:
      process.env.LEANTOKEN_BASE_URL ?? "https://api.leantoken.tech/v1",
  },
});

const response = await llm.invoke(
  "Коротко объясни, как LangChain помогает строить агентные workflow."
);

console.log(response.content);
```

В официальной JS-документации LangChain отдельно отмечено, что некоторые proxy или third-party providers не поддерживают `stream_options`. Если вы видите ошибку именно на streaming usage metadata, попробуйте добавить `streamUsage: false`.

```ts
const llm = new ChatOpenAI({
  model: "qwen3-235b",
  apiKey: process.env.LEANTOKEN_API_KEY!,
  streamUsage: false,
  configuration: {
    baseURL:
      process.env.LEANTOKEN_BASE_URL ?? "https://api.leantoken.tech/v1",
  },
});
```

### 6. JavaScript / TypeScript: используйте `initChatModel` для универсальной схемы

Если хотите инициализировать модель через общий API LangChain, используйте `initChatModel`.

```bash
npm install langchain @langchain/openai
```

```ts
import "dotenv/config";
import { initChatModel } from "langchain";

const model = await initChatModel("qwen3-235b", {
  modelProvider: "openai",
  apiKey: process.env.LEANTOKEN_API_KEY!,
  baseUrl:
    process.env.LEANTOKEN_BASE_URL ?? "https://api.leantoken.tech/v1",
  temperature: 0,
});

const response = await model.invoke(
  "Сделай короткий список причин использовать LangChain."
);

console.log(response.content);
```

Этот путь удобен, когда вы строите приложение вокруг общих интерфейсов LangChain и хотите позже менять модельный backend с минимальными правками.

### 7. Передайте модель дальше в agent или chain

После этого не нужно изобретать вторую схему аутентификации. Тот же объект модели можно использовать дальше в LangChain:

- в `create_agent(...)`
- в chain с prompt templates
- в tool-calling workflow
- в LangSmith tracing и evaluation flow

Идея простая: BYOK обычно настраивается один раз на уровне model object, а потом переиспользуется по всему приложению.

## LangSmith и трассировка

В материалах из `source/` отдельно присутствует LangSmith tracing. Это полезно, если вы хотите видеть фактические prompts, tool calls, latencies и ошибки после перехода на BYOK.

Минимальная схема:

```bash
LANGSMITH_TRACING=true
LANGSMITH_API_KEY=your_langsmith_key
LANGSMITH_PROJECT=langchain-leantoken
```

После этого LangChain и LangSmith смогут логировать вызовы модели, если ваш код уже использует LangChain runtime и у вас настроен ключ LangSmith.

## Практические рекомендации

- Держите LeanToken key в `.env`, secrets manager или CI/CD variables, а не в исходниках.
- Не заменяйте `qwen3-235b` на произвольный model ID, если для вашей интеграции разрешена только эта модель.
- Если вы строите provider-agnostic архитектуру, выбирайте `init_chat_model` или `initChatModel`.
- Если вы уже сидите на `ChatOpenAI`, достаточно просто передать LeanToken key и base URL явно.
- Если в JS появляются ошибки вокруг streaming usage, попробуйте `streamUsage: false`.
- Если вам нужна расширенная observability, добавьте LangSmith до выхода в production.

## Что проверить после настройки

- приложение больше не пытается ходить в дефолтный endpoint провайдера, а использует `https://api.leantoken.tech/v1`
- `ChatOpenAI`, `init_chat_model` или `initChatModel` получают корректный API key без `401 Unauthorized`
- в каждом примере используется именно `qwen3-235b`
- ключ не попал в git, логи контейнера, issue tracker или публичные скриншоты
- если включен LangSmith, вы видите trace после тестового вызова

## Итоговая схема

Для LangChain BYOK через `leantoken.tech` обычно сводится к четырем действиям:

1. получить LeanToken API key и endpoint
2. сохранить их в `.env` или secret manager
3. передать их в `ChatOpenAI`, `init_chat_model` или `initChatModel`, используя модель `qwen3-235b`
4. переиспользовать этот объект модели во всех chain, agent и tool-calling workflow

Если ваш проект уже использует OpenAI-compatible абстракции LangChain, переход на LeanToken обычно требует минимальных изменений в коде.
