# SourceCraft Code Assistant (Yandex Code Assistant)

## Sobre el Servicio

SourceCraft Code Assistant es un asistente de IA para desarrolladores disponible dentro de la plataforma SourceCraft y como plugin para IDE. En la pagina oficial actual, se describe como un asistente con agent mode, chat integrado, code review, sugerencias con contexto y soporte para editores populares como VS Code y las IDEs de JetBrains.

Si conoces el nombre anterior, es el mismo producto: en las FAQ de SourceCraft se indica expresamente que `Yandex Code Assistant` paso a formar parte de la plataforma SourceCraft y fue renombrado como `SourceCraft Code Assistant` sin perder funcionalidad.

Para LeanToken, lo importante es que la documentacion actual de SourceCraft ya describe perfiles de modelos y soporte para AI providers externos. Eso hace que BYOK con `https://leantoken.tech` sea una ruta natural y no un apaño.

## Donde SourceCraft Code Assistant es Especialmente Util

- trabajo estilo agente sobre tareas en lenguaje natural directamente en la IDE
- exploracion del repositorio y comprension de la estructura del proyecto mediante el chat integrado
- code review y sugerencias rapidas de IA sin cambiar a otro servicio
- `Next Edit Action` e inline completion mientras editas codigo
- generacion de documentacion, tests y cambios repetitivos usando el contexto del proyecto
- conexion de servidores MCP y uso de diferentes LLM segun el modo de trabajo

## Que Conviene Saber Antes de Configurar BYOK

Antes de conectar LeanToken, conviene entender dos sistemas basicos de SourceCraft Code Assistant.

### Perfiles de Modelos

Segun la documentacion oficial para Visual Studio Code, el asistente usa `configuration profiles`. Un perfil puede guardar:

- un AI provider
- una API key
- un modelo
- temperature y otros ajustes del modelo
- un rate limit
- opciones adicionales especificas del provider

Esto es util para BYOK porque no hace falta reemplazar una configuracion global cada vez. Puedes crear un perfil separado, por ejemplo `LeanToken`.

### Modos de Trabajo

En la documentacion de agentes de SourceCraft, los modos integrados son:

- `Code`
- `Ask`
- `Architect`
- `Debug`
- `Orchestrator`

Cada modo puede recordar su propio perfil y su propio modelo. En la practica, eso significa que LeanToken te permite:

- mantener un modelo mas fuerte para `Code` o `Architect`
- usar un modelo mas rapido o mas barato para `Ask`
- probar perfiles diferentes sin romper la configuracion por defecto

## Que Necesitas

- Visual Studio Code con el plugin SourceCraft Code Assistant instalado
- una cuenta en `https://leantoken.tech`
- una API key de LeanToken
- el endpoint OpenAI-compatible `https://api.leantoken.tech/v1`
- un model ID disponible en tu cuenta de LeanToken

El flujo de abajo esta escrito para VS Code. Es el punto de partida mas fiable para esta pagina porque la documentacion oficial de perfiles de modelos esta publicada ahora mismo para Visual Studio Code, y la captura guardada tambien proviene de esa interfaz.

## BYOK en leantoken.tech

En SourceCraft Code Assistant, Bring Your Own Key significa que no dependes solo del perfil integrado con modelos de Yandex. En su lugar, creas tu propio perfil para un provider externo OpenAI-compatible.

Para LeanToken, esto se ve asi:

1. creas tu propia API key en LeanToken
2. creas un perfil separado en Code Assistant
3. eliges `OpenAI Compatible`
4. defines `https://api.leantoken.tech/v1`
5. pegas tu API key de LeanToken
6. eliges el modelo que quieres usar
7. si hace falta, vinculas el perfil a los modos de trabajo que mas utilizas

Esto es util cuando quieres controlar:

- el modelo usado en un flujo concreto
- costes y cuotas
- el cambio entre varios perfiles
- la separacion entre modelos integrados y modelos externos

## Como Obtener una API Key en leantoken.tech

Primero consigue tu key y tu endpoint en LeanToken.

![Pagina publica de LeanToken con Sign up, Log in y Dashboard](../../images/leantoken/leantoken.tech.1.landing.png)

1. Abre `https://leantoken.tech`.
2. Haz clic en `Sign up` si todavia no tienes cuenta, en `Log in` si ya la tienes, o en `Dashboard` si ya has iniciado sesion.

![Vista general del dashboard de LeanToken con el endpoint API y accesos rapidos](../../images/leantoken/leantoken.tech.2.dashboard.png)

3. Despues de iniciar sesion, abre el dashboard o la pagina de resumen de la API.
4. Copia el endpoint API. En las capturas actuales es `https://api.leantoken.tech/v1`.
5. Ve a la gestion de claves mediante `Manage keys` o `API Keys`.

![Pagina API Keys de LeanToken con la accion Create key](../../images/leantoken/leantoken.tech.3.key-managment.png)

6. Haz clic en `Create key`.
7. Copia la API key generada.
8. Vuelve a los ajustes de SourceCraft Code Assistant y pega la key en el campo `OpenAI API Key`.
9. Trata la key como un secreto. Si aparece en git, logs, capturas o historial del shell, rotala.

## Como Conectar LeanToken en SourceCraft Code Assistant

La captura guardada de abajo todavia muestra el titulo antiguo `Yandex Code Assistant Settings`, pero la ruta de configuracion sigue siendo valida: el producto fue renombrado a SourceCraft Code Assistant, mientras que la pantalla de perfiles de modelos sigue siendo adecuada para el flujo BYOK.

![Pantalla de ajustes de SourceCraft Code Assistant con un perfil Custom, el provider OpenAI Compatible, la Base URL de LeanToken y un modelo seleccionado](../../images/yandex-code-assistant/yandex-code-assistant.png)

### Configuracion Paso a Paso

1. Abre el chat de SourceCraft Code Assistant en Visual Studio Code.
2. En la parte superior del chat, abre `Settings`.
3. En la seccion `Models`, haz clic en `+` junto al selector `Profile`.
4. Crea un perfil nuevo. Un nombre practico es `LeanToken`.
5. En `API Provider`, elige `OpenAI Compatible`.
6. En `Base URL`, introduce `https://api.leantoken.tech/v1`.
7. En `OpenAI API Key`, pega tu API key de LeanToken.
8. En `Model`, elige un modelo disponible en tu cuenta de LeanToken.
9. Si todavia no tienes claro el ajuste avanzado, deja los valores por defecto para `temperature`, `rate limit` y otras opciones especificas del provider.
10. Guarda el perfil y vuelve al chat.

La configuracion minima funcional es:

```text
Profile: LeanToken
API Provider: OpenAI Compatible
Base URL: https://api.leantoken.tech/v1
OpenAI API Key: <tu_LeanToken_API_key>
Model: <tu_model_id>
```

Segun la documentacion oficial, las API keys en este flujo se guardan en `VS Code Secret Storage` y no aparecen como texto plano en la interfaz.

## Como Usar Perfiles Junto con los Modos

Si ya usas el agent mode, conviene vincular tu perfil de LeanToken a flujos concretos:

- `Code` para implementacion y cambios de archivos
- `Ask` para explicaciones sin editar el proyecto
- `Architect` para planning y diseno de sistemas
- `Debug` para diagnosticar problemas

Una configuracion practica para empezar es:

- un perfil LeanToken para todos los modos al principio
- despues un perfil mas fuerte para `Code`
- y un perfil mas rapido para `Ask`

## Primer Smoke Test Despues de la Configuracion

Una vez guardado el perfil, no empieces enseguida con una tarea grande. Primero verifica el flujo basico.

1. Envia un prompt sencillo como `Read README and briefly explain how this project starts`.
2. Pide al asistente que muestre archivos relevantes o un plan corto de implementacion.
3. Si ya usas modos, confirma que el perfil correcto se selecciona en `Code` o `Ask`.
4. Despues pasa a una tarea pequena y practica: actualizar texto, generar un test o explicar un modulo.

## Que Comprobar si No Llegan Respuestas

- Asegurate de que `Base URL` sea exactamente `https://api.leantoken.tech/v1`.
- Comprueba que la API key se haya pegado completa y sin espacios extra.
- Verifica que el model ID seleccionado este disponible en tu cuenta de LeanToken.
- Si el perfil se creo hace tiempo, vuelve a abrir settings y refresca la seleccion del modelo.
- Si necesitas reglas mas estrictas de tratamiento de datos, revisa la configuracion actual de SourceCraft relacionada con modelos alojados por Yandex y providers externos.

## Que se Puede Ampliar Despues

Esta pagina ya cubre el flujo basico de BYOK con LeanToken. Si hace falta, la siguiente iteracion puede anadir:

- una configuracion separada especifica para JetBrains
- ejemplos de perfiles para distintos modos
- una seccion dedicada a servidores MCP
- una comparacion entre el perfil integrado de Yandex y un perfil externo de LeanToken
