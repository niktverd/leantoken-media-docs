# n8n

## Sobre el Servicio

n8n es una plataforma de automatizacion de workflows para conectar servicios por API y ensamblar procesos de varios pasos a partir de nodos. En la documentacion oficial de n8n se presenta como una herramienta de automatizacion con capacidades de IA, donde puedes conectar aplicaciones, transformar datos, lanzar interfaces de chat, aaNadir modelos, memoria, tools y logica de enrutamiento.

Para escenarios de IA esto resulta especialmente util porque n8n no se limita a una sola ventana de chat. El modelo se convierte en un paso dentro de un proceso real:

- recibe un evento entrante
- lee datos de un CRM, correo, hojas de calculo o webhook
- genera una respuesta, un resumen o una estructura
- pasa el resultado al siguiente nodo
- guarda logs, avisa al equipo o llama a una API externa

En el contexto de `leantoken.tech`, n8n encaja muy bien con BYOK porque el stack de IA estandar de n8n ya funciona con una credencial compatible con OpenAI. Eso permite usar LeanToken como endpoint unico en `https://api.leantoken.tech/v1` y reutilizar una sola credencial en varios workflows.

## Quao Necesitas

- acceso a `n8n Cloud` o a una instancia self-hosted de `n8n`
- permisos para crear workflows y credentials
- una cuenta en `https://leantoken.tech`
- una API key de LeanToken
- la base URL compatible con OpenAI `https://api.leantoken.tech/v1`
- un modelo disponible en tu cuenta de LeanToken
- tener claro cual de estos caminos quieres usar:
  - un workflow de chat o agente con `Chat Trigger`, `AI Agent` y `OpenAI Chat Model`
  - una automatizacion normal con el nodo `OpenAI` sin agente

## Casos de Uso con IA

n8n es especialmente util cuando la IA no es un destino aislado, sino un paso dentro de una automatizacion.

Casos practicos:

- clasificar leads, tickets y correos entrantes antes de enviarlos a un CRM o helpdesk
- resumir conversaciones, meeting notes y correos largos y enviar el resultado a Slack, Telegram o Notion
- extraer datos estructurados de formularios, documentos, cuestionarios o payloads de webhook
- montar un flujo de respuesta con IA a traves de `Chat Trigger` y `AI Agent`
- enriquecer registros de clientes, productos o solicitudes antes de escribirlos en una tabla o base de datos
- enrutar solicitudes por intent, por ejemplo soporte, ventas, onboarding o finanzas
- crear asistentes internos de IA que usan tools y APIs externas
- generar borradores de respuesta, FAQ, summaries, metadata y payloads JSON para nodos posteriores

## Por Quao BYOK Es Especialmente Util en n8n

Para n8n, BYOK mediante `leantoken.tech` es util por varias razones practicas:

- la misma key puede reutilizarse en varios workflows
- tu controlas el modelo, el endpoint y el ciclo de vida de la key
- una sola credencial OpenAI-compatible puede reutilizarse tanto en `OpenAI Chat Model` como en el nodo `OpenAI`
- es mas facil separar dev, staging y produccion con credenciales distintas
- si usas `n8n` self-hosted, las llamadas de IA pueden encajar en tus workflows internos actuales sin cambiar la arquitectura general

Otra ventaja es que en n8n las credentials son objetos propios de la plataforma. Esto es mejor que pegar una key manualmente en cada nodo. Segun la documentacion oficial de n8n, las credentials se guardan cifradas en la base de datos y, en planes Enterprise, tambien se pueden obtener desde external secrets vaults.

## CaEmo Obtener una API Key en leantoken.tech

Antes de configurar n8n, consigue tu key y tu endpoint en LeanToken.

![Pagina publica de LeanToken con Sign up, Log in y Dashboard](../../images/leantoken/leantoken.tech.1.landing.png)

1. Abre `https://leantoken.tech`.
2. Haz clic en `Sign up` si todavia no tienes cuenta, en `Log in` si ya la tienes, o en `Dashboard` si ya has iniciado sesion.

![Panel principal de LeanToken con el endpoint API y accesos rapidos](../../images/leantoken/leantoken.tech.2.dashboard.png)

3. Despues de iniciar sesion, abre el dashboard o la pagina de resumen de la API.
4. Copia el endpoint API. En las capturas aparece `https://api.leantoken.tech/v1`.
5. Ve a la gestion de claves mediante `Manage keys` o `API Keys`.

![Pagina API Keys de LeanToken con la accion Create key](../../images/leantoken/leantoken.tech.3.key-managment.png)

6. Haz clic en `Create key`.
7. Copia la API key generada.
8. Vuelve a n8n y prepara la credencial donde vas a pegar:
   - `API key`
   - `Base URL`: `https://api.leantoken.tech/v1`
9. No expongas la key en exports de workflow, issues, capturas o git. Si se filtra, rotala y actualiza la credencial.

## Arquitectura Minima de BYOK en n8n

La configuracion inicial mas visual en n8n se ve asi: un usuario manda un mensaje en el chat, el workflow recibe el evento, `AI Agent` procesa la solicitud y `OpenAI Chat Model` llama a LeanToken mediante una credencial OpenAI-compatible.

![Workflow minimo de IA en n8n: Chat Trigger -> AI Agent -> OpenAI Chat Model](../../images/n8n/01-simple-n8n-setup.png)

La captura muestra la cadena basica:

1. `When chat message received` acepta el mensaje del usuario
2. `AI Agent` controla la logica de la respuesta
3. `OpenAI Chat Model` llama al modelo mediante la credencial
4. el panel inferior muestra input, output y token usage

Este es un buen esqueleto para un smoke test. Despues puedes ampliar el workflow con memoria, tools, retrievers, APIs externas, tablas y otros nodos de n8n.

Hay un detalle importante de la documentacion oficial de n8n:

- `Chat Trigger` se usa para workflows de IA e interfaces de chat
- cada mensaje que entra en `Chat Trigger` crea una ejecucion independiente del workflow
- en la documentacion actual, `AI Agent` se describe como `Tools Agent`, y en algunas versiones espera al menos un tool sub-node

Si tu instancia de `n8n` exige un tool sub-node obligatorio, hay dos caminos practicos:

1. aaNadir un tool compatible y seguir usando `AI Agent`
2. usar directamente el nodo `OpenAI` para un flujo simple de prompt-response

## CaEmo Crear una Credencial OpenAI-Compatible para LeanToken

En n8n, la forma mas limpia de empezar es crear una credencial dedicada de tipo `OpenAI` u `OpenAI account` y luego reutilizarla en los nodos que la necesiten.

![Pantalla de credencial OpenAI account en n8n con campos API Key, Organization ID y Base URL](../../images/n8n/02-model-configuration.png)

Flujo tipico:

1. Abre `Credentials` en la interfaz de n8n, o crea la credencial directamente desde `OpenAI Chat Model` o desde el nodo `OpenAI`.
2. Elige el tipo de credencial `OpenAI`.
3. Pega tu key de LeanToken en `API Key`.
4. Si el formulario incluye `Organization ID`, normalmente se deja vacio para LeanToken. Ese campo pertenece al flujo estandar de OpenAI y no es necesario en una configuracion tipica con LeanToken.
5. En `Base URL`, introduce `https://api.leantoken.tech/v1`.
6. Ponle un nombre claro a la credencial, por ejemplo `LeanToken OpenAI`.
7. Guarda la credencial.

Notas practicas:

- si la lista de modelos no carga despues, lo primero que hay que revisar es `Base URL`
- si tienes varios entornos, crea credenciales separadas como `LeanToken Dev` y `LeanToken Prod`
- si trabajas en equipo, revisa quien puede acceder a la credencial y en que proyecto se comparte

## CaEmo Guardar la Key de Forma Segura en n8n

Segun la documentacion oficial de n8n, las credentials se guardan cifradas en la base de datos. Para muchos equipos eso ya es suficiente, pero si usas Enterprise Self-hosted o Enterprise Cloud, puedes ir un paso mas alla y conectar external secrets.

Esto resulta util cuando:

- el mismo secreto se reutiliza en varios entornos
- las keys deben vivir fuera de la base de datos de n8n
- quieres una rotacion centralizada y un control de acceso mas estricto

La documentacion de external secrets de n8n describe proveedores como `1Password`, `AWS Secrets Manager`, `Azure Key Vault`, `GCP Secrets Manager` y `HashiCorp Vault`. Despues de conectar un vault, los secretos se pueden referenciar en una credencial con una expresion como `{{ $secrets.<vault-name>.<secret-name> }}`.

## Opcion 1. Configuracion con AI Agent y OpenAI Chat Model

Este camino encaja mejor cuando estas construyendo un chatbot, un asistente de IA o un workflow donde el modelo forma parte de una logica mas agentica.

### Paso 1. AaNade Chat Trigger

1. Crea un workflow nuevo.
2. AaNade `Chat Trigger`.
3. Mientras configuras el flujo, deja desactivado el acceso publico.
4. Cuando llegue el momento de exponer el chat, elige uno de estos modos:
   - `Hosted Chat` si quieres usar la interfaz de chat incluida en n8n
   - `Embedded Chat` si vas a usar tu propia interfaz o widget
5. Si el chat debe estar restringido, configura la autenticacion:
   - `None`
   - `Basic Auth`
   - `n8n User Auth`

Esto importa desde el punto de vista operativo porque cada mensaje entrante crea una ejecucion independiente.

### Paso 2. AaNade AI Agent

1. Conecta `AI Agent` a `Chat Trigger`.
2. Usalo como nodo central que recibe la solicitud del usuario y decide como se construye la respuesta.
3. A medida que el workflow crece, puedes conectar:
   - `Chat Model`
   - memoria
   - tools
   - retrievers
   - sub-workflows

Si solo construyes el flujo de respuesta mas simple y sin tools, ten en cuenta las diferencias entre versiones de n8n. En la documentacion oficial actual, `AI Agent` se describe como `Tools Agent`, pero tu captura guardada ya muestra una version minima de esta configuracion, asi que el comportamiento real depende de la version de tu instancia.

### Paso 3. Conecta OpenAI Chat Model

![Nodo OpenAI Chat Model en n8n con una credencial seleccionada y el modelo qwen3-235b](../../images/n8n/03-ai-agent-config.png)

1. AaNade `OpenAI Chat Model` como sub-node debajo de `AI Agent`.
2. En `Credential to connect with`, selecciona la credencial de LeanToken que creaste.
3. En `Model`, elige uno de los modelos disponibles en tu cuenta de LeanToken.
4. Tu captura muestra `qwen3-235b` como ejemplo, pero la lista real depende de los modelos que tu credencial vea a traves del endpoint.

La documentacion oficial de n8n describe dos modos de funcionamiento para este nodo:

- `Chat Completions`
- `Responses API`

En la practica, esto significa:

- si quieres el camino mas compatible, empieza por el flujo basico de chat completion
- si estas creando un workflow nuevo y tu endpoint lo soporta, puedes activar `Use Responses API`
- cuando `Responses API` esta activo, pasan a estar disponibles los built-in tools de OpenAI
- en la documentacion de n8n, esos built-in tools se describen para la combinacion `OpenAI Chat Model` + `AI Agent`

Si aparecen errores de compatibilidad despues de activar `Use Responses API`, vuelve al modo basico sin esa opcion. Para un gateway OpenAI-compatible, esta es una rama normal de diagnostico: algunos endpoints solo soportan el flujo principal de chat y no todo el conjunto de funciones de Responses.

### Paso 4. Configura los Parametros del Modelo

Para la mayoria de configuraciones, lo mejor es empezar rellenando solo la credencial y el modelo, y aaNadir mas opciones solo cuando haga falta.

Ajustes utiles:

- `Maximum Number of Tokens` si quieres limitar la longitud de salida
- `Sampling Temperature` si necesitas respuestas mas predecibles o mas creativas
- `Timeout` si el workflow no debe esperar demasiado
- `Max Retries` si quieres tolerar fallos de red breves

No cargues la primera pasada con demasiadas opciones. Para un smoke test, valida primero `credential + base URL + model`.

### Paso 5. Prueba la Respuesta

1. Ejecuta el workflow manualmente.
2. Envia un mensaje corto en el chat, como `Hola` o `Resume este texto brevemente`.
3. Comprueba que aparece una respuesta del modelo en el output del nodo.
4. Abre los detalles de la ejecucion y confirma que:
   - la solicitud pasa sin error de autorizacion
   - el modelo realmente responde
   - el token usage aparece como esperas

## Opcion 2. Configuracion con el Nodo OpenAI sin Agente

Si no necesitas un workflow con agente y solo quieres llamar al modelo una vez dentro de una automatizacion, el nodo normal `OpenAI` suele ser mas simple.

Este camino es especialmente util para:

- resumir texto de un nodo anterior
- clasificar datos entrantes
- generar estructuras JSON
- extraer campos de un documento
- ejecutar un paso de enrich, rewrite o normalize en mitad del workflow

Segun la documentacion actual de n8n, el nodo `OpenAI` para texto ofrece al menos dos operaciones utiles:

- `Generate a Chat Completion`
- `Generate a Model Response`

La diferencia practica es:

- `Generate a Chat Completion` usa la API de Chat Completions
- `Generate a Model Response` usa la Responses API

Flujo basico:

1. AaNade un nodo `OpenAI`.
2. Selecciona la misma credencial de LeanToken.
3. En `Resource`, elige `Text`.
4. En `Operation`, elige:
   - `Generate a Chat Completion` si quieres el camino mas directo de prompt-response
   - `Generate a Model Response` si quieres usar intencionalmente la ruta de Responses API
5. Elige el modelo.
6. AaNade los mensajes con los roles que necesites:
   - `System` si quieres definir el comportamiento
   - `User` para la solicitud real
7. Si hace falta, activa:
   - `Simplify Output`
   - `Output Content as JSON`

Esto suele ser mas simple que `AI Agent` cuando no necesitas memoria, tool calling ni una superficie de chat como producto.

## Cuaendo Elegir AI Agent y Cuaendo el Nodo OpenAI

Usa `AI Agent` cuando:

- estas construyendo un chatbot o un assistant
- el modelo debe elegir tools
- quieres memoria, tools y una logica mas agentica
- ya tienes una conversacion impulsada por `Chat Trigger`

Usa el nodo `OpenAI` cuando:

- necesitas una sola llamada al modelo dentro de un workflow mas grande
- estas resumiendo, clasificando, reescribiendo o extrayendo datos
- la salida del modelo pasa directamente al siguiente nodo normal
- todavia no quieres complejidad adicional de agente

En la practica, muchos workflows de n8n empiezan con el nodo `OpenAI` y solo pasan a `AI Agent` mas adelante, cuando realmente hacen falta tools y bucles de razonamiento mas avanzados.

## Patrones de Trabajo Recomendados

### 1. Email triage

Flujo:

1. `Gmail Trigger` o `IMAP Email Trigger`
2. nodo `OpenAI` para clasificar el correo
3. `Switch` o `If`
4. enviar el resultado al CRM, Slack, Notion o helpdesk

### 2. Lead enrichment

Flujo:

1. `Webhook` o `Form Trigger`
2. limpiar y normalizar los campos de entrada
3. nodo `OpenAI` para un perfil corto del lead y su clasificacion por intent
4. escribir el resultado en el CRM

### 3. Asistente interno de IA

Flujo:

1. `Chat Trigger`
2. `AI Agent`
3. `OpenAI Chat Model`
4. tools para leer tablas, llamar APIs o lanzar sub-workflows

### 4. Extraccion estructurada de texto

Flujo:

1. `Webhook` recibe texto o un documento
2. el nodo `OpenAI` devuelve JSON
3. `Edit Fields`, `Set`, `Code` o `IF` validan el resultado
4. los datos se envian a una base de datos, hoja de calculo o API downstream

## Quao Comprobar Despuaos de la ConfiguraciaEn

Smoke test minimo:

1. Confirma que la credencial esta guardada con `Base URL` en `https://api.leantoken.tech/v1`.
2. Comprueba que la lista de modelos carga y muestra al menos un modelo disponible para tu cuenta.
3. Ejecuta la solicitud mas corta posible sin parametros extra.
4. Confirma que la respuesta vuelve sin `401`, `403` ni `404`.
5. Revisa el execution output y el token usage.
6. Si usas `Chat Trigger`, prueba el chat primero en modo no publico.
7. Si luego expones el chat publicamente, configura el modo de autenticacion adecuado.

## Problemas Frecuentes

### El modelo no aparece en la lista

Normalmente significa una de estas tres cosas:

- `Base URL` es incorrecta
- la API key no es valida o no tiene acceso
- el modelo objetivo no esta disponible en tu cuenta de LeanToken

### Error de autorizacion

Comprueba que:

- la key de LeanToken sigue siendo valida
- la key no tiene espacios extra
- la credencial realmente usa `https://api.leantoken.tech/v1`

### Errores al activar Responses API

Si el flujo basico de chat funciona pero `Use Responses API` falla, el problema suele ser una diferencia entre las capacidades del endpoint o del modelo y las funciones extra de Responses. En ese caso:

1. desactiva `Use Responses API`
2. verifica el camino simple de chat completion
3. solo despues vuelve a las funciones avanzadas

### AI Agent exige un tool sub-node

Esto sigue la documentacion actual de n8n, que describe `AI Agent` como `Tools Agent`. Si eso ocurre en tu version:

1. aaNade un tool sub-node compatible
2. o mueve la prueba al nodo normal `OpenAI`

### El chat publico se comporta distinto de lo esperado

Comprueba:

- si `Make Chat Publicly Available` esta activo o no
- si el modo es `Hosted Chat` o `Embedded Chat`
- si `Basic Auth`, `n8n User Auth` o la configuracion de `CORS` estan interfiriendo

### Las ejecuciones se consumen demasiado rapido

Esto importa con `Chat Trigger`: cada mensaje del usuario crea una nueva ejecucion del workflow. Si un usuario mantiene una conversacion larga, las ejecuciones se consumen rapido y debes tenerlo en cuenta en tu modelo operativo.

## Limitaciones y Notas Importantes

- Los nombres exactos de los campos y la posicion de los botones pueden variar entre versiones de n8n.
- En la documentacion de n8n, `OpenAI Chat Model` carga la lista de modelos de forma dinamica desde la credencial de cuenta, asi que la lista final depende del endpoint de LeanToken y de los permisos de tu cuenta.
- `Responses API` y los built-in tools solo deberian activarse despues de confirmar que el camino basico ya funciona.
- Si usas chat publico, piensa por separado en autenticacion, CORS y coste por ejecuciones.
- Si el workflow maneja datos sensibles, decide de antemano que campos se pueden enviar al modelo y cuales deben enmascararse o excluirse.

## Enlaces Utiles

- [n8n Docs](https://docs.n8n.io/)
- [OpenAI credentials en n8n](https://docs.n8n.io/integrations/builtin/credentials/openai/)
- [Nodo Chat Trigger](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-langchain.chattrigger/)
- [Nodo AI Agent](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/)
- [Nodo OpenAI Chat Model](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatopenai/)
- [OpenAI Text operations](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-langchain.openai/text-operations/)
- [External secrets en n8n](https://docs.n8n.io/external-secrets/)
- [LeanToken](https://leantoken.tech)
