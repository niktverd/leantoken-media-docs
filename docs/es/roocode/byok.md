# Roo Code

## Sobre el Servicio

Roo Code es una suite open source de AI coding para desarrollo asistido por LLM. Oficialmente, el producto existe en dos formatos principales:

- una extension local para VS Code y editores compatibles
- Roo Code Cloud Agents para trabajo autonomo mediante web, Slack, GitHub y otras integraciones

La idea principal de Roo Code es una configuracion model-agnostic. Roo no es un modelo: necesita un inference provider, y tu eliges el provider, el model ID, la API key y la configuracion relacionada. Esto importa para BYOK porque Roo Code no te ata a una sola nube ni a una sola suscripcion incluida.

En la practica, Roo Code hace mucho mas que un panel basico de chat AI en el editor. Segun la documentacion oficial, puede:

- generar codigo a partir de descripciones en lenguaje natural
- explicar la estructura y el comportamiento de una base de codigo existente
- refactorizar y corregir bugs
- actualizar documentacion
- ejecutar comandos
- trabajar con MCP servers
- seguir workflows de varios pasos mediante distintos modos

Esta pagina se centra en la extension local de Roo Code porque ese es el camino mas directo y predecible para BYOK con `leantoken.tech` y un endpoint OpenAI-compatible.

## Cuando Roo Code es Especialmente Util

Roo Code encaja muy bien en estos escenarios:

- explorar un repositorio desconocido y entender rapido la arquitectura, los entry points y las dependencias
- corregir bugs mediante el flujo `@problems` y `@terminal`, junto con lectura de archivos y ejecucion de comandos
- implementar tareas pequenas y medianas a partir de una especificacion sin saltar manualmente entre muchos archivos
- refactorizar y preparar pull requests con un workflow paso a paso
- generar tests, changelogs, release notes y documentacion tecnica
- revisar capturas de UI e imagenes si el modelo seleccionado soporta vision
- trabajo de varios pasos donde conviene separar planning, coding, debugging y orchestration en modos distintos

Dicho de forma simple, Roo Code es mas util cuando el autocomplete ya no basta y hace falta un workflow agentico con acceso a archivos, comandos y contexto real del proyecto.

## Formatos de Trabajo de Roo Code

Antes de configurar BYOK, conviene entender la diferencia entre las dos formas principales del producto.

### 1. Roo Code VS Code Extension

Este es el escenario local. Roo Code se ejecuta dentro del editor, usa el workspace actual, propone tools, pide aprobacion para las acciones y te deja controlar el proceso paso a paso.

Este formato es el mejor para LeanToken BYOK porque configuras directamente:

- el provider
- la base URL
- la API key
- el model ID
- la configuracion del modelo y del perfil

### 2. Roo Code Cloud Agents

Este es el formato cloud para trabajo autonomo de agentes mediante web e integraciones externas. Es util para ejecucion en paralelo, tareas desde Slack y colaboracion basada en GitHub, pero este documento no cubre el flujo de LeanToken para esa ruta.

## Que Necesitas

Para configurar Roo Code mediante BYOK normalmente necesitas:

- VS Code `1.84.0` o superior, o un editor compatible
- la extension Roo Code instalada
- una cuenta en `https://leantoken.tech`
- una API key de LeanToken
- el endpoint OpenAI-compatible `https://api.leantoken.tech/v1`
- un modelo disponible en tu endpoint de LeanToken que soporte native tool calling compatible con OpenAI

El ultimo punto es critico. Roo Code indica de forma explicita en su documentacion oficial que usa solo native tool calling. No existe fallback basado en XML. Un modelo puede responder a prompts normales de chat y aun asi ser una mala opcion para Roo Code si no maneja bien los tools.

## BYOK en leantoken.tech

En el contexto de Roo Code, BYOK en `leantoken.tech` significa que:

1. creas tu propia API key en LeanToken
2. conectas LeanToken como provider `OpenAI Compatible`
3. configuras `https://api.leantoken.tech/v1` como Base URL
4. eliges un modelo disponible a traves de tu endpoint
5. usas Roo Code como un coding agent normal dentro del editor, pero sobre tu propia key y tu propia ruta de acceso a modelos

Esto resulta util por varias razones:

- controlas la key y el endpoint tu mismo
- puedes cambiar el model ID sin alterar el workflow general en la IDE
- la misma key de LeanToken se puede reutilizar en otras integraciones OpenAI-compatible
- no dependes solo de Roo Code Router ni de una suscripcion first-party concreta

Si quieres la configuracion mas simple sin API key, Roo Code recomienda Roo Code Router. Pero si el objetivo es BYOK y el control de tu propio endpoint, para LeanToken hace falta el camino `OpenAI Compatible`.

## Como Obtener una API Key en leantoken.tech

Antes de configurar Roo Code, consigue tu key y tu endpoint en LeanToken.

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
8. Si planeas varios workflows, decide con antelacion si quieres una key compartida o keys separadas para distintas IDEs y proyectos.
9. Manten la key en secreto. Si termina en git, en el historial del shell, en logs, en un issue o en una captura, rotala.

## Como Instalar Roo Code

La documentacion oficial de Roo Code recomienda dos rutas principales de instalacion.

### Mediante VS Code Marketplace

1. Abre VS Code.
2. Ve a `Extensions`.
3. Busca `Roo Code`.
4. Selecciona la extension de `RooVeterinaryInc`.
5. Haz clic en `Install`.
6. Si el editor pide recargar, confirmalo.

Despues de la instalacion, el icono de Roo Code aparece en la activity bar.

### Mediante Open VSX

Este camino es util para editores sin acceso a Marketplace, incluidos algunos forks compatibles con VS Code.

1. Abre la vista de extensiones del editor.
2. Busca `Roo Code`.
3. Instala la extension.
4. Recarga el editor si hace falta.

### Instalacion desde VSIX

Si necesitas la ruta manual:

1. Descarga el archivo `.vsix` desde la pagina oficial de GitHub Releases de Roo Code.
2. En VS Code, abre el menu de extensiones.
3. Elige `Install from VSIX...`.
4. Selecciona el archivo descargado.

## Como Configurar LeanToken en Roo Code

Abajo esta el flujo principal de trabajo para Roo Code con LeanToken.

![Pantalla Providers de Roo Code Settings con el perfil default, OpenAI Compatible, la base URL de LeanToken y un modelo seleccionado](../../images/roocode/roocode.png)

Tu captura ya muestra exactamente la pantalla que hace falta para BYOK:

- la seccion `Providers`
- el perfil `default`
- `API Provider = OpenAI Compatible`
- `Base URL = https://api.leantoken.tech/v1`
- un campo `API Key` rellenado
- el modelo `qwen3-235b` seleccionado

### Configuracion Paso a Paso

1. Abre el panel de Roo Code en VS Code.
2. Haz clic en el icono del engranaje para abrir `Settings`.
3. Ve a la pestana `Providers`.
4. En `Configuration Profile`, deja `default` o crea un perfil nuevo con el boton `+`.
5. En `API Provider`, elige `OpenAI Compatible`.
6. En `Base URL`, escribe `https://api.leantoken.tech/v1`.
7. En `API Key`, pega tu key de LeanToken.
8. En `Model`, selecciona el model ID que quieres usar.
9. Haz clic en `Save`.

El conjunto minimo de parametros de trabajo es este:

```text
API Provider: OpenAI Compatible
Base URL: https://api.leantoken.tech/v1
API Key: <tu_LeanToken_API_key>
Model: <tu_model_id>
```

### Que Significan los Campos Adicionales

Segun la documentacion de Roo Code para providers OpenAI-compatible y segun tu captura, la interfaz puede mostrar configuracion adicional del modelo y metadatos como:

- `Max Output Tokens`
- `Context Window`
- `Image Support`
- `Computer Use`
- `Input Price`
- `Output Price`

Tu captura tambien muestra toggles de servicio y opciones especificas del provider, por ejemplo:

- `Enable streaming`
- `Use legacy OpenAI API format`
- `Include max output tokens`
- interruptores extra para ciertas familias de modelos

Esto significa que no basta con comprobar que el provider conecta; tambien hay que verificar como Roo interpreta las capacidades del model ID concreto.

### Recomendacion Practica para Elegir Modelo

Si esta es tu primera configuracion de LeanToken en Roo Code:

1. empieza con un modelo que ya hayas probado en tareas guiadas por tools
2. no juzgues el modelo solo por la calidad de sus respuestas de chat
3. verifica aparte la lectura de archivos, el uso de tools y la estabilidad del workflow de varios pasos

Si un modelo responde a prompts basicos pero se comporta mal con los tools, el valor real de Roo Code baja mucho.

## Perfiles de Configuracion

Una de las funciones mas utiles de Roo Code para BYOK es `API Configuration Profiles`.

Los perfiles permiten mantener varios conjuntos de configuracion al mismo tiempo. Cada perfil puede tener su propia:

- configuracion del provider
- API keys
- seleccion de modelo
- ajustes tipo temperature
- configuracion de rate limits
- opciones especificas del provider

Esto es especialmente util cuando quieres:

- tener un perfil dedicado a LeanToken
- usar modelos distintos para tipos de trabajo diferentes
- cambiar rapido entre providers sin reescribir los campos manualmente cada vez

### Como Usar los Perfiles en la Practica

Para Roo Code con LeanToken, una estructura practica seria:

- `default` o `leantoken-main` para el workflow principal de coding
- un perfil aparte para un modelo mas barato o mas rapido
- un perfil aparte para tareas de vision si tienes un modelo compatible

La documentacion oficial de Roo Code tambien indica que:

- los perfiles pueden crearse con el boton `+` al lado del selector
- los perfiles pueden cambiarse no solo en `Settings`, sino tambien desde el dropdown en la interfaz de chat
- los perfiles usados con frecuencia pueden fijarse arriba de la lista
- los perfiles pueden renombrarse con el icono del lapiz
- los perfiles pueden borrarse con el icono de la papelera
- las tareas recuerdan el perfil con el que empezaron
- distintos modos pueden asociarse a perfiles diferentes

Esto reduce el riesgo de cambiar de modelo por accidente en mitad de una tarea larga.

### Donde Guarda Roo las Keys

Segun la documentacion de Roo Code, las API keys se guardan en `VSCode Secret Storage` y no deberian aparecer como plain text en la configuracion normal. Eso no elimina la necesidad de una higiene basica de seguridad, pero es mejor que poner la key directamente en archivos del proyecto.

## Modos de Roo Code

Roo Code se apoya en modos de trabajo. Cada modo define comportamiento, acceso a tools y estilo de ejecucion.

### Code

Este es el modo por defecto.

Encaja bien para:

- escribir codigo
- refactorizar
- corregir bugs
- la mayoria del trabajo diario de desarrollo

Este modo tiene acceso completo a `read`, `edit`, `command` y `mcp`.

### Ask

Este modo es para explicacion y exploracion de contexto sin permiso para editar archivos ni ejecutar comandos.

Es util para:

- explicar codigo desconocido
- discutir arquitectura
- validar ideas
- leer un proyecto de forma segura sin cambios accidentales

### Architect

Este modo es para diseno y planning.

Es util para:

- dividir una tarea grande en pasos
- system design
- planificar cambios antes de implementarlos

### Debug

Este modo es para diagnosticar y acotar problemas.

Es util para:

- fallos en tests
- errores de build
- analisis de logs
- localizar comportamiento inestable

### Orchestrator

Este modo sirve para descomponer tareas grandes y delegar subtareas a otros modos.

Es util para:

- tareas largas de feature
- refactors grandes
- cadenas de trabajo donde planning, coding y debugging ocurren en secuencia

Este modo no tiene acceso directo a los tools normales de archivos y comandos. En su lugar, delega subtareas a otros modos.

### Como Cambiar de Modo

Roo Code documenta cuatro caminos principales para cambiar de modo:

1. el dropdown a la izquierda del chat input
2. slash commands como `/code`, `/ask`, `/architect`, `/debug` y `/orchestrator`
3. `Cmd + .` en macOS o `Ctrl + .` en Windows y Linux
4. aceptar una sugerencia de cambio de modo ofrecida por Roo durante el workflow

### Por Que los Modos Importan para BYOK

Roo Code recuerda la ultima seleccion de modelo para cada modo. Eso te permite, por ejemplo:

- mantener un modelo de reasoning mas fuerte para `Architect`
- usar un modelo mas rapido para `Code`
- usar un perfil separado para `Debug`

Para LeanToken, esto es util si quieres controlar coste y calidad a nivel de workflow, en lugar de forzar un solo model ID para todo.

## Como Usa Roo Code los Tools

La documentacion de Roo Code explica muy claramente el modelo de tools: Roo no solo responde con texto. Llama a tools para trabajar con el codigo y con el entorno.

Las categorias principales son:

- `read` para leer y buscar archivos
- `edit` para crear y modificar archivos
- `command` para ejecutar comandos de terminal
- `image` para generacion AI de imagenes
- `mcp` para integraciones mediante Model Context Protocol
- tools de workflow para follow-up questions, finalizar tareas, cambiar de modo y crear subtareas

### Como se Ve el Workflow

Un workflow tipico es asi:

1. describes la tarea en lenguaje natural
2. Roo selecciona el tool necesario
3. muestra los parametros del tool
4. espera tu aprobacion
5. ejecuta la accion
6. continua hasta que la tarea termina

Por defecto, esto es mas seguro que un flujo totalmente autonomo porque controlas:

- que archivos se van a modificar
- que comandos se van a ejecutar
- que acciones propuestas llegan realmente a ejecutarse

## Por Que Native Tool Calling es Critico

Este es uno de los detalles tecnicos mas importantes para Roo Code.

En la documentacion de `OpenAI Compatible`, Roo indica explicitamente:

- Roo usa solo native tool calling
- no hay soporte para fallback basado en XML
- si un modelo no soporta function o tool calling compatible con OpenAI, no puede usarse con Roo Code

De esto sale una conclusion practica para LeanToken:

- la compatibilidad con un chat endpoint OpenAI-compatible por si sola no garantiza que Roo Code vaya a funcionar bien
- tienes que verificar por separado la compatibilidad con native tool calling
- si un modelo produce errores de tool calling, casi siempre es mejor cambiar de model ID que forzar la misma configuracion

## Mentions Utiles en Roo Code

Una de las funciones mas potentes de Roo Code son los mentions contextuales. Permiten traer no solo texto normal, sino tambien archivos, carpetas, imagenes, diagnostics y contexto de git.

### Los Mentions mas Utiles en la Practica

- `@/path/to/file.ts` para agregar un archivo al contexto
- `@/path/to/folder/` para agregar el contenido de archivos dentro de una carpeta
- `@/path/to/image.png` para agregar una imagen si el modelo soporta vision
- `@problems` para incluir diagnostics del panel Problems de VS Code
- `@terminal` para incluir output reciente de terminal
- `@git-changes` para incluir cambios no commiteados
- `@https://example.com` para importar el contenido de una pagina web

### Ejemplos Reales de Prompt

```text
@problems Corrige todos los errores de build y explica la causa raiz.
```

```text
Explica que hace @/src/app.ts e identifica los principales riesgos de arquitectura.
```

```text
Compara @git-changes con la tarea actual y sugiere un buen commit message.
```

```text
Que esta mal en esta pantalla? @/screenshots/bug.png
```

Para Roo Code con LeanToken, esto es especialmente util si quieres respuestas basadas en el repositorio real, en el output del terminal y en los cambios locales actuales, en lugar de consejos genericos.

## Capacidad Adicional: Codebase Indexing

Roo Code puede construir un indice semantico de la base de codigo. Segun la documentacion oficial, este mecanismo:

1. analiza el codigo mediante Tree-sitter
2. crea embeddings para semantic blocks
3. guarda vectores en Qdrant
4. da acceso a Roo a `codebase_search`

Esta es una capacidad aparte, no obligatoria para la configuracion basica de BYOK. Pero resulta util en repositorios grandes donde la lectura archivo por archivo ya no alcanza.

Es importante entender la arquitectura de esta funcion:

- necesitas un embedding provider
- necesitas una vector database, normalmente Qdrant
- si quieres un embedder OpenAI-compatible, necesitas un endpoint compatible y una embedding model compatible

Para LeanToken, esto significa que conviene configurar indexing por separado del camino principal de chat y perfiles. Primero estabiliza el workflow normal de Roo y luego anade embeddings y vector search.

## Un Workflow Inicial Practico

Despues de configurar BYOK, no empieces con una tarea grande de feature. Es mas seguro ejecutar primero una secuencia corta de verificacion.

### Paso 1. Verificar la Lectura de Contexto

Pidele a Roo que lea uno o dos archivos:

```text
Explica la estructura de este proyecto y empieza por @/package.json y @/src/
```

### Paso 2. Verificar Diagnostics

Si el proyecto tiene errores:

```text
@problems Analiza estos errores y sugiere el orden correcto de correccion.
```

### Paso 3. Verificar el Uso de Tools

Dale a Roo una tarea pequena y segura:

```text
Anade un comentario corto en este archivo y muestra exactamente los cambios que planeas hacer.
```

### Paso 4. Verificar Comandos

Si ya estas listo para aprobar un comando:

```text
Ejecuta los tests de este paquete y explica brevemente que fallo.
```

Con esto confirmas rapido:

- si Roo puede ver los archivos
- si el modelo llama bien a los tools
- si la ruta del provider mediante LeanToken es estable

## Seguridad y Operacion

Roo Code se basa en aprobaciones por defecto, y ese es un buen modelo de seguridad.

### Que Conviene Dejar Activado por Defecto

- aprobacion manual para edits
- aprobacion manual para commands
- revision cuidadosa de todas las propuestas de tools

### Que Hay Que Entender Sobre Auto-Approve

Roo Code incluye una advertencia directa en su documentacion: auto-approve acelera el trabajo, pero aumenta de forma importante el riesgo. Para comandos de terminal y cambios de archivos puede provocar perdida de datos, corrupcion de archivos u otros efectos no deseados.

Para una configuracion con LeanToken, la recomendacion practica es simple:

- primero consigue un workflow manual estable
- solo despues activa selective auto-approve
- no actives auto-approve agresivo en proyectos desconocidos ni en configuraciones sensibles de produccion

### Reglas Basicas de Seguridad

- no guardes la key de LeanToken en un repositorio
- no pegues la key en archivos de configuracion del proyecto salvo que sea realmente necesario
- no publiques capturas con una key sin enmascarar
- rota la key de inmediato si se filtra
- usa perfiles separados para distintos tipos de trabajo si quieres aislar accesos

## Problemas Frecuentes y Como Revisarlos

### Roo Code no Conecta con LeanToken

Revisa:

- si `Base URL` es correcto
- si la API key es correcta
- si `https://api.leantoken.tech/v1` es accesible
- si la key se recorto por accidente al copiarla

### Roo Code Ve el Modelo pero Funciona Mal con los Tools

Esto suele ser una razon para comprobar:

- si el model ID concreto soporta native tool calling
- si el provider limita la semantica de tool o function calling
- si el modelo sirve para workflows de coding de varios pasos y no solo para respuestas puntuales de chat

### Error `Model Not Found`

Comprueba el model ID exacto expuesto por tu endpoint de LeanToken. Muchas veces este error no viene de la key, sino del nombre del modelo.

### Errores de Conexion

Revisa:

- si el endpoint tiene una errata
- si el endpoint es accesible desde tu red
- si un proxy o firewall bloquea las peticiones de la IDE

### El Contexto se Llena Demasiado Rapido

Roo Code recomienda por separado ser deliberado con `Max Tokens` y con la ventana de contexto. Si usas budgets de thinking muy altos, consumes mas rapido el espacio del historial de la tarea.

En la practica ayuda:

- mantener limites de thinking mas altos para `Architect` y `Debug`
- usar ajustes mas moderados para `Code`
- dividir el trabajo en varias tareas en lugar de una sesion infinita

## Que Comprobar Despues de la Configuracion

- Roo Code abre sin errores y es visible en la sidebar
- en `Providers` queda guardado un perfil con `OpenAI Compatible`
- `Base URL` esta configurado en `https://api.leantoken.tech/v1`
- el modelo responde a un prompt corto de prueba
- Roo puede leer archivos y proponer tools
- las operaciones guiadas por tools no fallan por incompatibilidad del modelo
- la key se guarda como secreto y no termina en git, logs ni notas

## Recomendacion Final

Para Roo Code con LeanToken, la ruta inicial mas fiable es esta:

1. instala Roo Code en VS Code
2. crea un perfil dedicado para LeanToken
3. elige `OpenAI Compatible`
4. configura `https://api.leantoken.tech/v1`
5. pega la API key de LeanToken
6. selecciona un modelo ya verificado para native tool calling
7. ejecuta un workflow corto de prueba con archivos, diagnostics y comandos

Si esa base funciona de forma estable, despues puedes ampliar la configuracion:

- perfiles separados para distintos modos
- escenarios de vision
- indexing
- selective auto-approve
- custom modes y workflows mas avanzados
