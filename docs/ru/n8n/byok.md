# n8n

## О сервисе

n8n — это платформа для автоматизации workflow, интеграции сервисов по API и сборки многошаговых процессов из узлов. В официальной документации n8n описывает себя как workflow automation tool с AI-возможностями, где можно соединять приложения, обрабатывать данные, запускать chat-интерфейсы, добавлять модели, память, tools и логику маршрутизации.

Для сценариев с AI это особенно удобно, потому что n8n не ограничивается одним окном чата. Здесь модель становится частью реального процесса:

- получает входящее событие
- читает данные из CRM, почты, таблиц или webhook
- генерирует ответ, summary или структуру
- передает результат в следующий узел
- сохраняет лог, уведомляет команду или вызывает внешний API

В контексте `leantoken.tech` n8n хорошо подходит для BYOK, потому что типовой AI-стек в n8n уже умеет работать через OpenAI-compatible credential. Это позволяет использовать LeanToken как единый endpoint `https://api.leantoken.tech/v1` и переиспользовать один credential в нескольких workflow.

## Что понадобится

- доступ к `n8n Cloud` или self-hosted instance `n8n`
- права на создание workflow и credentials
- аккаунт на `https://leantoken.tech`
- API key LeanToken
- OpenAI-compatible base URL `https://api.leantoken.tech/v1`
- модель, доступная в вашем аккаунте LeanToken
- понимание, какой сценарий вы строите:
  - chat / agent workflow через `Chat Trigger`, `AI Agent` и `OpenAI Chat Model`
  - обычную автоматизацию через `OpenAI` node без агента

## Сценарии использования AI

n8n особенно полезен там, где AI нужен не сам по себе, а как шаг внутри автоматизации.

Практические сценарии:

- классификация лидов, тикетов и входящих писем перед передачей в CRM или helpdesk
- суммаризация диалогов, meeting notes и длинных писем с отправкой результата в Slack, Telegram или Notion
- извлечение структурированных данных из анкет, документов, форм и webhook payload
- AI-ответ в чат-воркфлоу через `Chat Trigger` и `AI Agent`
- enrichment карточек клиентов, товаров и заявок перед записью в таблицу или базу
- маршрутизация запросов по intent: поддержка, продажи, onboarding, финансы
- внутренние AI-ассистенты, которые используют tools и внешние API
- генерация черновиков ответов, FAQ, резюме, metadata и JSON-структур для следующих узлов

## Почему BYOK в n8n особенно полезен

Для n8n BYOK через `leantoken.tech` удобен по нескольким причинам:

- один и тот же key можно переиспользовать в нескольких workflow
- вы сами контролируете модель, endpoint и жизненный цикл ключа
- один OpenAI-compatible credential можно использовать и в `OpenAI Chat Model`, и в `OpenAI` node
- проще разводить dev, staging и production через разные credentials
- если у вас self-hosted `n8n`, AI-вызовы можно встроить в уже существующие внутренние процессы без смены архитектуры

Отдельный плюс n8n в том, что credentials здесь являются отдельным объектом платформы. Это лучше, чем вставлять ключ вручную в каждый узел. По официальной документации n8n хранит credentials в зашифрованном виде в своей базе данных, а для Enterprise-планов также поддерживает external secrets vault.

## Как получить API key на leantoken.tech

Перед настройкой `n8n` получите key и endpoint в LeanToken.

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
8. Вернитесь в `n8n` и подготовьте credential, в который вы вставите:
   - `API key`
   - `Base URL`: `https://api.leantoken.tech/v1`
9. Не публикуйте ключ в workflow export, issue, скриншотах и git. Если ключ был раскрыт, перевыпустите его и обновите credential.

## Минимальная схема BYOK в n8n

Самый наглядный стартовый сценарий для `n8n` выглядит так: пользователь пишет сообщение в чат, workflow принимает это событие, `AI Agent` обрабатывает запрос, а `OpenAI Chat Model` обращается к LeanToken через OpenAI-compatible credential.

![Минимальный AI workflow в n8n: Chat Trigger -> AI Agent -> OpenAI Chat Model](../../images/n8n/01-simple-n8n-setup.png)

На скриншоте видно базовую связку:

1. `When chat message received` принимает сообщение пользователя
2. `AI Agent` управляет логикой ответа
3. `OpenAI Chat Model` вызывает модель через credential
4. в нижней панели можно сразу увидеть input, output и token usage

Это хороший стартовый каркас для smoke test. После этого workflow можно расширять памятью, tools, ретриверами, внешними API, таблицами и любыми другими узлами n8n.

Важно учитывать нюанс из официальной документации n8n:

- `Chat Trigger` используется для AI workflow и chat-интерфейсов
- каждое сообщение в `Chat Trigger` запускает отдельное исполнение workflow
- в текущей документации `AI Agent` описан как `Tools Agent`, а для некоторых версий и конфигураций он ожидает хотя бы один tool sub-node

Если ваш instance `n8n` просит обязательный tool sub-node, есть два практических пути:

1. добавить совместимый tool и оставить `AI Agent`
2. для простого prompt-response сценария использовать `OpenAI` node без агента

## Как создать credential OpenAI-compatible для LeanToken

В n8n удобнее всего начать с отдельного credential типа `OpenAI` или `OpenAI account`, а затем переиспользовать его в нужных узлах.

![Экран credential OpenAI account в n8n с полями API Key, Organization ID и Base URL](../../images/n8n/02-model-configuration.png)

Типовой поток:

1. Откройте `Credentials` в интерфейсе n8n или нажмите создание credential прямо из `OpenAI Chat Model` или `OpenAI` node.
2. Выберите credential для `OpenAI`.
3. В поле `API Key` вставьте ваш LeanToken key.
4. Если в форме есть `Organization ID`, для LeanToken его обычно оставляют пустым. Это поле относится к обычному OpenAI account flow и не требуется для типовой LeanToken-конфигурации.
5. В поле `Base URL` укажите `https://api.leantoken.tech/v1`.
6. Назовите credential так, чтобы его было легко найти, например `LeanToken OpenAI`.
7. Сохраните credential.

Практические замечания:

- если модельный список потом не загружается, первым делом проверьте именно `Base URL`
- если у вас несколько окружений, заведите отдельные credentials: `LeanToken Dev`, `LeanToken Prod`
- если вы работаете в команде, проверьте, кому разрешен доступ к credential и в каком project он доступен

## Как хранить ключ безопасно в n8n

По официальной документации n8n хранит credentials в зашифрованном виде в базе данных. Для многих команд этого уже достаточно, но если у вас Enterprise Self-hosted или Enterprise Cloud, можно пойти дальше и подключить external secrets.

Когда это полезно:

- один и тот же секрет используется в нескольких окружениях
- ключи должны храниться не в самой базе n8n, а во внешнем vault
- вы хотите централизованную ротацию и более жесткий контроль доступа

В документации n8n для external secrets описаны провайдеры `1Password`, `AWS Secrets Manager`, `Azure Key Vault`, `GCP Secrets Manager` и `HashiCorp Vault`. После подключения vault секреты можно подставлять в credential через expression вида `{{ $secrets.<vault-name>.<secret-name> }}`.

## Вариант 1. Настройка через AI Agent и OpenAI Chat Model

Этот путь подходит, если вы строите чат-бота, AI-ассистента или более сложный workflow, где модель должна работать как часть агентной логики.

### Шаг 1. Добавьте Chat Trigger

1. Создайте новый workflow.
2. Добавьте `Chat Trigger`.
3. Пока вы только настраиваете поток, оставьте публичный доступ отключенным.
4. Когда дойдете до публикации, выберите один из режимов:
   - `Hosted Chat`, если хотите использовать готовый chat UI n8n
   - `Embedded Chat`, если у вас будет собственный интерфейс или widget
5. Если чат должен быть ограничен, настройте аутентификацию:
   - `None`
   - `Basic Auth`
   - `n8n User Auth`

Это важно для эксплуатации, потому что каждый входящий chat message будет создавать отдельное execution.

### Шаг 2. Добавьте AI Agent

1. Подключите `AI Agent` к `Chat Trigger`.
2. Используйте его как центральный узел, который принимает пользовательский запрос и решает, как формировать ответ.
3. Если ваш workflow будет расширяться, к агенту можно подключать:
   - `Chat Model`
   - память
   - tools
   - retrievers
   - sub-workflows

Если вы строите только самый простой ответ без tools, учитывайте различия между версиями n8n. В актуальной документации `AI Agent` описан как `Tools Agent`, но минимальный тестовый canvas у вас уже сохранен на скриншотах именно в этой форме, поэтому ориентируйтесь на поведение вашей версии instance.

### Шаг 3. Подключите OpenAI Chat Model

![Узел OpenAI Chat Model в n8n с выбранным credential и моделью qwen3-235b](../../images/n8n/03-ai-agent-config.png)

1. Добавьте `OpenAI Chat Model` как sub-node к `AI Agent`.
2. В поле `Credential to connect with` выберите созданный LeanToken credential.
3. В списке `Model` выберите модель, доступную в вашем LeanToken-аккаунте.
4. На вашем скриншоте показан пример с `qwen3-235b`, но фактический список определяется тем, какие модели видит credential через ваш endpoint.

Официальная документация n8n отдельно отмечает два режима работы этого узла:

- `Chat Completions`
- `Responses API`

Что это означает на практике:

- если нужен максимально простой совместимый путь, начните с базового chat completion сценария
- если вы строите новый проект и ваш endpoint поддерживает этот режим, можно включить `Use Responses API`
- при использовании `Responses API` становятся доступны built-in tools OpenAI
- built-in tools в документации n8n описаны для связки `OpenAI Chat Model` + `AI Agent`

Если после включения `Use Responses API` появляются ошибки совместимости, вернитесь к базовому пути без этой опции. Для OpenAI-compatible gateway это нормальная диагностическая развилка: часть endpoint-ов поддерживает только основной chat flow, а не весь набор OpenAI Responses features.

### Шаг 4. Настройте параметры модели

Для большинства сценариев достаточно сначала заполнить только credential и model, а затем уже при необходимости добавлять дополнительные параметры.

Полезные настройки:

- `Maximum Number of Tokens`, если нужно ограничить длину ответа
- `Sampling Temperature`, если нужен более предсказуемый или более творческий ответ
- `Timeout`, если workflow не должен ждать слишком долго
- `Max Retries`, если хотите переживать кратковременные сетевые ошибки

Не добавляйте слишком много опций на первом проходе. Для smoke test лучше проверить сначала связку `credential + base URL + model`.

### Шаг 5. Протестируйте ответ

1. Запустите workflow вручную.
2. Отправьте короткое сообщение в чат, например `Привет` или `Сделай краткое summary этого текста`.
3. Проверьте, что в output узла появляется ответ модели.
4. Откройте execution details и убедитесь, что:
   - запрос уходит без ошибки авторизации
   - модель реально отвечает
   - token usage отображается ожидаемо

## Вариант 2. Настройка через OpenAI node без агента

Если вам не нужен агентный сценарий, а нужно просто один раз вызвать модель внутри автоматизации, удобнее использовать обычный `OpenAI` node.

Этот путь особенно хорош для:

- суммаризации текста из предыдущего узла
- классификации входящих данных
- генерации JSON-структуры
- извлечения полей из документа
- enrich / rewrite / normalize этапа в середине workflow

По актуальной документации n8n у `OpenAI` node для текста есть как минимум два полезных режима:

- `Generate a Chat Completion`
- `Generate a Model Response`

Практическая разница:

- `Generate a Chat Completion` использует Chat Completions API
- `Generate a Model Response` использует Responses API

Базовый поток:

1. Добавьте `OpenAI` node.
2. Выберите тот же LeanToken credential.
3. В `Resource` выберите `Text`.
4. В `Operation` выберите:
   - `Generate a Chat Completion`, если нужен максимально прямой prompt-response путь
   - `Generate a Model Response`, если вы сознательно работаете с Responses API
5. Выберите модель.
6. Добавьте сообщения с нужными ролями:
   - `System`, если хотите задать поведение модели
   - `User`, если передаете сам запрос
7. При необходимости включите:
   - `Simplify Output`
   - `Output Content as JSON`

Это часто проще, чем `AI Agent`, когда вам не нужны memory, tool calling и чат как продуктовый интерфейс.

## Когда выбирать AI Agent, а когда OpenAI node

Используйте `AI Agent`, если:

- вы строите chatbot или assistant
- модель должна выбирать инструменты
- нужен путь к memory, tools и более агентной логике
- у вас есть `Chat Trigger` и пользовательский диалог

Используйте `OpenAI` node, если:

- нужен одиночный вызов модели внутри большого workflow
- вы суммаризируете, классифицируете или переписываете данные
- ответ модели сразу идет в следующий обычный узел
- не хочется добавлять лишнюю агентную сложность

На практике многие workflow в n8n начинаются с `OpenAI` node, а к `AI Agent` переходят позже, когда действительно появляется необходимость в tools и более сложном reasoning loop.

## Рекомендуемые рабочие схемы

### 1. Email triage

Поток:

1. `Gmail Trigger` или `IMAP Email Trigger`
2. `OpenAI` node с классификацией письма
3. `Switch` или `If`
4. отправка в нужный канал: CRM, Slack, Notion, helpdesk

### 2. Lead enrichment

Поток:

1. `Webhook` или `Form Trigger`
2. очистка и нормализация входных полей
3. `OpenAI` node для краткого профиля лида и intent classification
4. запись результата в CRM

### 3. Внутренний AI-ассистент

Поток:

1. `Chat Trigger`
2. `AI Agent`
3. `OpenAI Chat Model`
4. tools для чтения таблиц, вызова API или запуска под-workflow

### 4. Извлечение структуры из текста

Поток:

1. `Webhook` принимает сырой текст или документ
2. `OpenAI` node возвращает JSON
3. `Edit Fields`, `Set`, `Code` или `IF` валидируют результат
4. данные уходят в БД, таблицу или downstream API

## Что проверить после настройки

Минимальный smoke test:

1. Убедитесь, что credential сохранен и использует `Base URL` `https://api.leantoken.tech/v1`.
2. Проверьте, что список моделей загружается и в нем видна хотя бы одна доступная вам модель.
3. Запустите самый короткий запрос без сложных параметров.
4. Убедитесь, что ответ возвращается без `401`, `403` или `404`.
5. Проверьте execution output и token usage.
6. Если используете `Chat Trigger`, проверьте чат сначала в непубличном режиме.
7. Если потом открываете чат наружу, настройте подходящий режим аутентификации.

## Типичные проблемы

### Модель не появляется в списке

Обычно это означает одно из трех:

- неправильно указан `Base URL`
- API key невалиден или не имеет доступа
- нужная модель недоступна в вашем LeanToken-аккаунте

### Ошибка авторизации

Проверьте:

- что вставлен актуальный LeanToken key
- что key не содержит лишних пробелов
- что credential использует именно `https://api.leantoken.tech/v1`

### Ошибка после включения Responses API

Если базовый chat flow работает, а `Use Responses API` ломается, проблема обычно в несовпадении поддерживаемых возможностей endpoint-а или модели. В таком случае:

1. выключите `Use Responses API`
2. проверьте простой chat completion путь
3. только потом возвращайтесь к более продвинутым функциям

### AI Agent просит tool sub-node

Это связано с текущим описанием `AI Agent` в документации n8n как `Tools Agent`. Если это происходит в вашей версии:

1. добавьте совместимый tool sub-node
2. или временно перенесите тест в `OpenAI` node без агента

### Публичный чат работает не так, как ожидалось

Проверьте:

- выключен или включен ли `Make Chat Publicly Available`
- какой выбран режим: `Hosted Chat` или `Embedded Chat`
- не мешают ли `Basic Auth`, `n8n User Auth` или `CORS` настройки

### Слишком быстро расходуются executions

Это важно для `Chat Trigger`: каждое сообщение пользователя запускает workflow заново. Если пользователь ведет долгий диалог, executions расходуются быстро, и это нужно учитывать в вашей модели эксплуатации.

## Ограничения и важные нюансы

- Точные названия полей и расположение кнопок могут отличаться между версиями `n8n`.
- В документации n8n `OpenAI Chat Model` динамически загружает список моделей из account credential, поэтому итоговый список зависит от LeanToken endpoint и доступов вашего аккаунта.
- `Responses API` и built-in tools стоит включать только после того, как подтвержден базовый рабочий путь.
- Если вы используете public chat, отдельно продумайте аутентификацию, CORS и стоимость исполнений.
- Если workflow содержит чувствительные данные, решите заранее, какие поля можно отправлять в модель, а какие нужно маскировать или исключать.

## Полезные ссылки

- [n8n Docs](https://docs.n8n.io/)
- [OpenAI credentials в n8n](https://docs.n8n.io/integrations/builtin/credentials/openai/)
- [Chat Trigger node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-langchain.chattrigger/)
- [AI Agent node](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)
- [OpenAI Chat Model node](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatopenai/)
- [OpenAI Text operations](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-langchain.openai/text-operations/)
- [External secrets в n8n](https://docs.n8n.io/external-secrets/)
- [LeanToken](https://leantoken.tech)
