# OpenClaw

![Главный экран сохраненной страницы OpenClaw с логотипом и слоганом сервиса](../../images/openclaw/openclaw.png)

## О сервисе

OpenClaw — это персональный AI-ассистент, который вы запускаете на своих устройствах. В сохраненных материалах сервис описан как self-hosted assistant с Gateway в роли control plane: через него OpenClaw связывает модели, каналы связи, tools, skills, сессии и WebChat.

OpenClaw умеет работать в тех каналах, которыми вы уже пользуетесь: WhatsApp, Telegram, Slack, Discord, Google Chat, Signal, iMessage, BlueBubbles, Microsoft Teams, Matrix, WeChat, WebChat и других. Отдельно в материалах отмечены voice-сценарии на macOS, iOS и Android, live Canvas, browser control, skills и multi-agent routing.

Рекомендуемый путь настройки в исходниках OpenClaw один и тот же: сначала установить CLI, затем пройти `openclaw onboard`, а уже внутри onboarding настроить Gateway, workspace, модельный доступ, каналы и skills.

## Что понадобится

- Node.js 24 рекомендуется, Node.js 22.16+ поддерживается
- macOS или Linux; для Windows в материалах OpenClaw рекомендуется WSL2
- аккаунт на `https://leantoken.tech`
- API key LeanToken и, если нужен OpenAI-compatible endpoint, base URL
- машина, где будет работать Gateway: локальный компьютер или удаленный Linux-хост

## Сценарии использования AI

По сохраненным материалам OpenClaw особенно полезен в таких сценариях:

- личный AI-ассистент в мессенджерах и WebChat, который отвечает в привычных каналах вместо отдельного веб-чата
- помощь в разработке: обзор кодовой базы, запуск тестов, разбор ошибок, работа с GitHub и подготовка pull request
- DevOps и операционные задачи: просмотр логов, контроль сервисов, алерты и повторяющиеся runbook-процессы
- исследовательские задачи: поиск и синтез информации, работа с заметками и knowledge workflows
- голосовые сценарии и device-local actions через companion apps и nodes
- автоматизация браузера, Canvas и других tool-driven процессов

Если использовать BYOK через `leantoken.tech`, один gateway и один набор credentials можно переиспользовать во всех этих сценариях, не завязываясь только на встроенную OAuth-подписку конкретного провайдера.

## BYOK на leantoken.tech

В контексте OpenClaw BYOK означает, что вы подключаете собственный API key для модели или gateway вместо того, чтобы полагаться только на OAuth-подписку провайдера. В сохраненном GitHub-материале OpenClaw прямо разделяет два пути аутентификации:

- subscriptions через OAuth
- API keys

Там же отдельно упоминаются:

- поддержка многих providers и models
- auth profile rotation для OAuth и API keys
- model failover
- рекомендация использовать сильную актуальную модель, доступную вам

Для `leantoken.tech` это удобно, когда вы хотите:

- держать модельный доступ отдельно от личной OAuth-подписки
- централизованно управлять ключом и endpoint
- переключать модели без смены общей схемы интеграции
- использовать один gateway для OpenClaw, каналов, skills и automation flows

Сохраненные материалы OpenClaw рекомендуют строить такую настройку вокруг `openclaw onboard`, поэтому практический поток ниже опирается именно на onboarding, а не на ручное редактирование внутренних файлов с недокументированными полями.

## Как получить API key на leantoken.tech

Перед настройкой OpenClaw получите key и endpoint в LeanToken.

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
8. Сохраните и endpoint, и key в секрете. Если ключ был раскрыт, перевыпустите его и замените в OpenClaw.

## Как добавить BYOK

Ниже поток, который лучше всего совпадает с сохраненными материалами OpenClaw.

### 1. Установите OpenClaw

```bash
npm install -g openclaw@latest
```

Если вы хотите сразу установить daemon и пройти рекомендуемый onboarding-поток, используйте:

```bash
openclaw onboard --install-daemon
```

В исходниках этот путь назван preferred setup: onboarding шаг за шагом помогает настроить Gateway, workspace, channels и skills.

### 2. Выберите API-key путь на этапе model/auth setup

Во время onboarding дойдите до этапа настройки модельного доступа. Если вам нужен BYOK через `leantoken.tech`, ориентируйтесь на API-key схему, а не на OAuth subscription.

Практически это означает следующее:

1. Подготовьте LeanToken API key заранее.
2. Если мастер настройки запрашивает endpoint или base URL для OpenAI-compatible провайдера, используйте `https://api.leantoken.tech/v1`.
3. Вставьте LeanToken API key в шаге аутентификации провайдера.
4. Выберите модель, доступную через ваш LeanToken endpoint.

Сохраненные материалы OpenClaw не показывают точные названия всех экранов onboarding по версиям, поэтому формулировки в мастере могут немного отличаться. Но общая логика в исходниках стабильна: сначала auth, потом выбор модели, затем подключение каналов и skills.

### 3. Завершите настройку Gateway и каналов

После model/auth setup закончите onboarding:

- установите Gateway daemon, если он еще не установлен
- настройте workspace
- подключите нужные каналы, например Telegram, Slack, Discord, WhatsApp или WebChat
- добавьте skills только для тех задач, которые реально будете использовать

В сохраненных материалах OpenClaw прямо сказано, что Gateway — это control plane, а сам assistant работает уже поверх подключенных каналов, apps и tools.

### 4. Проверьте, что OpenClaw отвечает через ваш BYOK

Для первичной проверки используйте dashboard и локальный Gateway.

В сохраненных материалах упоминается локальный dashboard по адресу:

```text
http://localhost:18789/
```

После onboarding проверьте:

1. что Gateway запущен
2. что модельный доступ работает без ошибки аутентификации
3. что assistant отвечает в CLI, WebChat или подключенном канале

Для ручной проверки можно использовать CLI-сообщение:

```bash
openclaw agent --message "Сделай короткую самопроверку конфигурации и подтверди, что модель отвечает" --thinking high
```

Если нужен отладочный запуск в foreground, в сохраненных материалах также есть пример:

```bash
openclaw gateway --port 18789 --verbose
```

## Рекомендуемые сценарии BYOK

BYOK через LeanToken особенно полезен для OpenClaw в таких задачах:

- один assistant обслуживает сразу несколько каналов, а модельный доступ централизован в одном key и endpoint
- вы хотите контролировать биллинг и не завязываться только на OAuth subscription
- нужно быстро менять модели для разных задач: coding, research, summaries, background automation
- важно сохранить общий Gateway и workflow OpenClaw, но при этом использовать собственный путь доступа к моделям

## Практические рекомендации

Сохраненные материалы OpenClaw отдельно подчеркивают вопросы безопасности и эксплуатации. Для BYOK-сценария это особенно важно.

- Не публикуйте Gateway напрямую в интернет. В материалах рекомендуется держать его на loopback и для удаленного доступа использовать Tailscale или SSH tunnels.
- Не храните API key в репозитории и не вставляйте его в публичные конфиги. Держите ключ как секрет и меняйте его при утечке.
- Относитесь к skills как к коду с доступом к вашим данным и инструментам. Устанавливайте только те skills, которым доверяете.
- Для group и channel сценариев используйте более жесткую изоляцию, если assistant получает доступ к shell и automation tools.
- После настройки прогоните `openclaw doctor`, если хотите быстро найти рискованные или сломанные части конфигурации.

## Что проверить после настройки

- OpenClaw запускается без ошибок на вашей версии Node.js
- Gateway доступен локально и открывает dashboard на `localhost:18789`
- выбранная модель действительно отвечает через ваш LeanToken key
- assistant виден в нужном канале связи
- ключ и endpoint сохранены как секреты и не попали в git, shared notes или shell history
