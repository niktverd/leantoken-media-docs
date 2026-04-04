# LangChain

## Sobre el Servicio

LangChain es un framework open source para crear aplicaciones y agentes de IA. En la documentacion oficial de LangChain se presenta como un framework con una arquitectura de agentes preconstruida e integraciones para modelos, tools y sistemas externos, de modo que puedas lanzar una aplicacion rapido y seguir teniendo la opcion de cambiar de proveedor mas adelante.

LangChain resulta especialmente util cuando una llamada simple al modelo ya no basta: necesitas tool calling, structured output, APIs externas, flujos de agentes o trazas mediante LangSmith. Los agentes de LangChain se construyen sobre LangGraph, por lo que puedes empezar con una integracion sencilla y ampliar la arquitectura con el tiempo.

Para BYOK, LangChain encaja bien porque ya admite APIs compatibles con OpenAI. Si el acceso al modelo pasa por `https://api.leantoken.tech/v1`, la integracion normalmente se reduce a pasar tu propia API key, la base URL y el model ID que tienes permitido usar.

## Qué Necesitas

- un proyecto en Python o Node.js que ya use LangChain
- una cuenta en `https://leantoken.tech`
- una API key de LeanToken
- el endpoint compatible con OpenAI `https://api.leantoken.tech/v1`
- el modelo `qwen3-235b`
- opcionalmente, una cuenta de LangSmith si quieres trazas y depuracion

## Casos de Uso con IA

LangChain es especialmente util en escenarios como estos:

- un asistente interno de IA que llama a tus propias APIs y tools
- una interfaz de chat o un asistente interno que funciona con un unico model ID estable
- extraccion de datos estructurados de correos, contratos, formularios, fichas de producto u otros textos densos
- flujos con agentes en los que el modelo elige tools, ejecuta pasos y arma la respuesta final
- mover la misma aplicacion entre distintos proveedores mediante las interfaces estandar de LangChain
- trazar, depurar y comparar ejecuciones con LangSmith sin reescribir toda la aplicacion

## BYOK en leantoken.tech

En el contexto de LangChain, BYOK en `leantoken.tech` significa que no dependes de una credencial compartida del proveedor. En su lugar, pasas tu propia key y tu propio endpoint a LangChain.

En la practica, eso te da varias ventajas concretas:

- la misma key se puede reutilizar en desarrollo local, staging y produccion
- el mismo model ID `qwen3-235b` se puede reutilizar en todos tus escenarios de chat
- el mismo patron de integracion funciona tanto en Python como en TypeScript
- mantienes el secreto en `.env`, Docker secrets, variables de CI/CD o un secret manager en la nube
- cambiar de backend en el futuro es mas facil porque LangChain se apoya en interfaces estandar

Sigue existiendo una limitacion importante de la documentacion oficial de LangChain: `ChatOpenAI` apunta a la especificacion oficial de la API de OpenAI. Si un proveedor añade campos de respuesta no estandar, LangChain puede no conservarlos. Para un flujo BYOK OpenAI-compatible estandar con key, model y base URL, esto normalmente no es un problema.

## Cómo Obtener una API Key en leantoken.tech

Antes de configurar LangChain, consigue tu key y endpoint en LeanToken.

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
8. Vuelve a tu proyecto LangChain y guarda la key en `.env` u otro almacen de secretos. Si tu integracion usa la ruta OpenAI-compatible, guarda tambien `https://api.leantoken.tech/v1` como base URL.
9. No hagas commit de la key en git. Si aparece en logs, capturas, tickets o en un repositorio, rotala y actualiza la configuracion de la aplicacion.

## Cómo Agregar BYOK

LangChain no expone una pantalla de configuracion dedicada para esto. En la practica, añades la key a `.env` y luego la pasas de forma explicita al codigo de inicializacion del modelo.

### 1. Prepara `.env`

Usa una configuracion simple como esta:

```bash
LEANTOKEN_API_KEY=lt_your_key_here
LEANTOKEN_BASE_URL=https://api.leantoken.tech/v1
LEANTOKEN_MODEL=qwen3-235b
```

Los nombres de las variables pueden ser distintos en tu proyecto. En esta guia, el model ID queda fijado como `qwen3-235b` porque esta integracion debe usar solo ese modelo. En los ejemplos de codigo de abajo, el modelo tambien aparece hardcodeado de forma explicita.

### 2. Python: conecta el modelo de chat con `ChatOpenAI`

Si quieres la ruta OpenAI-compatible mas directa para chat completions, usa `langchain-openai`.

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
        ("system", "Ayudas a un desarrollador y respondes de forma breve."),
        ("human", "Explica que es LangChain en un solo parrafo."),
    ]
)

print(response.content)
```

Este es el camino mas directo cuando tu aplicacion ya construye chains, tools o agentes sobre un objeto concreto de chat model.

### 3. Python: usa `init_chat_model` para una entrada mas generica

LangChain tambien documenta una ruta mas generica mediante `init_chat_model`. Es util cuando quieres mantenerte mas cerca de las APIs agnosticas al proveedor de LangChain.

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

result = model.invoke("Enumera las fortalezas principales de LangChain para aplicaciones de IA.")
print(result.text)
```

Si esperas cambiar de proveedor mas adelante, esta entrada suele escalar mejor que depender de una sola clase wrapper.

### 4. Embeddings y RAG

Esta pagina no incluye ejemplos de embeddings. Con tu restriccion actual, la configuracion de LangChain debe usar solo `qwen3-235b`, y esta guia documenta unicamente la ruta de integracion para chat.

Si LeanToken expone mas adelante un embedding model dedicado, conviene documentarlo por separado en lugar de mezclarlo con el flujo actual de `qwen3-235b`.

### 5. JavaScript / TypeScript: conecta `ChatOpenAI`

En LangChain JS, una URL OpenAI-compatible personalizada se pasa mediante `configuration.baseURL`.

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
  "Explica brevemente como LangChain ayuda con los flujos de agentes."
);

console.log(response.content);
```

La documentacion oficial de JS tambien indica que algunos proxies o proveedores de terceros no admiten `stream_options`. Si ves ese problema exacto, prueba con `streamUsage: false`.

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

### 6. JavaScript / TypeScript: usa `initChatModel`

Si quieres la ruta de inicializacion mas generica de LangChain, usa `initChatModel`.

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
  "Da una lista breve de razones para usar LangChain."
);

console.log(response.content);
```

Esta ruta funciona bien cuando tu aplicacion se organiza alrededor de interfaces genericas de LangChain y quieres una sustitucion de proveedor mas limpia mas adelante.

### 7. Reutiliza el objeto modelo en la aplicacion

Despues de eso, no necesitas una segunda ruta de autenticacion. El mismo objeto de modelo se puede reutilizar en varias funciones de LangChain:

- en `create_agent(...)`
- en chains basadas en prompt templates
- en flujos con tool calling
- en trazas y evaluaciones de LangSmith

La idea base es simple: configuras BYOK una sola vez en el nivel del model object y luego reutilizas ese objeto en toda la aplicacion.

## LangSmith y Trazas

El material fuente guardado de LangChain tambien menciona las trazas con LangSmith. Esto resulta util cuando quieres inspeccionar prompts, tool calls, latencia y errores de ejecucion despues de pasar a BYOK.

Configuracion minima:

```bash
LANGSMITH_TRACING=true
LANGSMITH_API_KEY=your_langsmith_key
LANGSMITH_PROJECT=langchain-leantoken
```

Despues de eso, LangChain y LangSmith pueden registrar tus llamadas al modelo si tu codigo ya se ejecuta a traves de LangChain y tienes una key valida de LangSmith.

## Recomendaciones Practicas

- Guarda la key de LeanToken en `.env`, en un secret manager o en variables de CI/CD, no en el codigo fuente.
- No sustituyas `qwen3-235b` por un model ID arbitrario si tu integracion esta restringida solo a este modelo.
- Si quieres una arquitectura agnostica al proveedor, prefiere `init_chat_model` o `initChatModel`.
- Si ya usas `ChatOpenAI`, normalmente solo necesitas pasar la key de LeanToken y la base URL de forma explicita.
- Si los errores de JS mencionan streaming usage metadata, prueba `streamUsage: false`.
- Si quieres mejor observabilidad antes de produccion, añade LangSmith cuanto antes.

## Qué Comprobar Después de la Configuración

- la aplicacion ya no llama al endpoint por defecto del proveedor y usa `https://api.leantoken.tech/v1`
- `ChatOpenAI`, `init_chat_model` o `initChatModel` recibe una API key valida y no falla con `401 Unauthorized`
- todos los ejemplos usan `qwen3-235b`
- la key no se filtro en git, logs, tickets o capturas
- si LangSmith esta activado, puedes ver una traza despues de una peticion de prueba

## Patron Final de Configuracion

Para BYOK de LangChain mediante `leantoken.tech`, el flujo practico es:

1. obtener una API key y un endpoint de LeanToken
2. guardarlos en `.env` o en un secret manager
3. pasarlos a `ChatOpenAI`, `init_chat_model` o `initChatModel`, usando `qwen3-235b`
4. reutilizar ese model object en tus flujos de chain, agent y tool calling

Si tu proyecto ya usa abstracciones LangChain compatibles con OpenAI, moverlo a LeanToken normalmente requiere solo cambios minimos en el codigo.
