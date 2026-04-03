# MiroFish

![Logotipo de MiroFish](../../images/mirofish/mirofish.png)

## Sobre el Servicio

MiroFish es un motor open-source de predicción con IA multiagente. Según los materiales guardados, el proyecto construye un modelo digital de la sociedad a partir de datos de entrada como noticias, borradores de leyes, informes analíticos o señales financieras, y luego ejecuta dentro de ese modelo una simulación del comportamiento de miles de agentes autónomos.

Cada agente recibe un rol, lógica de comportamiento, memoria a largo plazo y relaciones con otras entidades. Como resultado, MiroFish no es solo una herramienta de IA de un solo prompt. Ejecuta un flujo completo: construye un grafo de conocimiento, genera el entorno, simula interacciones sociales, produce un informe y permite hablar con agentes y con ReportAgent después de que termina la simulación.

Según las fuentes guardadas, el servicio combina:

- un frontend construido con Vue.js 3, Vite, D3.js y Tailwind CSS
- un backend construido con Python 3.11+ y Flask
- el motor de simulación OASIS de CAMEL-AI
- memoria y GraphRAG mediante Zep Cloud
- orquestación LLM compatible con OpenAI, que puede funcionar con BYOK a través de `leantoken.tech`

![Captura guardada de la interfaz de MiroFish tomada de los materiales del proyecto](../../images/mirofish/mirofish-overview.webp)

Desde la perspectiva del usuario final, MiroFish se parece más a una sandbox digital de modelado que a un chat de IA tradicional: proporcionas materiales de origen y un objetivo de predicción, y luego observas cómo la sociedad digital reacciona al evento y qué resultados pueden seguir.

## Cómo Funciona MiroFish

En la documentación guardada, MiroFish se describe como un flujo de trabajo de cinco etapas:

1. `Graph Building` — el servicio extrae entidades y relaciones de los materiales de origen, construye GraphRAG e inyecta memoria colectiva.
2. `Environment Setup` — genera personas de agentes, roles y parámetros de plataformas simuladas.
3. `Simulation Execution` — ejecuta interacciones paralelas entre agentes y actualiza su memoria entre rondas.
4. `Report Generation` — ReportAgent analiza los logs de la simulación y produce un informe predictivo estructurado.
5. `Deep Interaction` — después de la ejecución, puedes hablar directamente con agentes concretos o con ReportAgent para explorar el escenario con más detalle.

Según los materiales guardados, la interfaz de MiroFish está construida como un asistente paso a paso. El frontend normalmente se ejecuta en `http://localhost:3000`, y la API backend en `http://localhost:5001`.

## Casos de Uso con IA

Basándose en el README, DeepWiki y el artículo guardado, MiroFish es especialmente útil en estos escenarios:

- modelar la reacción pública a noticias, iniciativas políticas, cambios regulatorios y eventos informativos de crisis
- probar estrategias de PR y comunicación antes de lanzarlas en el mundo real, cuando es importante ver de antemano coaliciones, conflictos y puntos de tensión
- trabajar con señales financieras y de mercado cuando necesitas algo más que un resumen corto y quieres simular el comportamiento posterior de los participantes
- explorar plataformas sociales y debates mediante un modelo en capas de agentes en lugar de una única respuesta genérica
- escenarios creativos y narrativos: líneas argumentales alternativas, continuaciones e hipótesis del tipo "qué pasaría si"

Importante: en los materiales guardados, MiroFish se presenta como un prediction engine y una digital sandbox, pero esos mismos materiales también indican claramente que se trata de una versión temprana sin backtesting documentado. Conviene tratarlo más como una herramienta de análisis de escenarios y rehearsal que como una garantía de predicción exacta del mundo real.

## Qué Necesitas

Para ejecutar MiroFish con BYOK según los materiales guardados, necesitas:

- Node.js 18+
- Python 3.11+; el README guardado también hace referencia al rango 3.11-3.12
- `uv` para las dependencias de Python
- `npm`
- una cuenta en `https://leantoken.tech`
- una API key de LeanToken
- un endpoint compatible con OpenAI de LeanToken si conectas MiroFish mediante un proveedor externo
- un identificador de modelo disponible en tu endpoint
- un `ZEP_API_KEY` independiente para Zep Cloud, porque la memoria y GraphRAG se gestionan mediante un servicio aparte dentro de MiroFish

Si prefieres una instalación basada en contenedores, los materiales guardados también documentan una ruta con `docker compose`.

## BYOK en leantoken.tech

Para MiroFish, BYOK no es un campo de la interfaz dentro de la aplicación. Es configuración del proveedor LLM en el archivo `.env` raíz del proyecto. Los materiales guardados indican explícitamente que el servicio soporta cualquier API LLM en formato compatible con OpenAI y utiliza estas variables:

- `LLM_API_KEY`
- `LLM_BASE_URL`
- `LLM_MODEL_NAME`

Aquí es donde puedes proporcionar la key, el endpoint y el modelo desde `leantoken.tech`.

En la práctica, eso significa:

- `LLM_API_KEY` — tu API key de LeanToken
- `LLM_BASE_URL` — un endpoint compatible con OpenAI, como `https://api.leantoken.tech/v1`
- `LLM_MODEL_NAME` — un modelo disponible en tu endpoint de LeanToken

Al mismo tiempo, BYOK mediante LeanToken solo cubre la parte LLM de MiroFish. Según los materiales guardados, el servicio sigue requiriendo un `ZEP_API_KEY` independiente, porque GraphRAG y la memoria a largo plazo de los agentes se gestionan mediante Zep Cloud.

BYOK mediante `leantoken.tech` es conveniente para MiroFish cuando quieres:

- conectar un modelo compatible con OpenAI sin quedar atado al proveedor originalmente recomendado
- gestionar la key y el endpoint de forma centralizada
- cambiar de modelo sin reescribir la lógica general de MiroFish
- reutilizar la misma key en otras herramientas compatibles con OpenAI

## Cómo Obtener una API Key en leantoken.tech

Antes de configurar MiroFish, consigue tu key y endpoint en LeanToken.

![Página pública de LeanToken con Sign up, Log in y Dashboard](../../images/leantoken/leantoken.tech.1.landing.png)

1. Abre `https://leantoken.tech`.
2. Haz clic en `Sign up` si todavía no tienes cuenta, en `Log in` si ya tienes una, o en `Dashboard` si ya has iniciado sesión.

![Panel principal de LeanToken con el endpoint API y accesos rápidos](../../images/leantoken/leantoken.tech.2.dashboard.png)

3. Después de iniciar sesión, abre el dashboard o la página de resumen de la API.
4. Copia el endpoint API. En las capturas aparece `https://api.leantoken.tech/v1`.
5. Ve a `Manage keys` o `API Keys`.

![Página API Keys de LeanToken con la acción Create key](../../images/leantoken/leantoken.tech.3.key-managment.png)

6. Haz clic en `Create key`.
7. Copia la API key generada.
8. Prepara también el identificador exacto del modelo que piensas usar en MiroFish mediante `LLM_MODEL_NAME`.
9. Guarda la key y el endpoint como secretos. Si la key se expone, rótala y sustitúyela en `.env`.

## Cómo Añadir BYOK

El flujo siguiente es el que mejor encaja con los materiales guardados de MiroFish.

### 1. Prepara el proyecto y `.env`

El README guardado recomienda este punto de partida:

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
```

Luego abre `.env` y rellena el bloque LLM con valores de LeanToken:

```env
LLM_API_KEY=tu_key_de_leantoken
LLM_BASE_URL=https://api.leantoken.tech/v1
LLM_MODEL_NAME=tu_identificador_de_modelo
ZEP_API_KEY=tu_key_de_zep
```

Lo importante aquí:

- `LLM_API_KEY` y `LLM_BASE_URL` vienen de LeanToken
- `LLM_MODEL_NAME` debe coincidir con un modelo realmente servido por tu endpoint de LeanToken
- `ZEP_API_KEY` debe obtenerse por separado, porque LeanToken no sustituye la capa de memoria de Zep Cloud

La configuración guardada también incluye un bloque opcional `Boost` para acelerar algunas operaciones paralelas. En el fragmento `.env` que enviaste, usa estas variables:

```env
LLM_BOOST_API_KEY=your_api_key_here
LLM_BOOST_BASE_URL=your_base_url_here
LLM_BOOST_MODEL_NAME=your_model_name_here
```

Si quieres usar una configuración de aceleración separada mediante LeanToken, puedes rellenar este bloque con el mismo endpoint y modelo o dedicar un modelo distinto a Boost. Si no necesitas esta aceleración, el fragmento proporcionado recomienda explícitamente dejar estas variables fuera de `.env`.

### 2. Instala las dependencias

Los materiales guardados recomiendan una única ruta de instalación:

```bash
npm run setup:all
```

Si prefieres instalar en pasos separados:

```bash
npm run setup
npm run setup:backend
```

Según la documentación guardada, esto instala las dependencias del root y del frontend mediante `npm`, y las del backend mediante `uv`.

### 3. Inicia MiroFish

Para desarrollo local, usa:

```bash
npm run dev
```

Después del arranque, los materiales guardados sitúan los servicios aquí:

- frontend: `http://localhost:3000`
- backend API: `http://localhost:5001`

Si necesitas iniciar las partes por separado:

```bash
npm run backend
npm run frontend
```

### 4. Recorre el flujo paso a paso en la interfaz

Después del arranque, abre el frontend y recorre las etapas estándar de MiroFish:

1. En `Graph Construction`, sube seed materials como noticias, informes analíticos, policy drafts u otros documentos de origen.
2. En `Environment Setup`, confirma que el entorno y las personas de los agentes se generan sin errores de autenticación LLM.
3. En `Simulation Execution`, inicia la simulación.
4. En `Report`, espera el informe analítico final.
5. En `Interaction`, haz preguntas de seguimiento a agentes concretos o a ReportAgent.

Un detalle importante de MiroFish: no pegas la key de LeanToken en un campo dedicado dentro del asistente. Según los materiales guardados, la configuración LLM se lee desde `.env`, así que la principal señal de un BYOK correcto es que las etapas respaldadas por el modelo funcionan correctamente.

![Captura guardada del mundo demo de MiroFish tomada de los materiales del proyecto](../../images/mirofish/mirofish-demo-world.webp)

### 5. Empieza con una simulación corta y barata

El README guardado indica explícitamente que las simulaciones pueden consumir créditos LLM con rapidez. También da una recomendación práctica: empezar con ejecuciones de menos de 40 rondas.

Para una comprobación inicial de BYOK, es mejor:

- usar un conjunto pequeño de materiales de origen
- ejecutar una simulación corta
- confirmar que graph building y persona generation terminan sin errores de autenticación
- solo entonces aumentar la complejidad del escenario y el número de rondas

## Alternativa: Docker

Los materiales guardados de MiroFish también describen una instalación basada en contenedores:

```bash
cp .env.example .env
docker compose up -d
```

Según DeepWiki y el README:

- la imagen utilizada es `ghcr.io/666ghj/mirofish:latest`
- `docker compose` lee `.env` desde la raíz del proyecto
- se exponen los puertos `3000` y `5001`
- `./backend/uploads` se monta dentro del contenedor para que los archivos subidos y los logs de simulación persistan

Para BYOK, la ruta Docker es la misma que la ruta source deployment: el punto clave es que `.env` ya contenga tu key, endpoint y modelo de LeanToken.

## Notas Prácticas

- Según el artículo guardado, MiroFish es una versión temprana `0.1.0`, y los materiales no documentan backtesting confirmado.
- El artículo también indica que el proyecto todavía no había sido validado por completo en Windows.
- La documentación de MiroFish recomienda una configuración LLM compatible con OpenAI y depende por separado de Zep Cloud, así que una key de LeanToken por sí sola no cubre toda la pila del servicio.
- No guardes `.env` en git ni expongas `LLM_API_KEY`, `ZEP_API_KEY` y el endpoint en logs o notas públicas.
- Si quieres entender el producto antes de hacer una instalación local, los materiales guardados también incluyen una [demo online](https://666ghj.github.io/mirofish-demo/).

## Qué Comprobar Después de la Configuración

- `.env` contiene valores válidos para `LLM_API_KEY`, `LLM_BASE_URL`, `LLM_MODEL_NAME` y `ZEP_API_KEY`
- MiroFish arranca sin errores mediante `npm run dev` o `docker compose up -d`
- el frontend está disponible en `localhost:3000` y el backend en `localhost:5001`
- graph building y environment setup no fallan por autenticación LLM
- la simulación arranca y devuelve un informe
- puedes abrir post-simulation interaction y hacer una pregunta a un agente o a ReportAgent
- las keys de LeanToken y Zep no terminaron en git, en el historial del shell ni en configuraciones compartidas
