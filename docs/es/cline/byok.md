# Cline

## Sobre el Servicio

Cline es un AI coding agent open source que funciona directamente dentro del editor o del terminal. Segun la documentacion oficial, Cline puede leer y modificar archivos, ejecutar comandos, usar un navegador, conectar tools externos mediante MCP servers y ayudarte a desarrollar software a traves de un flujo de conversacion normal.

Esto lo hace especialmente util para BYOK, porque Cline no te ata a un provider incluido. Puedes elegir tu propio API provider, configurar tu propio endpoint, pegar tu propia API key y trabajar con el modelo que mejor encaje con tu workflow de desarrollo.

Esta pagina se centra en el camino donde Cline se conecta a `https://leantoken.tech` mediante el provider `OpenAI Compatible`. Es la ruta que mejor coincide con la UI actual de Settings de Cline y con el flujo oficial de configuracion OpenAI-compatible.

## Cuando Cline es Especialmente Util

Cline resulta mas util cuando necesitas algo mas que un simple panel de chat dentro del editor.

- explorar un repositorio desconocido y entender la arquitectura mediante lectura de archivos, busqueda y preguntas de seguimiento
- implementar nuevas features cuando conviene discutir primero un plan y despues pasar a cambios reales de codigo
- depurar bugs mediante una combinacion de comandos de terminal, logs, tests y fixes iterativos
- refactorizar con mayor velocidad y recuperar el estado con seguridad mediante checkpoints
- trabajar con aplicaciones web cuando el browser tool ayuda a validar el comportamiento de la UI
- ampliar el workflow con MCP servers para GitHub, documentacion, bases de datos, sistemas de tickets y tools internos

## Como se Organiza el Trabajo en Cline

La documentacion oficial de Cline describe varios sistemas base que conviene entender antes de elegir modelo.

### Tasks

Todo el trabajo en Cline ocurre dentro de tasks. Una task conserva:

- el prompt original
- toda la conversacion
- los cambios de archivos
- los comandos ejecutados
- las decisiones y el contexto intermedio
- uso, coste y tiempo de ejecucion

Esto es util en trabajo de ingenieria mas largo, porque puedes volver despues, revisar lo que cambio y continuar con el mismo contexto en lugar de empezar desde cero.

### Plan y Act

Uno de los workflows mas utiles de Cline es la separacion entre `Plan` y `Act`.

- `Plan` sirve para explorar la base de codigo, discutir la estrategia, localizar los archivos afectados y perfilar la implementacion sin editar archivos ni ejecutar comandos
- `Act` sirve para los cambios reales de archivos, comandos, tests y ejecucion una vez que el plan ya esta claro

En la practica, esto hace que el workflow sea mas predecible. Para tareas medianas y grandes, normalmente conviene empezar en `Plan` y cambiar a `Act` solo cuando el enfoque ya tenga sentido.

### Checkpoints

Cline crea checkpoints por defecto. Son snapshots internas del estado del proyecto que no ensucian tu historial normal de Git. Si el agente toma una direccion equivocada, puedes restaurar archivos, restaurar la task o restaurar ambas cosas.

Eso importa para BYOK porque te permite trabajar con mas confianza:

- puedes permitir mas autonomia en tareas bien definidas
- puedes probar enfoques distintos sin perder el control del estado del proyecto

### Otros Sistemas que Conviene Conocer Pronto

Ademas de la configuracion del provider, Cline incluye varios sistemas que mejoran mucho el trabajo real de desarrollo:

- `Rules` para convenciones y restricciones persistentes del proyecto
- `Skills` para conocimiento especializado cargado bajo demanda
- `Workflows` para procesos repetibles de varios pasos
- `Hooks` para validacion automatica y mecanismos de seguridad
- `.clineignore` para excluir archivos y directorios ruidosos del contexto
- `Memory Bank` para preservar contexto estructurado del proyecto entre sesiones
- `MCP servers` para agregar tools y resources externos

En otras palabras, Cline se vuelve mucho mas potente cuando lo adaptas a tu base de codigo y no lo tratas como una simple caja de prompts.

## Que Necesitas

Para configurar Cline mediante BYOK normalmente necesitas:

- VS Code, Cursor u otro editor compatible con Cline instalado
- una cuenta en `https://leantoken.tech`
- una API key de LeanToken
- el endpoint OpenAI-compatible `https://api.leantoken.tech/v1`
- un modelo que funcione bien en tareas de coding y se comporte de forma estable en workflows con tool calling
- un repositorio o carpeta de trabajo abierto en el editor

Los pasos de abajo estan escritos sobre todo para la interfaz estilo VS Code que aparece en tu captura.

## BYOK en leantoken.tech

En la documentacion oficial de Cline, BYOK significa usar tus propias claves y tu propio provider en lugar del `Cline Provider` integrado.

Para `leantoken.tech`, eso significa:

1. crear tu propia API key de LeanToken
2. elegir `OpenAI Compatible` en Cline
3. configurar `https://api.leantoken.tech/v1` como `Base URL`
4. pegar tu key de LeanToken
5. seleccionar un model ID disponible a traves de tu endpoint de LeanToken

Esto es util cuando quieres:

- controlar tu propio billing y tus propias claves
- usar un mismo endpoint en varias herramientas de IA y no solo en Cline
- cambiar de modelo sin modificar el workflow general de la IDE
- mantener separada la capa del provider de cualquier suscripcion incluida del editor

La documentacion oficial de Cline tambien indica que las API keys se guardan en el system credential manager y se envian solo al provider seleccionado. Para LeanToken, ese es el camino mas limpio y directo.

## Como Obtener una API Key en leantoken.tech

Antes de configurar Cline, consigue tu key y tu endpoint en LeanToken.

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
8. Mantén en secreto tanto el endpoint como la key. Si la key aparece en logs, capturas, git o notas compartidas, rotala y reemplazala en Cline.

## Como Instalar Cline

Segun la documentacion oficial actual, Cline soporta varios entornos, incluidos VS Code, Cursor, JetBrains IDEs, CLI, Zed, Neovim, VSCodium y Windsurf. Para la captura que has dado y para el camino mas directo con LeanToken, la mejor referencia es la extension estilo VS Code.

El flujo basico de instalacion es:

1. Abre tu editor.
2. Ve a `Extensions`.
3. Busca `Cline`.
4. Instala la extension.
5. Abre Cline desde la activity bar o desde la command palette.

Si el icono no aparece despues de la instalacion:

- permite la ejecucion de la extension si el editor lo solicita
- reinicia el editor
- abre Cline desde la command palette

## Como Configurar LeanToken en Cline

La captura de abajo muestra exactamente la pantalla de Settings de Cline que necesitas para usar LeanToken como provider OpenAI-compatible.

![Pantalla de Settings de Cline con OpenAI Compatible seleccionado, la Base URL de LeanToken, el campo OpenAI Compatible API Key y qwen3-235b como modelo](../../images/cline/cline-openai-compatible-settings.png)

Los campos clave visibles en la captura son:

- `API Provider`
- `Base URL`
- `OpenAI Compatible API Key`
- `Model ID`
- `Custom Headers`
- las secciones `MODEL CONFIGURATION` y `ADVANCED`

### Configuracion Paso a Paso

1. Abre Cline en la barra lateral del editor.
2. Haz clic en el icono de engranaje en la esquina superior derecha del panel de Cline.
3. Abre la seccion `API Configuration`.
4. En `API Provider`, elige `OpenAI Compatible`.
5. En `Base URL`, escribe `https://api.leantoken.tech/v1`.
6. En `OpenAI Compatible API Key`, pega tu API key de LeanToken.
7. En `Model ID`, escribe el modelo que quieres usar. En tu captura, el valor de ejemplo es `qwen3-235b`.
8. Deja `Custom Headers` vacio salvo que tu configuracion de LeanToken requiera headers adicionales.
9. Deja desactivados `Set Azure API version` y `Use Azure Identity Authentication` si estas usando el endpoint OpenAI-compatible normal de LeanToken.
10. Haz clic en `Done` o cierra Settings si la interfaz ya guardo el estado.
11. Vuelve al chat de Cline y envia un mensaje simple para verificar la conexion.

La configuracion minima funcional se ve asi:

```text
API Provider: OpenAI Compatible
Base URL: https://api.leantoken.tech/v1
OpenAI Compatible API Key: <tu_LeanToken_API_key>
Model ID: <tu_model_id>
```

### Que Significan los Campos Adicionales

La documentacion oficial de Cline para providers OpenAI-compatible indica que la pantalla puede mostrar tambien campos adicionales de configuracion del modelo como:

- `Max Output Tokens`
- `Context Window`
- `Image Support`
- `Computer Use`
- `Input Price`
- `Output Price`

No son necesarios para conseguir la primera conexion correcta. Para empezar, lo esencial es un provider correcto, una base URL correcta, una API key valida y un model ID disponible.

Tu captura tambien muestra la opcion `Use different models for Plan and Act modes`. Esto es util si quieres:

- un modelo de razonamiento mas fuerte para `Plan`
- un modelo mas rapido o mas barato para `Act`

Para la primera configuracion, sin embargo, normalmente conviene hacer que un solo modelo funcione de forma estable antes de dividir el workflow entre varios modelos.

## Como Elegir un Model ID para Cline

No existe un unico model ID perfecto para todos los workflows de Cline. Importan mas unas pocas reglas practicas que el nombre del modelo en si.

- Elige un modelo fuerte en tareas de coding, no solo en chat normal.
- En Cline, la fiabilidad dentro de workflows guiados por tools importa tanto como la calidad de la respuesta.
- Si trabajas con repositorios grandes y tareas largas, el context window importa mucho.
- Si usas mucho `Plan` y `Act`, la velocidad y el coste tambien importan en sesiones largas.

Tu captura tambien muestra una advertencia dentro de Cline diciendo que Cline usa prompts complejos y suele funcionar mejor con modelos mas fuertes. En la practica, eso significa:

- empieza con un modelo en el que ya confies para trabajo agentico de desarrollo
- si un modelo responde pero falla en trabajo largo de varios pasos, cambia a una opcion mas fuerte orientada a coding

## Primera Verificacion despues de la Configuracion

Una vez conectado LeanToken, no empieces directamente con una tarea grande. Primero valida el circuito basico.

1. Envia un mensaje simple y confirma que Cline responde a traves del nuevo provider.
2. Pide a Cline que lea algunos archivos del repositorio actual y explique la estructura del proyecto.
3. Usa `Plan` primero si quieres probar el analisis sin modificar archivos.
4. Despues dale una tarea muy pequena en `Act`, como cambiar un texto, anadir una pequena guard clause o actualizar un comentario.
5. Solo despues de eso pasa a trabajo mas grande.

Buenos primeros prompts:

- `Lee package.json y explica como arranca este proyecto`
- `Encuentra el entry point principal y resume el flujo de inicializacion`
- `Crea un plan corto para agregar una validacion simple a este modulo`

## Un Workflow Practico para el Dia a Dia

Despues de la configuracion inicial, un buen workflow de Cline + LeanToken suele verse asi:

1. Abre el repositorio e inicia una task con un objetivo bien acotado.
2. Para trabajo mediano y grande, empieza en `Plan`.
3. Pide a Cline que encuentre los archivos relevantes y proponga un camino de implementacion.
4. Aclara las restricciones: estilo de codigo, tests, reglas de arquitectura y que no debe cambiarse.
5. Cambia a `Act` una vez que el plan sea aceptable.
6. Revisa el resultado y restaura checkpoints si quieres intentar otro enfoque.

En tareas mas grandes tambien ayuda usar:

- `Memory Bank` para contexto persistente del proyecto
- `Rules` para convenciones y restricciones fijas
- `Workflows` para procesos repetibles
- `Hooks` para validacion automatica y guardrails
- `MCP servers` cuando quieras que Cline interactue con sistemas externos

## Como Reducir Coste y Ruido de Contexto

Uno de los problemas mas comunes en Cline no es el modelo, sino el contexto irrelevante. La documentacion oficial recomienda especificamente usar `.clineignore` para que Cline no desperdicie contexto en archivos que no ayudan a resolver la tarea.

En `.clineignore`, normalmente conviene excluir:

- `node_modules/`
- `dist/`
- `build/`
- `.next/`
- `coverage/`
- `.env` y archivos secretos similares
- assets binarios grandes, dumps de datos y archivos generados

Esto ayuda por tres razones:

- reduce el coste de las peticiones
- acelera las respuestas
- disminuye el riesgo de que el contexto util quede desplazado por archivos irrelevantes

En un proyecto grande, un buen `.clineignore` suele tener un efecto practico mayor que cambiar de modelo.

## Seguridad y Control

Cline es lo bastante potente como para que la estrategia de permisos importe mucho.

- No habilites auto-approve agresivo en una base de codigo desconocida.
- No actives `YOLO mode` salvo que estes comodo con una aprobacion completamente automatica.
- Para trabajo serio, empieza aprobando manualmente ediciones y comandos.
- Si mas adelante quieres mas velocidad, habilita auto-approve gradualmente y solo para acciones cuyo riesgo entiendas.
- Mantén checkpoints activos si experimentas mucho con implementaciones.
- Trata los MCP servers y los Hooks con la misma cautela que aplicarias a cualquier ruta de ejecucion externa.

Para LeanToken, combinelo con la higiene normal de claves:

- no guardes la API key en el repositorio
- no la pegues en `.env.example`, README o sistemas de issues
- no la expongas en capturas ni notas compartidas
- rotala de inmediato si se filtro

## Problemas Comunes y Que Revisar

### Cline no Responde despues de la Configuracion

Comprueba si:

- `API Provider = OpenAI Compatible`
- la `Base URL` es correcta
- `/v1` esta incluido donde corresponde
- la API key es valida
- el model ID elegido esta realmente disponible en tu cuenta de LeanToken

### `Invalid API Key` u Otros Errores de Autorizacion

- copia la key otra vez desde LeanToken
- asegurate de haber pegado una API key y no otro token
- si la key pudo filtrarse o la configuracion parece corrupta, crea una key nueva y reemplaza la anterior

### `Model Not Found`

- verifica el model ID exacto en tu cuenta de LeanToken
- asegurate de que el modelo realmente este disponible a traves de tu endpoint
- si tienes dudas, empieza con un modelo que ya sepas que funciona y solo despues experimenta mas

### Las Sesiones Resultan Demasiado Caras o Demasiado Largas

- agrega `.clineignore`
- crea tasks nuevas en lugar de extender una sola conversacion indefinidamente
- usa `Plan` antes de `Act` para que el uso costoso de tools ocurra solo cuando el enfoque ya este claro
- si hace falta, configura modelos distintos para `Plan` y `Act`

### El Modelo Responde pero se Comporta Mal en Trabajo de Ingenieria

Esto es habitual. En Cline no importa solo la calidad de la respuesta, sino tambien el comportamiento agentico. Si el modelo:

- pierde el hilo despues de varios pasos
- no sigue bien un plan
- se comporta de forma debil con comandos y contexto de archivos
- produce ediciones inestables

pasa a un modelo mas fuerte y orientado a coding dentro de LeanToken.

## Donde Cline y LeanToken Encajan Especialmente Bien Juntos

La combinacion de Cline + LeanToken es especialmente util cuando:

- un mismo endpoint OpenAI-compatible se reutiliza en varias herramientas del equipo
- quieres cambiar model IDs de forma centralizada sin tocar el workflow de la IDE
- quieres una separacion clara entre el workflow del editor y la capa de acceso a modelos del provider
- quieres BYOK no solo para Cline, sino para un stack mas amplio de herramientas de desarrollo

En esa configuracion, LeanToken deja de ser solo un sitio donde guardas una key. Pasa a ser una capa compartida de control para el acceso a modelos en todo tu tooling de desarrollo.
