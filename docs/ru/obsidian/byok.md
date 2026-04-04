# Obsidian

## О сервисе

Obsidian — это local-first редактор заметок и персональная база знаний поверх обычных Markdown-файлов. Его сильная сторона не в одном встроенном AI-модуле, а в экосистеме community plugins: вы храните vault у себя, сами выбираете workflow и подключаете только те AI-инструменты, которые реально нужны.

Для сценария BYOK в Obsidian удобно разделять две роли:

- `AI Providers` хранит и переиспользует настройки провайдеров AI в одном месте
- плагин-потребитель, например `Local GPT`, уже использует выбранный провайдер для действий над текстом, RAG, vision и других AI-функций

Это важное различие: `AI Providers` сам по себе не является чатом и не редактирует заметки. Это конфигурационный слой, который избавляет от повторного ввода `API key`, `Provider URL` и модели в каждом отдельном плагине.

В официальных материалах `AI Providers` описан как централизованный hub для AI provider configurations с поддержкой `20+` провайдеров, включая `OpenAI`, `Anthropic`, `OpenRouter`, `Ollama`, `Google Gemini`, `Groq`, `LM Studio` и `OpenAI compatible API`. Для `leantoken.tech` это особенно удобно, потому что LeanToken выдает OpenAI-compatible endpoint `https://api.leantoken.tech/v1`, который хорошо ложится в такой workflow.

В этом руководстве основной пример строится вокруг связки:

1. `Obsidian`
2. `AI Providers`
3. `Local GPT`
4. `leantoken.tech` как OpenAI-compatible BYOK endpoint

## Что понадобится

- установленный Obsidian Desktop
- включенные `Community plugins`
- плагин `AI Providers`
- плагин `Local GPT`, если вы хотите сразу использовать AI-действия внутри заметок
- аккаунт на `https://leantoken.tech`
- API key LeanToken
- base URL `https://api.leantoken.tech/v1`
- модель, доступная в вашем LeanToken-аккаунте
- опционально отдельный embedding provider, если вы хотите использовать RAG-функции в `Local GPT`

## Сценарии использования AI

Obsidian особенно хорош там, где AI должен работать не отдельно от ваших заметок, а прямо внутри живой базы знаний.

Практические сценарии:

- суммаризация длинных заметок, meeting notes, research-материалов и статей
- извлечение action items, тезисов, чек-листов и структурированных списков из текста
- продолжение черновика, переписывание абзацев, исправление стиля и орфографии
- Q&A по связанным заметкам, backlinks и PDF-файлам через плагины с RAG
- генерация новых заметок, промежуточных резюме, wiki-страниц и исследовательских дайджестов
- работа с локальными и облачными моделями в одном vault, когда часть задач вы хотите держать офлайн, а часть отправлять во внешний endpoint
- накопление результатов обратно в vault, чтобы каждое новое исследование усиливало вашу персональную базу знаний

Хороший практический образец такого подхода описал Andrej Karpathy в посте про `LLM Knowledge Bases`: он собирает сырые материалы в Markdown и изображения, использует LLM для постепенной "сборки" wiki, задает сложные вопросы по этой базе знаний и сохраняет результаты обратно в Obsidian. Ссылка на пост: [LLM Knowledge Bases by Andrej Karpathy](https://x.com/karpathy/status/2039805659525644595).

Если перенести этот workflow на Obsidian, получается понятная схема:

1. собрать статьи, заметки, репозитории, PDF и изображения в vault
2. использовать AI для суммаризации, категоризации и создания внутренних wiki-страниц
3. задавать вопросы по накопленному корпусу знаний
4. сохранять ответы, схемы, слайды и производные заметки обратно в vault

## Почему BYOK в Obsidian особенно полезен

Для Obsidian BYOK удобен по нескольким причинам:

- у вас остается один контролируемый ключ и один endpoint, который можно использовать в нескольких совместимых плагинах
- вы сами выбираете модель, а не зависите только от заранее зашитого провайдера конкретного плагина
- вы можете смешивать cloud и local setup, например держать `Ollama` для локальных задач и `LeanToken` для более сильных моделей
- перенос конфигурации между vault-ами и плагинами становится проще, потому что логика провайдера вынесена в отдельный hub
- проще управлять стоимостью, ротацией ключей и секретами

Для `leantoken.tech` это особенно естественный сценарий, потому что конечная настройка сводится к двум главным значениям:

- `API key`
- `Provider URL` или `base URL`: `https://api.leantoken.tech/v1`

## Как получить API key на leantoken.tech

Перед настройкой Obsidian получите key и endpoint в LeanToken.

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
8. Вернитесь в Obsidian и вставьте key в поле `API key` выбранного провайдера.
9. Вставьте `https://api.leantoken.tech/v1` в поле `Provider URL`, если используемый плагин ожидает отдельный endpoint.
10. Держите key в секрете. Если он попал в git, заметки, issue, скриншоты, shell history или логи, перевыпустите его.

## Какие плагины использовать для этого сценария

В Obsidian много AI-плагинов, но для документируемого и повторяемого BYOK-сценария удобнее брать связку, где роли разделены явно:

- `AI Providers` отвечает за конфигурацию провайдера
- `Local GPT` использует этот провайдер для конкретных AI-действий

Почему именно эта связка:

- README `AI Providers` прямо перечисляет `Local GPT` как плагин, который использует этот hub
- README `Local GPT` прямо указывает, что для настройки провайдеров нужен `AI Providers`
- `Local GPT` уже умеет типовые действия над текстом, поддержку изображений, action palette и RAG-сценарии

Если позже вы захотите подключить другие AI-плагины, логика остается той же: сначала проверяете, умеет ли плагин работать через `AI Providers`, и только потом переиспользуете уже созданный LeanToken provider.

## Как установить AI Providers и Local GPT

### 1. Включите community plugins

1. Откройте `Settings`.
2. Перейдите в `Community plugins`.
3. Если у вас включен `Restricted mode`, отключите его, чтобы ставить сторонние плагины.

### 2. Установите AI Providers

1. В разделе `Community plugins` нажмите `Browse`.
2. Найдите `AI Providers`.
3. Установите плагин и включите его.
4. После установки откройте его страницу настроек в `Settings`.

### 3. Установите Local GPT

1. Снова откройте `Browse` в `Community plugins`.
2. Найдите `Local GPT`.
3. Установите плагин и включите его.
4. При желании сразу настройте hotkeys для `Local GPT: Show context menu` и `Local GPT: Action Palette` через раздел `Hotkeys`.

## Как добавить LeanToken в AI Providers

Ниже показан экран `AI Providers` в настройках Obsidian со списком уже добавленных провайдеров.

![Экран AI Providers в Obsidian со списком настроенных провайдеров и кнопкой добавления нового провайдера](../../images/obsidian/obsidian-ai-providers-settings.png)

Типовой поток для LeanToken:

1. Откройте `Settings | Community plugins | AI Providers`.
2. Нажмите кнопку `+`, чтобы добавить нового провайдера.
3. Выберите тип провайдера, который в вашей версии плагина соответствует OpenAI-compatible API.
4. Если в интерфейсе есть явный вариант `OpenAI compatible API`, используйте его.
5. Если названия в UI немного отличаются, ориентируйтесь на главное требование: провайдер должен позволять вручную указать `Provider URL` и работать с OpenAI-compatible endpoint.
6. Назовите подключение, например `LeanToken`.
7. Вставьте `https://api.leantoken.tech/v1` в поле `Provider URL`.
8. Вставьте ваш LeanToken key в поле `API key`.
9. Нажмите кнопку обновления списка моделей или `refresh`, если она есть в вашей версии плагина.
10. Выберите модель из тех, которые доступны в вашем LeanToken-аккаунте.
11. Сохраните настройки.

После этого LeanToken provider должен появиться в общем списке AI Providers рядом с другими провайдерами.

Что здесь важно на практике:

- если вы хотите использовать несколько моделей, можно создать несколько provider entries с разными названиями
- если вам нужен отдельный embedding backend для RAG, его лучше завести как отдельную запись, а не смешивать с chat-конфигурацией
- если нужен отдельный vision-capable model, его тоже удобнее хранить как отдельный provider

## Как использовать LeanToken в Local GPT

После того как провайдер сохранен в `AI Providers`, можно переходить к действиям внутри заметок через `Local GPT`.

Базовый поток:

1. Откройте `Settings | Community plugins | Local GPT`.
2. Найдите выбор основного AI provider или связанный selector, который использует конфигурации из `AI Providers`.
3. Выберите ранее созданный LeanToken provider.
4. Если вы хотите использовать vision-функции, укажите отдельный provider с моделью, которая поддерживает изображения.
5. Если вы хотите использовать RAG-функции или enhanced actions, настройте embedding provider отдельно.
6. Откройте любую заметку, выделите текст и вызовите `Local GPT` через context menu или `Action Palette`.

У `Local GPT` есть удобные стартовые сценарии:

- `Continue writing`
- `Summarize text`
- `Fix spelling and grammar`
- `Find action items`
- `General help`

Этого достаточно, чтобы быстро проверить, что LeanToken-подключение реально работает.

## Рекомендуемый Obsidian workflow для базы знаний

Если вы используете Obsidian не как простой блокнот, а как AI-assisted knowledge base, удобно работать так:

1. собирайте источники в vault: статьи, конспекты, заметки, PDF, изображения и ссылки
2. при необходимости сохраняйте web-страницы в Markdown через Obsidian Web Clipper
3. используйте `Local GPT` для суммаризации, переписывания и извлечения структуры из заметок
4. храните производные материалы рядом с исходниками: summary notes, comparison notes, wiki pages, slide outlines
5. для больших vault-ов постепенно наращивайте внутренние связи между заметками, чтобы AI-plugins могли лучше использовать links и backlinks
6. для RAG-сценариев подключайте отдельный embedding provider только там, где он действительно нужен

Это хорошо совпадает с workflow из поста Karpathy:

- raw sources складываются в локальную базу знаний
- LLM помогает собирать wiki из Markdown-файлов
- сложные вопросы задаются уже не по одной заметке, а по накопленному корпусу
- результаты не теряются в чате, а сохраняются обратно в knowledge base

## Что проверить после настройки

Минимальный smoke test:

1. откройте заметку с несколькими абзацами текста
2. выделите фрагмент
3. вызовите `Local GPT`
4. выберите `Summarize text`
5. убедитесь, что ответ приходит без ошибки авторизации и без ошибки провайдера

Если ответ не приходит:

- проверьте, что `Provider URL` задан как `https://api.leantoken.tech/v1`
- проверьте, что в поле `API key` вставлен актуальный ключ LeanToken
- обновите список моделей через `refresh`
- убедитесь, что выбранная модель действительно доступна в вашем аккаунте
- если ломается именно RAG, проверьте embedding provider отдельно, потому что chat model и embeddings — это не одно и то же

## Ограничения и важные нюансы

- `AI Providers` не является самостоятельным AI-ассистентом. Он только хранит и отдает конфигурацию другим плагинам.
- Точные названия полей и presets могут немного отличаться в зависимости от версии плагина.
- `Local GPT` может использовать local models через `Ollama` и cloud/OpenAI-compatible providers одновременно, поэтому LeanToken удобно комбинировать с локальным стеком.
- Для vision-сценариев и RAG часто нужны отдельные provider entries под разные задачи.
- Если вы храните чувствительные заметки, заранее решите, какие типы данных допустимо отправлять во внешний AI endpoint, а какие лучше оставить на локальной модели.

## Полезные ссылки

- [AI Providers: документация](https://pfrankov-obsidian-ai-providers.mintlify.app/)
- [AI Providers: GitHub repository](https://github.com/pfrankov/obsidian-ai-providers)
- [AI Providers: страница community plugin](https://obsidian.md/plugins?id=ai-providers)
- [Local GPT: документация](https://pfrankov-obsidian-local-gpt.mintlify.app/)
- [Local GPT: GitHub repository](https://github.com/pfrankov/obsidian-local-gpt)
- [Local GPT: страница community plugin](https://obsidian.md/plugins?id=local-gpt)
- [Obsidian Web Clipper](https://obsidian.md/clipper)
- [Andrej Karpathy: LLM Knowledge Bases](https://x.com/karpathy/status/2039805659525644595)
