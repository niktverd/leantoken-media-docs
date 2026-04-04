# LightRAG

## Sobre el Servicio

LightRAG es un framework open source de retrieval-augmented generation que combina busqueda vectorial estandar con extraccion de entidades, extraccion de relaciones y una capa de knowledge graph. En el repositorio oficial, el proyecto se describe como `Simple and Fast Retrieval-Augmented Generation`, y la superficie principal para la mayoria de usuarios es LightRAG Server: un servidor local con Web UI, API e interfaz compatible con Ollama.

En la practica, esto significa que LightRAG no se limita a un flujo basico de "dividir documentos en chunks y ejecutar similarity search". Durante la indexacion construye varias capas de conocimiento al mismo tiempo:

1. chunks para el retrieval RAG estandar
2. entidades y relaciones extraidas del corpus
3. un grafo para graph-aware retrieval
4. storages dedicados para documentos, estados, datos del grafo y vectores

Por eso LightRAG puede responder no solo a preguntas factuales sencillas sobre documentos, sino tambien a consultas mas estructurales en las que importan roles, dependencias, eventos y relaciones entre partes del corpus.

## Como Funciona LightRAG

Un flujo tipico en LightRAG se ve asi:

1. Subes archivos o texto al servidor.
2. LightRAG divide el contenido en chunks.
3. El LLM extrae entidades y relaciones del texto.
4. El modelo de embeddings codifica chunks, entidades y relaciones en vectores.
5. El servicio guarda los resultados en sus storage backends.
6. En la fase de consulta, LightRAG combina vector retrieval, knowledge graph retrieval y, si hace falta, un reranker.

Por eso LightRAG exige mas a la configuracion del modelo que un stack RAG simple. Segun el README oficial:

- el LLM deberia estar al menos en la clase `32B`
- la longitud de contexto deberia ser como minimo `32KB`, idealmente `64KB`
- los reasoning models no se recomiendan para la indexacion de documentos
- los embeddings deben elegirse antes de la primera indexacion y no cambiarse sin reindexar
- un reranker mejora mucho los escenarios de mixed-query

## Casos de Uso con IA

LightRAG es especialmente util en escenarios como estos:

- busqueda interna con IA sobre wikis, runbooks, engineering docs y notas operativas
- RAG sobre documentacion de API y especificaciones tecnicas donde importan las relaciones entre entidades
- asistentes analiticos para research, revision legal, due diligence y knowledge management
- chat sobre tu propia base de conocimiento cuando necesitas mejor retrieval que el de un simple chat con archivos
- knowledge graphs construidos a partir de articulos, PDF, presentaciones, policy documents y documentacion de proyectos
- un pipeline multimodal de RAG si mas adelante amplias la instalacion hacia flujos del estilo RAG-Anything

Dicho de forma simple, LightRAG resulta util cuando una configuracion estandar de "chat con archivos" se queda corta y necesitas que el sistema entienda entidades, relaciones y la estructura global del corpus.

## Que Necesitas

Para ejecutar LightRAG con BYOK normalmente necesitas:

- un despliegue local de LightRAG Server o el codigo fuente de `HKUDS/LightRAG`
- `uv` o `pip` para la parte de Python
- `bun` si compilas la Web UI desde el codigo fuente
- una cuenta en `https://leantoken.tech`
- una API key de LeanToken
- el endpoint compatible con OpenAI `https://api.leantoken.tech/v1`
- un chat model potente disponible a traves de tu endpoint de LeanToken
- un backend de embeddings, porque LightRAG requiere embeddings aparte del LLM
- opcionalmente, un reranker si quieres mejor retrieval en modo `mix`

## Lo Importante Antes de BYOK

En LightRAG, BYOK no es solo una variable de API key. El servicio tiene tres capas de configuracion separadas:

1. LLM
2. Embeddings
3. Reranker

Las dos primeras son obligatorias. Sin embeddings, LightRAG no puede indexar documentos ni recuperar contexto correctamente.

En `.env`, esto se refleja con:

- `LLM_BINDING`, `LLM_MODEL`, `LLM_BINDING_HOST`, `LLM_BINDING_API_KEY`
- `EMBEDDING_BINDING`, `EMBEDDING_MODEL`, `EMBEDDING_DIM`, `EMBEDDING_BINDING_HOST`, `EMBEDDING_BINDING_API_KEY`
- `RERANK_BINDING`, `RERANK_MODEL`, `RERANK_BINDING_HOST`, `RERANK_BINDING_API_KEY`

La conclusion principal para LeanToken es:

- LeanToken encaja bien en la capa LLM mediante una API OpenAI-compatible
- la compatibilidad de embeddings debe validarse aparte
- el reranking puede desactivarse al principio, pero conviene planificarlo para retrieval en produccion

## BYOK en leantoken.tech

En el contexto de LightRAG, BYOK en `leantoken.tech` suele significar que:

1. obtienes tu propia API key de LeanToken
2. configuras `https://api.leantoken.tech/v1` como `LLM_BINDING_HOST`
3. pones la key en `LLM_BINDING_API_KEY`
4. eliges un modelo realmente disponible en tu cuenta de LeanToken

Esto encaja bien con LightRAG porque el proyecto admite oficialmente `openai` y backends OpenAI-compatible para la capa LLM.

Hay una distincion importante que conviene mantener clara: `LLM_BINDING_API_KEY` y `LIGHTRAG_API_KEY` no son lo mismo.

- `LLM_BINDING_API_KEY` permite a LightRAG llamar al proveedor del modelo
- `LIGHTRAG_API_KEY` protege tu propia API de LightRAG Server

No las confundas. La primera sirve para acceso al modelo. La segunda sirve para asegurar tu instancia de LightRAG.

## Como Obtener una API Key en leantoken.tech

Antes de configurar LightRAG, consigue tu key y endpoint en LeanToken.

![Pagina publica de LeanToken con Sign up, Log in y Dashboard](../../images/leantoken/leantoken.tech.1.landing.png)

1. Abre `https://leantoken.tech`.
2. Haz clic en `Sign up` si todavia no tienes cuenta, en `Log in` si ya tienes una, o en `Dashboard` si ya has iniciado sesion.

![Panel principal de LeanToken con el endpoint API y accesos rapidos](../../images/leantoken/leantoken.tech.2.dashboard.png)

3. Despues de iniciar sesion, abre el dashboard o la pagina de resumen de la API.
4. Copia el endpoint API. En las capturas aparece `https://api.leantoken.tech/v1`.
5. Ve a la gestion de claves mediante `Manage keys` o `API Keys`.

![Pagina API Keys de LeanToken con la accion Create key](../../images/leantoken/leantoken.tech.3.key-managment.png)

6. Haz clic en `Create key`.
7. Copia la API key generada.
8. Vuelve al proyecto LightRAG y pega la key en `.env` como `LLM_BINDING_API_KEY`.
9. En el mismo `.env`, configura `LLM_BINDING_HOST=https://api.leantoken.tech/v1`.
10. Guarda la key como secreto. Si aparece en git, logs, tickets, capturas o el historial del shell, rotala.

## Como Elegir Modelos para LightRAG

Antes de editar `.env`, conviene pensar en tres roles distintos.

### 1. LLM para indexacion y respuestas

Segun la guia de LightRAG:

- usa un chat model potente y non-reasoning
- apunta al menos a la clase `32B`
- prefiere una longitud de contexto de `32KB` o mas, idealmente `64KB`

Si tu cuenta de LeanToken expone varios modelos, una opcion generalista y estable sin comportamiento extra de reasoning suele ser la eleccion mas segura para la primera indexacion.

### 2. Modelo de embeddings

Los materiales oficiales mencionan directamente opciones potentes como:

- `BAAI/bge-m3`
- `text-embedding-3-large`

Para LightRAG, el modelo de embeddings es critico por dos motivos:

- afecta directamente a la calidad del retrieval
- su dimension vectorial debe coincidir con `EMBEDDING_DIM`

Si mas adelante cambias el modelo de embeddings o su dimension, normalmente tendras que reconstruir el indice y reindexar los documentos.

### 3. Reranker

El reranking es opcional en LightRAG, pero el README oficial lo recomienda explicitamente para escenarios de mixed-query. En los ejemplos de upstream aparecen:

- `BAAI/bge-reranker-v2-m3`
- rerankers basados en Jina

Para el primer despliegue, empezar con `RERANK_BINDING=null` esta bien. Para la calidad de retrieval en produccion, suele merecer la pena anadirlo despues.

## Patron Recomendado de LeanToken

La configuracion inicial mas segura con LeanToken para LightRAG es:

- usar LeanToken para el LLM
- mantener los embeddings en un backend separado que ya controles
- activar el reranking mas adelante cuando el indice base ya sea estable

Eso es mas seguro que intentar forzar las tres capas a traves de un solo endpoint y luego depurar si el fallo esta en chat, embeddings o reranking.

## Configuracion Base de `.env`

Aqui tienes una plantilla inicial practica.

```env
HOST=0.0.0.0
PORT=9621
SUMMARY_LANGUAGE=Spanish
MAX_ASYNC=4
MAX_PARALLEL_INSERT=2

LLM_BINDING=openai
LLM_BINDING_HOST=https://api.leantoken.tech/v1
LLM_BINDING_API_KEY=lt_your_key_here
LLM_MODEL=<tu_chat_model_de_LeanToken>

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

Puntos importantes:

- `SUMMARY_LANGUAGE=Spanish` es un valor razonable para corpus en espanol
- `LLM_BINDING=openai` funciona con backends OpenAI-compatible, incluido LeanToken
- la combinacion por defecto de `JsonKVStorage`, `JsonDocStatusStorage`, `NetworkXStorage` y `NanoVectorDBStorage` es adecuada para una prueba
- esta es una buena forma de arrancar un proof of concept local, no necesariamente la arquitectura final de produccion

## Variante con Embeddings OpenAI-Compatible

Si ya tienes un backend de embeddings OpenAI-compatible, la configuracion puede verse asi:

```env
EMBEDDING_BINDING=openai
EMBEDDING_BINDING_HOST=<tu_endpoint_de_embeddings>
EMBEDDING_BINDING_API_KEY=<tu_key_de_embeddings>
EMBEDDING_MODEL=<tu_modelo_de_embeddings>
EMBEDDING_DIM=<dimension_exacta_de_embeddings>
EMBEDDING_SEND_DIM=false
```

Para esta variante, revisa tres cosas con cuidado:

1. que el endpoint realmente exponga embeddings en formato OpenAI-compatible
2. que `EMBEDDING_MODEL` coincida con el model ID real
3. que `EMBEDDING_DIM` coincida con la dimension vectorial real del modelo

Si cualquiera de esos puntos es incorrecto, la indexacion y el retrieval se volveran inestables o fallaran por completo.

## Como Instalar LightRAG Server

LightRAG tiene varias rutas de instalacion. Para la mayoria de usuarios, las mas practicas son instalar desde el codigo fuente o usar Docker Compose.

### Opcion 1. Instalar desde el codigo fuente

El workflow oficial se ve asi:

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

Si no usas `make dev`, upstream tambien documenta la ruta equivalente con `uv`:

```bash
uv sync --extra test --extra offline
source .venv/bin/activate
```

Despues de eso, rellena `.env` y arranca el servidor.

### Opcion 2. Docker Compose

Si prefieres una configuracion en contenedores:

```bash
git clone https://github.com/HKUDS/LightRAG.git
cd LightRAG
cp env.example .env
docker compose up
```

La logica BYOK es la misma en Docker: LLM, embeddings y, opcionalmente, el reranker siguen configurandose mediante `.env`.

### Opcion 3. Setup wizard

Las versiones recientes de upstream tambien documentan un setup wizard que genera `.env` y archivos compose relacionados:

```bash
make env-base
make env-storage
make env-server
make env-security-check
```

Esto resulta util si no quieres editar manualmente el largo archivo `env.example`.

## Como Anadir BYOK Paso a Paso

El flujo siguiente es el punto de partida mas practico para LeanToken.

### 1. Prepara `.env`

Copia `env.example`:

```bash
cp env.example .env
```

Despues rellena al menos estos campos:

```env
SUMMARY_LANGUAGE=Spanish

LLM_BINDING=openai
LLM_BINDING_HOST=https://api.leantoken.tech/v1
LLM_BINDING_API_KEY=lt_your_key_here
LLM_MODEL=<tu_chat_model_de_LeanToken>

EMBEDDING_BINDING=<tu_embedding_binding>
EMBEDDING_BINDING_HOST=<tu_endpoint_de_embeddings>
EMBEDDING_BINDING_API_KEY=<tu_key_de_embeddings_si_hace_falta>
EMBEDDING_MODEL=<tu_modelo_de_embeddings>
EMBEDDING_DIM=<tu_dimension_de_embeddings>
```

Si todavia no usas reranker:

```env
RERANK_BINDING=null
```

### 2. Recuerda que `.env` se carga desde el startup directory

El README oficial de la API indica expresamente:

- `.env` debe estar presente en el startup directory
- LightRAG carga `.env` en el entorno del proceso al arrancar
- las variables del sistema tienen prioridad sobre `.env`
- despues de editar `.env`, se recomienda abrir una nueva sesion de terminal

Eso importa si ejecutas varias instancias de LightRAG con configuraciones distintas.

### 3. Arranca el servidor

Para el modo local normal:

```bash
lightrag-server
```

Para el modo multiproceso de produccion en Linux:

```bash
lightrag-gunicorn --workers 4
```

Segun la documentacion oficial, el puerto por defecto es `9621`.

### 4. Abre la Web UI

Despues del arranque, la URL local habitual es:

```text
http://localhost:9621
```

En la Web UI, comprueba:

1. que el servidor abre sin errores de arranque
2. que la subida de documentos funciona
3. que el pipeline de indexacion termina sin errores del LLM ni de embeddings
4. que las consultas funcionan cuando termina la indexacion

### 5. Empieza con un corpus pequeno

Para la primera validacion BYOK, no empieces con decenas de PDF grandes.

Una primera pasada mejor es:

- subir 3 a 5 documentos
- esperar a que la indexacion termine por completo
- probar varios tipos de consulta
- solo entonces aumentar el tamano del corpus y la complejidad del storage

## Como Trabajar con la Web UI y la API

LightRAG Server es util porque cubre tres superficies a la vez:

- Web UI para uso interactivo
- REST API para integracion en aplicaciones
- una interfaz compatible con Ollama para clientes externos de chat con IA

Un ciclo tipico de trabajo se ve asi:

1. subir documentos
2. esperar la indexacion
3. abrir el knowledge graph o la interfaz de consulta
4. comparar varios query modes
5. ajustar reranking, storage y limites de contexto si hace falta

Si quieres integrar LightRAG en otro producto, upstream recomienda en general usar la REST API en lugar de incrustar LightRAG Core directamente, salvo que tengas una necesidad especifica de investigacion o experimentacion.

## Query Modes

LightRAG admite varios query modes. En la practica, los mas utiles de conocer son:

- `local`, que se centra mas en retrieval contextual local
- `global`, que trabaja con conocimiento mas amplio a nivel de corpus
- `hybrid`, que combina retrieval local y global
- `naive`, que es un modo base mas simple
- `mix`, que combina knowledge graph y vector retrieval y es el que mas se beneficia del reranking

Para una instalacion inicial, merece la pena comparar al menos `hybrid` y `mix`. Si tu corpus es rico en entidades y relaciones, la diferencia puede notarse bastante.

## Reranker: Cuando Importa

No necesitas un reranker para empezar, pero se vuelve especialmente util si:

- tus consultas son mixed queries complejas
- tus documentos son largos y semanticamente solapados
- quieres reducir ruido en el contexto recuperado
- `mix` se convierte en tu modo principal de consulta

Si el despliegue base ya funciona y quieres mejorar la calidad del retrieval sin cambiar toda la arquitectura, el reranker suele ser la siguiente capa logica a anadir.

## Storage Backends y Directorios de Trabajo

LightRAG usa cuatro clases de storage:

- KV storage
- vector storage
- graph storage
- document status storage

Para un entorno de prueba, upstream recomienda defaults sencillos:

- `JsonKVStorage`
- `JsonDocStatusStorage`
- `NetworkXStorage`
- `NanoVectorDBStorage`

A medida que crezca la carga, puedes evaluar backends mas pesados como PostgreSQL, Neo4j, MongoDB, Redis, Milvus, Qdrant, Memgraph u OpenSearch, pero eso ya es un paso de escalado operativo, no un requisito para el primer despliegue BYOK.

Por defecto:

- `WORKING_DIR` es `./rag_storage`
- `INPUT_DIR` es `./inputs`

## Como Proteger el Propio LightRAG Server

Esta seccion importa porque es facil confundir la key del proveedor con la key del servidor.

### 1. API key del servidor

Para proteger la API de LightRAG, usa:

```env
LIGHTRAG_API_KEY=your-secure-api-key-here
```

Luego esta key se envia en las peticiones mediante el encabezado:

```text
X-API-Key
```

### 2. Login de la Web UI

Si quieres restringir la Web UI, upstream recomienda configurar tanto cuentas como parametros JWT:

```env
AUTH_ACCOUNTS='admin:{bcrypt}<hash>'
TOKEN_SECRET='your-secret'
TOKEN_EXPIRE_HOURS=4
```

La documentacion oficial indica expresamente que `LIGHTRAG_API_KEY` por si sola no basta si el acceso guest a traves de la Web UI y ciertas rutas de API sigue abierto.

El resumen practico es:

- `LLM_BINDING_API_KEY` da a LightRAG acceso a LeanToken
- `LIGHTRAG_API_KEY` protege tu API de LightRAG
- `AUTH_ACCOUNTS` y `TOKEN_SECRET` protegen el acceso a la Web UI y la autenticacion de usuarios

## Notas para Produccion

Para despliegues en produccion, conviene revisar algunas cosas desde el principio.

### Reverse proxy y archivos grandes

El README oficial de la API incluye una seccion especifica para Nginx:

- `/documents/upload` puede requerir un `client_max_body_size` mayor
- los streaming endpoints funcionan mejor con gzip desactivado
- puede ser necesario ajustar los timeouts para uploads y generaciones largas

Esto importa especialmente si subes PDF, presentaciones o documentos tecnicos grandes.

### Varias instancias

LightRAG admite varias instancias con distintos `.env` desde diferentes startup directories. Para aislamiento logico de datos, tambien puedes usar `WORKSPACE`.

### Docker y localhost

Si LightRAG corre en Docker y necesita llegar a un backend local, upstream indica que puede hacer falta `host.docker.internal` en lugar de `localhost`.

### Corpus no ingles

Si tu corpus principal no esta en ingles, ajusta `SUMMARY_LANGUAGE` en consecuencia para que los summaries y otros procesos internos no se queden por defecto en ingles.

## Problemas Comunes

Estos son los problemas mas tipicos en una primera configuracion de LightRAG.

### El servidor arranca, pero la indexacion falla

Normalmente eso significa una de estas tres cosas:

- el LLM funciona, pero los embeddings no estan configurados
- `EMBEDDING_DIM` no coincide con la dimension real del modelo
- el endpoint de embeddings no es compatible con el binding elegido

### Todo se rompe despues de cambiar el modelo de embeddings

Eso es esperable. LightRAG advierte expresamente que al cambiar el modelo de embeddings hay que limpiar el storage vectorial anterior y reindexar los documentos.

### La calidad de la indexacion es mala

Una causa frecuente es usar un modelo debil o un reasoning model para la extraccion. LightRAG recomienda un modelo potente y non-reasoning para la indexacion de documentos.

### La API del servidor parece protegida, pero sigue habiendo acceso

Comprueba si solo configuraste `LIGHTRAG_API_KEY` sin `AUTH_ACCOUNTS`. Upstream advierte expresamente que para una proteccion real conviene activar ambas capas.

### El upload falla con `413 Request Entity Too Large`

Si LightRAG esta detras de Nginx u otro reverse proxy, aumenta el limite del cuerpo de la peticion en el proxy, no solo dentro de `.env`.

## Que Comprobar Despues de la Configuracion

- `LLM_BINDING_HOST` apunta a `https://api.leantoken.tech/v1`
- `LLM_BINDING_API_KEY` contiene una key valida de LeanToken
- `LLM_MODEL` coincide con un modelo real disponible en tu cuenta de LeanToken
- el backend de embeddings responde correctamente y usa el `EMBEDDING_MODEL` y `EMBEDDING_DIM` adecuados
- el servidor arranca en `localhost:9621` sin errores
- los documentos se indexan por completo
- las consultas `hybrid` y `mix` devuelven respuestas utiles
- despues de cambiar el modelo de embeddings, haces una reindexacion completa
- `LIGHTRAG_API_KEY`, `AUTH_ACCOUNTS` y `TOKEN_SECRET` estan configurados si la instancia es accesible mas alla del uso solo local
- las claves no se filtraron en git, logs de Docker, notas compartidas o capturas

## Patron Final de Configuracion

Si resumes la configuracion de LightRAG con LeanToken en un patron corto, queda asi:

1. consigue tu API key y endpoint de LeanToken
2. configura LeanToken como `LLM_BINDING=openai`
3. configura los embeddings por separado
4. anade un reranker mas adelante si hace falta
5. ejecuta `lightrag-server`
6. sube un corpus pequeno de prueba
7. valida la indexacion, `hybrid` y `mix`
8. solo entonces pasa a escalar storage, endurecer auth y desplegar en produccion

Para LightRAG, esto es mucho mas seguro que intentar empezar desde el primer dia con una configuracion totalmente maximizada. Primero estabiliza la cadena `LLM + embeddings + indexing`, y despues mejora retrieval, storage y seguridad.
