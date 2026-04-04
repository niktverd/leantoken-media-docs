# LightRAG

## О сервисе

LightRAG — это open source framework для retrieval-augmented generation, который объединяет обычный векторный поиск, извлечение сущностей и связей, а также knowledge graph. В официальном репозитории проект описан как `Simple and Fast Retrieval-Augmented Generation`, а основной рабочий сценарий строится вокруг LightRAG Server: локального сервера с Web UI, API и Ollama-compatible интерфейсом.

Практически это означает, что LightRAG не ограничивается схемой "разбили документы на куски и сделали similarity search". Во время индексации он строит несколько слоев знаний одновременно:

1. chunks для обычного RAG-поиска
2. сущности и отношения между ними
3. граф знаний для graph-aware retrieval
4. отдельные хранилища для документов, статусов, графа и векторов

Из-за этого LightRAG может отвечать не только на простые factual-вопросы по документам, но и на более сложные запросы, где важны связи, роли, события, зависимости и глобальная структура корпуса.

## Как LightRAG работает

Типичный workflow в LightRAG выглядит так:

1. Вы загружаете файлы или текст в сервер.
2. LightRAG разбивает документы на chunks.
3. LLM извлекает из текста сущности и отношения.
4. Embedding-модель кодирует chunks, сущности и отношения в векторы.
5. Сервис сохраняет результаты в storage backends.
6. Во время запроса LightRAG комбинирует vector retrieval, knowledge graph retrieval и, при необходимости, reranker.

Именно поэтому требования к модели у LightRAG выше, чем у простого RAG. По официальному README:

- для LLM рекомендуется модель минимум уровня `32B`
- длина контекста должна быть хотя бы `32KB`, лучше `64KB`
- на этапе индексации не рекомендуется reasoning-модель
- embeddings должны быть выбраны до первой индексации и потом не меняться без переиндексации
- reranker заметно улучшает mixed-query сценарии

## Сценарии использования AI

LightRAG особенно полезен в таких задачах:

- внутренний AI-поиск по wiki, runbook, engineering docs и операционным заметкам
- RAG по API-документации и техническим спецификациям, где важны связи между сущностями
- аналитические ассистенты для research, legal review, due diligence и knowledge management
- чат над своей базой знаний, где нужен не только ответ модели, но и более качественный retrieval
- knowledge graph по статьям, PDF, презентациям, policy-документам и проектной документации
- multimodal RAG-пайплайн, если вы позже захотите развивать установку в сторону RAG-Anything

Если говорить проще, LightRAG нужен там, где обычный "чат с файлами" уже слишком поверхностный, а пользователю важно, чтобы система понимала сущности, отношения и глобальную структуру корпуса.

## Что понадобится

Для собственного запуска LightRAG с BYOK обычно нужны:

- локальная установка LightRAG Server или исходники `HKUDS/LightRAG`
- `uv` или `pip` для Python-части
- `bun`, если вы собираете Web UI из исходников
- аккаунт на `https://leantoken.tech`
- API key LeanToken
- OpenAI-compatible endpoint `https://api.leantoken.tech/v1`
- сильная chat-модель, доступная на вашем LeanToken endpoint
- embedding backend, потому что LightRAG требует embeddings отдельно от LLM
- опционально reranker, если вы хотите лучший retrieval в `mix`-режиме

## Что важно понять перед BYOK

Для LightRAG BYOK нельзя свести к одной переменной с API key. У сервиса есть три независимых слоя конфигурации:

1. LLM
2. Embeddings
3. Reranker

Из них обязательны первые два. Без embeddings LightRAG не сможет нормально индексировать документы и выполнять поиск.

В `.env` это отражается так:

- `LLM_BINDING`, `LLM_MODEL`, `LLM_BINDING_HOST`, `LLM_BINDING_API_KEY`
- `EMBEDDING_BINDING`, `EMBEDDING_MODEL`, `EMBEDDING_DIM`, `EMBEDDING_BINDING_HOST`, `EMBEDDING_BINDING_API_KEY`
- `RERANK_BINDING`, `RERANK_MODEL`, `RERANK_BINDING_HOST`, `RERANK_BINDING_API_KEY`

Ключевой вывод для LeanToken такой:

- LeanToken удобно использовать как LLM backend через OpenAI-compatible API
- embedding-конфигурацию нужно проверить отдельно
- reranker можно сначала отключить, но для production retrieval его лучше продумать заранее

## BYOK на leantoken.tech

В контексте LightRAG BYOK на `leantoken.tech` обычно означает, что вы:

1. берете свой API key в LeanToken
2. указываете `https://api.leantoken.tech/v1` как `LLM_BINDING_HOST`
3. вставляете API key в `LLM_BINDING_API_KEY`
4. выбираете модель, которая реально доступна в вашем LeanToken-аккаунте

Это хороший сценарий для LightRAG, потому что сам проект официально поддерживает `openai` и OpenAI-compatible backend для LLM.

Но здесь есть важный практический нюанс: `LLM_BINDING_API_KEY` и `LIGHTRAG_API_KEY` — это не одно и то же.

- `LLM_BINDING_API_KEY` нужен LightRAG, чтобы обращаться к модели
- `LIGHTRAG_API_KEY` нужен уже вам, чтобы защитить API самого LightRAG Server

Их нельзя путать. Первый ключ открывает путь к провайдеру модели. Второй защищает ваш локальный или удаленный инстанс LightRAG.

## Как получить API key на leantoken.tech

Перед настройкой LightRAG получите key и endpoint в LeanToken.

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
8. Вернитесь в проект LightRAG и вставьте key в `.env` в поле `LLM_BINDING_API_KEY`.
9. В этом же `.env` укажите `LLM_BINDING_HOST=https://api.leantoken.tech/v1`.
10. Сохраните ключ как секрет. Если он попал в git, логи, issue, скриншот или shell history, перевыпустите его.

## Как выбрать модели для LightRAG

Перед настройкой `.env` полезно сразу определить три роли:

### 1. LLM для индексации и ответов

По рекомендациям LightRAG:

- берите сильную non-reasoning chat-модель
- ориентируйтесь минимум на класс `32B`
- желательно иметь контекст не меньше `32KB`, лучше `64KB`

Если в вашем LeanToken-аккаунте доступно несколько моделей, для первой установки имеет смысл выбрать стабильную general-purpose модель без лишней reasoning-логики на этапе document indexing.

### 2. Embedding-модель

В официальных материалах среди хороших вариантов прямо названы:

- `BAAI/bge-m3`
- `text-embedding-3-large`

Для LightRAG embedding-модель критична по двум причинам:

- она напрямую влияет на качество retrieval
- ее размерность (`EMBEDDING_DIM`) должна совпадать с реальной моделью

Если вы потом меняете embedding-модель или ее размерность, старую индексную базу обычно нужно пересоздать и переиндексировать.

### 3. Reranker

Reranker в LightRAG опционален, но официальный README отдельно рекомендует его для mixed-query сценариев. В upstream-примерах фигурируют:

- `BAAI/bge-reranker-v2-m3`
- Jina-based rerankers

Для первой установки можно начать с `RERANK_BINDING=null`, но для production retrieval этот слой часто оправдан.

## Рекомендуемая схема BYOK для LeanToken

Самый безопасный стартовый вариант для LightRAG сейчас такой:

- LeanToken использовать для LLM
- embeddings держать на отдельном backend, который вы уже контролируете
- reranker включать позже, после того как базовый индекс стабильно работает

Это лучше, чем пытаться сразу привязать к одному endpoint все три слоя и потом разбираться, что именно сломалось: chat, embeddings или rerank.

## Базовая конфигурация `.env`

Ниже практический шаблон для первого запуска.

```env
HOST=0.0.0.0
PORT=9621
SUMMARY_LANGUAGE=Russian
MAX_ASYNC=4
MAX_PARALLEL_INSERT=2

LLM_BINDING=openai
LLM_BINDING_HOST=https://api.leantoken.tech/v1
LLM_BINDING_API_KEY=lt_your_key_here
LLM_MODEL=<ваша_chat_модель_из_LeanToken>

EMBEDDING_BINDING=ollama
EMBEDDING_BINDING_HOST=http://localhost:11434
EMBEDDING_MODEL=bge-m3:latest
EMBEDDING_DIM=1024
OLLAMA_EMBEDDING_NUM_CTX=8192

RERANK_BINDING=null

LIGHTRAG_KV_STORAGE=JsonKVStorage
LIGHTRAG_DOC_STATUS_STORAGE=JsonDocStatusStorage
LIGHTRAG_GRAPH_STORAGE=NetworkXStorage
LIGHTRAG_VECTOR_STORAGE=NanoVectorDBStorage
```

Что здесь важно:

- `SUMMARY_LANGUAGE=Russian` полезен, если вы индексируете русскоязычные документы
- `LLM_BINDING=openai` подходит для OpenAI-compatible backend, включая LeanToken
- defaults `JsonKVStorage`, `JsonDocStatusStorage`, `NetworkXStorage` и `NanoVectorDBStorage` подходят для тестового стенда
- эта схема хороша для локального proof of concept, но не обязана быть вашей production-конфигурацией

## Вариант с OpenAI-compatible embeddings

Если у вас уже есть совместимый embedding backend, конфигурация может выглядеть так:

```env
EMBEDDING_BINDING=openai
EMBEDDING_BINDING_HOST=<ваш_embedding_endpoint>
EMBEDDING_BINDING_API_KEY=<ваш_embedding_key>
EMBEDDING_MODEL=<имя_embedding_модели>
EMBEDDING_DIM=<точная_размерность>
EMBEDDING_SEND_DIM=false
```

Для такого варианта важны три проверки:

1. endpoint действительно отдает embeddings в OpenAI-compatible формате
2. `EMBEDDING_MODEL` совпадает с реальным model ID
3. `EMBEDDING_DIM` совпадает с настоящей размерностью модели

Если хотя бы один из этих пунктов неверен, индексация и retrieval начнут работать нестабильно или сломаются полностью.

## Как установить LightRAG Server

У LightRAG есть несколько путей установки. Для большинства пользователей удобнее два варианта: исходники или Docker Compose.

### Вариант 1. Установка из исходников

Официальный workflow проекта выглядит так:

```bash
git clone https://github.com/HKUDS/LightRAG.git
cd LightRAG
make dev
source .venv/bin/activate
cd lightrag_webui
bun install --frozen-lockfile
bun run build
cd ..
cp env.example .env
```

Если вы не используете `make dev`, upstream также показывает эквивалентную схему через `uv`:

```bash
uv sync --extra test --extra offline
source .venv/bin/activate
```

После этого заполните `.env` и запускайте сервер.

### Вариант 2. Docker Compose

Если вам удобнее изолированный запуск:

```bash
git clone https://github.com/HKUDS/LightRAG.git
cd LightRAG
cp env.example .env
docker compose up
```

В этом варианте логика BYOK та же самая: LLM, embeddings и, при необходимости, reranker задаются через `.env`.

### Вариант 3. Setup wizard

В новых версиях upstream есть setup wizard, который умеет собирать `.env` и связанные compose-файлы:

```bash
make env-base
make env-storage
make env-server
make env-security-check
```

Для LightRAG это удобный вариант, если вы не хотите править длинный `env.example` вручную.

## Как добавить BYOK пошагово

Ниже наиболее практический поток для LeanToken.

### 1. Подготовьте `.env`

Скопируйте `env.example`:

```bash
cp env.example .env
```

После этого заполните минимум такие поля:

```env
SUMMARY_LANGUAGE=Russian

LLM_BINDING=openai
LLM_BINDING_HOST=https://api.leantoken.tech/v1
LLM_BINDING_API_KEY=lt_your_key_here
LLM_MODEL=<ваша_chat_модель_из_LeanToken>

EMBEDDING_BINDING=<ваш_embedding_binding>
EMBEDDING_BINDING_HOST=<ваш_embedding_endpoint>
EMBEDDING_BINDING_API_KEY=<ваш_embedding_key_если_нужен>
EMBEDDING_MODEL=<ваша_embedding_модель>
EMBEDDING_DIM=<размерность_embedding_модели>
```

Если reranker пока не нужен:

```env
RERANK_BINDING=null
```

### 2. Учитывайте, что `.env` загружается из текущей папки запуска

В официальном API README отдельно подчеркивается:

- `.env` должен лежать именно в startup directory
- LightRAG подгружает `.env` в системное окружение при старте
- системные переменные имеют приоритет выше `.env`
- после правок `.env` лучше открыть новую terminal session

Это важный момент, если вы держите несколько инстансов LightRAG с разными конфигурациями.

### 3. Запустите сервер

Для обычного локального режима:

```bash
lightrag-server
```

Для production-многопроцессного режима на Linux:

```bash
lightrag-gunicorn --workers 4
```

По официальной документации дефолтный порт — `9621`.

### 4. Откройте Web UI

После старта обычно используйте:

```text
http://localhost:9621
```

В Web UI проверьте:

1. что сервер открылся без ошибок
2. что загрузка документа проходит
3. что pipeline индексации завершается без ошибок LLM и embeddings
4. что после индексации можно выполнить query

### 5. Начните с маленького корпуса документов

Для первой проверки BYOK не стоит сразу грузить десятки больших PDF.

Лучше:

- взять 3-5 документов
- дождаться полной индексации
- протестировать несколько типов запросов
- только потом увеличивать объем данных и сложность storage-конфигурации

## Как работать с Web UI и API

LightRAG Server полезен тем, что закрывает сразу три сценария:

- Web UI для интерактивной работы
- REST API для интеграции в свой продукт
- Ollama-compatible interface для подключения внешних AI chat clients

Типичный цикл работы такой:

1. загрузить документы
2. дождаться индексации
3. открыть knowledge graph или query interface
4. прогнать несколько типов запросов
5. при необходимости донастроить reranker, storage и ограничения контекста

Если вы интегрируете LightRAG в другой продукт, сам upstream рекомендует ориентироваться именно на REST API сервера, а не на прямое встраивание LightRAG Core, если у вас нет отдельной исследовательской задачи.

## Режимы запроса

В LightRAG есть несколько query modes. На практике полезно знать такие:

- `local` — больше ориентирован на контекстно-зависимую локальную информацию
- `global` — работает с более глобальным знанием по корпусу
- `hybrid` — комбинирует локальный и глобальный retrieval
- `naive` — более простой базовый поиск без полной graph-логики
- `mix` — сочетает knowledge graph и vector retrieval; в upstream именно этот путь особенно выигрывает от reranker

Для первой установки имеет смысл сравнить хотя бы `hybrid` и `mix`. Если документы сложные и богатые на сущности и связи, разница может быть заметной.

## Reranker: когда он нужен

Reranker не обязателен для старта, но он особенно полезен если:

- у вас сложные mixed queries
- документы длинные и тематически пересекаются
- вы хотите уменьшить шум в retrieved context
- `mix`-режим становится основным режимом работы

Если базовая установка уже отвечает нормально, а потом вы хотите поднять качество retrieval без полной смены архитектуры, именно reranker часто оказывается следующим логичным шагом.

## Storage backends и рабочие директории

LightRAG использует четыре класса хранилищ:

- KV storage
- vector storage
- graph storage
- doc status storage

Для тестовой установки upstream сам рекомендует простые дефолты:

- `JsonKVStorage`
- `JsonDocStatusStorage`
- `NetworkXStorage`
- `NanoVectorDBStorage`

По мере роста нагрузки можно рассматривать более тяжелые backends, например PostgreSQL, Neo4j, MongoDB, Redis, Milvus, Qdrant, Memgraph или OpenSearch, но это уже отдельный операционный этап, а не обязательное условие для первого BYOK-запуска.

По умолчанию:

- `WORKING_DIR` — `./rag_storage`
- `INPUT_DIR` — `./inputs`

## Как защитить сам LightRAG Server

Это важный раздел, потому что здесь легко перепутать ключ провайдера модели и ключ сервера.

### 1. API key сервера

Чтобы защитить API LightRAG, используйте:

```env
LIGHTRAG_API_KEY=your-secure-api-key-here
```

Этот ключ потом передается в запросах через заголовок:

```text
X-API-Key
```

### 2. Web UI login

Если вы хотите закрыть Web UI, upstream рекомендует настроить и аккаунты, и JWT-параметры:

```env
AUTH_ACCOUNTS='admin:{bcrypt}<hash>'
TOKEN_SECRET='your-secret'
TOKEN_EXPIRE_HOURS=4
```

В официальной документации отдельно отмечено, что одного только `LIGHTRAG_API_KEY` недостаточно, если Guest-доступ остается открытым через Web UI и часть API.

Итоговая практическая схема:

- `LLM_BINDING_API_KEY` защищает доступ LightRAG к LeanToken
- `LIGHTRAG_API_KEY` защищает ваш LightRAG API
- `AUTH_ACCOUNTS` и `TOKEN_SECRET` защищают Web UI и учетные записи

## Production-замечания

Для production LightRAG полезно учесть несколько вещей заранее.

### Reverse proxy и большие файлы

В официальном API README отдельно описан кейс с Nginx:

- для `/documents/upload` нужно увеличить `client_max_body_size`
- для streaming endpoints лучше отключить gzip
- таймауты для upload и long-running generation нужно увеличивать отдельно

Это особенно важно, если вы загружаете PDF, презентации или длинные документы.

### Несколько инстансов

LightRAG специально допускает запуск нескольких инстансов с разными `.env` из разных startup directories. Для логической изоляции данных можно использовать `WORKSPACE`.

### Docker и localhost

Если LightRAG сам запущен в Docker, upstream напоминает, что для локальных бэкендов вместо `localhost` часто нужен `host.docker.internal`.

### Русскоязычный корпус

Если документы и ответы у вас в основном русскоязычные, задайте `SUMMARY_LANGUAGE=Russian`, чтобы summary и служебные операции не тяготели к английскому по умолчанию.

## Частые ошибки

Ниже самые типичные проблемы при первой настройке LightRAG.

### Сервер стартует, но индексация падает

Обычно это значит одно из трех:

- LLM отвечает, но embeddings не настроены
- `EMBEDDING_DIM` не совпадает с реальной моделью
- endpoint embeddings не совместим с выбранным binding

### После смены embedding-модели все ломается

Это ожидаемо. LightRAG прямо предупреждает, что при смене embedding-модели старый индекс и vector-related storage нужно очищать и строить заново.

### Плохое качество индексации

Частая причина — слишком слабая LLM или reasoning-модель на этапе extraction. LightRAG рекомендует сильную non-reasoning модель для document indexing.

### API сервера вроде закрыт, но доступ все равно остается

Проверьте, не настроили ли вы только `LIGHTRAG_API_KEY` без `AUTH_ACCOUNTS`. Upstream прямо отмечает, что для реальной защиты лучше настраивать оба уровня.

### Upload падает с `413 Request Entity Too Large`

Если LightRAG стоит за Nginx или другим reverse proxy, увеличьте лимит размера запроса на уровне прокси, а не только внутри `.env`.

## Что проверить после настройки

- `LLM_BINDING_HOST` указывает на `https://api.leantoken.tech/v1`
- `LLM_BINDING_API_KEY` заполнен корректным LeanToken key
- `LLM_MODEL` соответствует реальной модели из вашего LeanToken-аккаунта
- embedding backend отвечает и использует корректные `EMBEDDING_MODEL` и `EMBEDDING_DIM`
- сервер стартует на `localhost:9621` без ошибок
- документы индексируются полностью
- запросы в `hybrid` и `mix` возвращают осмысленный ответ
- после смены embedding-модели вы выполняете полную переиндексацию
- `LIGHTRAG_API_KEY`, `AUTH_ACCOUNTS` и `TOKEN_SECRET` настроены, если инстанс доступен не только локально
- ключи не попали в git, docker logs, shared notes или публичные скриншоты

## Итоговая схема

Если свести настройку LightRAG через LeanToken к короткой формуле, она выглядит так:

1. взять LeanToken API key и endpoint
2. настроить LeanToken как `LLM_BINDING=openai`
3. отдельно настроить embeddings
4. при необходимости отдельно добавить reranker
5. запустить `lightrag-server`
6. загрузить маленький тестовый корпус
7. проверить индексацию, `hybrid` и `mix`
8. только потом усложнять storage, auth и production deployment

Для LightRAG это намного надежнее, чем пытаться стартовать сразу с максимальной конфигурацией. Сначала добейтесь стабильной связки `LLM + embeddings + indexing`, и уже потом улучшайте retrieval, хранение и безопасность.
