# JetBrains IDEs

## О сервисе

JetBrains IDEs — это инструменты разработки от JetBrains, включая [CLion](https://blog.jetbrains.com/clion/), [DataGrip](https://blog.jetbrains.com/datagrip/), [DataSpell](https://blog.jetbrains.com/dataspell/), [GoLand](https://blog.jetbrains.com/go/), [IntelliJ IDEA](https://blog.jetbrains.com/idea/), [PhpStorm](https://blog.jetbrains.com/phpstorm/), [PyCharm](https://blog.jetbrains.com/pycharm/), [RustRover](https://blog.jetbrains.com/rust/), [Rider](https://blog.jetbrains.com/dotnet/tag/rider/), [RubyMine](https://blog.jetbrains.com/ruby/), и [WebStorm](https://blog.jetbrains.com/webstorm/).

Поддержка BYOK в JetBrains позволяет подключить собственного AI-провайдера и собственные модели прямо внутри IDE. Согласно сохраненной статье из блога JetBrains AI, BYOK доступен в AI chat и в AI-агентах, включая Junie и Claude Agent, если выбранный провайдер отдает нужные модели.

## Сценарии использования AI

- объяснение незнакомого кода, стек-трейсов и структуры проекта без выхода из IDE
- подготовка рефакторинга, тестов, скриптов и повторяющихся заготовок кода быстрее
- использование AI chat и агентных сценариев с нужной вам моделью для ревью кода, реализации задач и отладки
- подключение локальных или self-hosted endpoint-ов через OpenAI-compatible API, когда важны приватность или контроль стоимости
- генерация технической документации, заметок к изменениям и сводок по ревью во время работы с репозиторием

## BYOK на leantoken.tech

Bring Your Own Key на `leantoken.tech` означает, что вы используете собственные credentials провайдера вместо того, чтобы полагаться только на общий встроенный доступ. Для JetBrains IDEs это полезно, когда вы хотите сами контролировать модель, провайдера, endpoint, биллинг и границу приватности.

Согласно объявлению JetBrains, BYOK поддерживает API keys от Anthropic, OpenAI и других OpenAI-compatible провайдеров. JetBrains также отдельно отмечает, что ключи хранятся локально на вашей машине и не передаются JetBrains.

JetBrains также встроил короткое walkthrough-видео в исходную статью: [Bring Your Own Key in JetBrains IDEs](https://www.youtube.com/watch?v=JcJyoFyLEOs).

![JetBrains AI chat с моделями стороннего BYOK-провайдера](../../images/jetbrains-ides-byok-chat.webp)

Селектор моделей в чате может показывать модели, доступные через подключенного провайдера, поэтому переключаться между ними можно прямо внутри IDE.

## Как получить API key на leantoken.tech

Используйте этот поток перед настройкой JetBrains IDEs:

![Публичная страница LeanToken с Sign up, Log in и Dashboard](../../images/leantoken.tech.1.landing.png)

1. Откройте `https://leantoken.tech`.
2. Нажмите `Sign up`, если у вас еще нет аккаунта, `Log in`, если аккаунт уже есть, или `Dashboard`, если вы уже вошли в систему.

![Панель LeanToken с API endpoint и быстрыми ссылками](../../images/leantoken.tech.2.dashboard.png)

3. После входа откройте dashboard или обзорную страницу.
4. Скопируйте API endpoint, который показан там. На скриншотах это `https://api.leantoken.tech/v1`.
5. Используйте `Manage keys` или откройте `API Keys` через левое боковое меню.

![Страница API Keys в LeanToken с действием Create key](../../images/leantoken.tech.3.key-managment.png)

6. На странице `API Keys` нажмите `Create key`.
7. Скопируйте созданный API key, который хотите использовать в JetBrains IDEs.
8. Держите в секрете и ключ, и endpoint. Если ключ был раскрыт, отзовите или перевыпустите его и замените в настройках IDE.

## Как добавить BYOK

Сохраненная статья JetBrains показывает путь настройки как `Settings | Tools | AI Assistant | Models`.

![Старая настройка BYOK на странице Models](../../images/jetbrains-ides-byok-models-settings.webp)

В более новой UI из вашего скриншота настройка находится в `Settings | Tools | AI Assistant | Providers & API keys`.

![Текущий экран JetBrains Providers & API keys для настройки BYOK](../../images/jetbrains-ides-byok-providers-api-keys.jpg)

Типовой поток в текущем UI:

1. Получите API key LeanToken и, если нужно, соответствующий API URL по инструкции выше.
2. В JetBrains IDEs откройте `Settings | Tools | AI Assistant | Providers & API keys`.
3. В блоке `Third-party AI providers` выберите тип провайдера.
4. Если LeanToken в вашей схеме доступен как OpenAI-compatible endpoint, укажите base URL `https://api.leantoken.tech/v1` и свой API key.
5. Нажмите `Test Connection`.
6. Включите `Tool calling`, если ваш провайдер и ваш сценарий это поддерживают.
7. В `Model Assignment` выберите, какие подключенные модели должны использоваться для core features и helpers.
8. Откройте AI chat и проверьте, что ожидаемые модели появились в селекторе.

## JetBrains AI и BYOK вместе

JetBrains прямо указывает, что не все AI-функции гарантированно работают только со сторонними моделями провайдера. Для cloud code completion и других editor-integrated функций JetBrains рекомендует также активировать JetBrains AI.

Практическая схема выглядит так:

- использовать собственный ключ для провайдера и моделей, которые вы хотите контролировать напрямую
- держать JetBrains AI активным, когда нужна лучшая совместимость со встроенными функциями IDE
- выбирать нужную связку провайдер-модель в чате, когда важны конкретные возможности, цена или профиль приватности

## Рекомендуемые сценарии BYOK

- Используйте модели Anthropic или OpenAI, когда нужна сильная помощь при программировании в chat и агентных сценариях.
- Используйте OpenAI-compatible gateway, если команда стандартизирует доступ к нескольким провайдерам через один endpoint.
- Используйте локальный endpoint, если хотите приватно экспериментировать внутри IDE, не отправляя prompts в публичный хостинг API.
- Используйте `leantoken.tech` как операционную точку, где вы управляете и переиспользуете собственные credentials провайдера во всей своей AI-конфигурации.

## Примечания

- JetBrains пишет, что ожидается поддержка новых провайдеров, включая Google Gemini, Azure и Amazon Bedrock.
- Точные названия пунктов меню могут отличаться в зависимости от версии IDE. В сохраненной статье используется `Models`, а на вашем скриншоте показан `Providers & API keys`.
- Поддержка агентов все равно зависит от того, отдает ли выбранный провайдер модели, которые требуются Junie или Claude Agent.
