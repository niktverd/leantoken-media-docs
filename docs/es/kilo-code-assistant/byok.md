# Kilo Code

## Sobre el Servicio

Kilo Code es un AI coding assistant open source que te ayuda a escribir codigo, entender bases de codigo, corregir bugs, refactorizar y automatizar trabajo de ingenieria. En la documentacion oficial actual, Kilo se presenta como una herramienta que funciona donde ya trabajan los desarrolladores: en la IDE, en CLI, en el navegador, en apps moviles y en Slack.

Esto importa en la practica por dos motivos:

- Kilo Code no esta limitado a una sola forma integrada de acceder a modelos
- el mismo workflow puede trasladarse entre VS Code, JetBrains y CLI sin cambiar la logica base

Esta pagina se centra en el escenario mas util para LeanToken: conectar `https://leantoken.tech` a Kilo Code como provider `OpenAI Compatible` con tu propia API key y tu propio model ID.

## Cuando Kilo Code es Especialmente Util

Kilo Code no sirve solo como chat con un modelo, sino como herramienta real de desarrollo.

- explorar rapidamente un repositorio desconocido mediante lectura de archivos y explicaciones de arquitectura
- implementar tareas pequenas y medianas directamente desde el chat
- hacer refactors con cuidado, entendiendo primero el codigo y aplicando luego cambios concretos
- depurar mediante una combinacion de diagnosticos, salida de terminal, logs y fixes iterativos
- actualizar documentacion, changelogs y archivos de soporte junto al codigo
- trabajo de ingenieria en varios pasos donde conviene alternar entre planning, coding y debugging

## Como se Organiza el Trabajo en Kilo Code

Para configurar bien BYOK, conviene entender primero algunos sistemas base de Kilo.

### Agents

En la documentacion actual de Kilo, las personas de trabajo principales se llaman `agents`. Son modos especializados con distinto acceso a tools y distinto enfoque.

- `code` es el agent principal para implementar, editar archivos y desarrollar en general
- `ask` sirve para preguntas y explicaciones sin modificar la base de codigo
- `plan` sirve para diseno, planificacion y preparacion de la implementacion
- `debug` sirve para diagnostico e investigacion paso a paso de fallos

En la practica, la regla simple es:

- usa `ask` y `plan` para investigar y decidir
- usa `code` y `debug` para ejecutar trabajo real

### Contexto y Menciones

Kilo puede descubrir archivos relevantes por si solo mediante tools integrados como `read`, `grep` y `glob`, pero tambien puedes dirigirlo manualmente. La documentacion oficial describe menciones con `@` como:

- `@/path/to/file` para un archivo concreto
- `@problems` para errores del panel Problems
- `@terminal` para salida del terminal
- `@git-changes` para cambios sin commit

La extension tambien incorpora automaticamente el contexto del editor activo y de las pestanas abiertas. Esto hace que Kilo sea util cuando quieres pasar rapido de una pregunta a un cambio preciso.

### Aprobaciones y Auto-Approve

Segun el quickstart actual, Kilo Code trabaja de forma iterativa: el agent propone una accion, tu la apruebas o la rechazas, y despues el trabajo continua.

La documentacion tambien indica que muchos tools se aprueban automaticamente por defecto, mientras que la aprobacion es especialmente importante para:

- comandos de shell
- acceso a directorios externos
- lecturas sensibles de archivos

Si necesitas una configuracion mas estricta, `Settings -> Auto-Approve` te permite elegir uno de estos estados para cada tool:

- `Allow`
- `Ask`
- `Deny`

Esto importa para BYOK porque un modelo fuerte a traves de LeanToken puede trabajar con bastante autonomia. Cuanto mas fuerte sea el modelo y mas confianza deposites en el, con mas cuidado debes revisar permisos para `bash`, edicion de archivos y tools externos.

### Settings y Archivos de Configuracion

La documentacion actual de Kilo dice que la extension de VS Code y el CLI usan el mismo sistema base de configuracion. Los cambios hechos en la UI se reflejan en archivos de configuracion.

Las ubicaciones principales de configuracion son:

- configuracion global: `~/.config/kilo/kilo.jsonc`
- configuracion del proyecto: `kilo.jsonc` en la raiz del proyecto
- ruta alternativa del proyecto: `.kilo/kilo.jsonc`

Esto es util para trabajo en equipo y para uso desde CLI, pero tambien crea riesgo: si haces commit de configuraciones en git, no debes dejar secretos alli.

## Por que Kilo Code Encaja Bien con BYOK

Kilo Code puede usarse con el provider integrado de Kilo, pero ese no es el unico camino. La documentacion oficial actual indica explicitamente que el producto soporta docenas de providers externos, incluido `OpenAI Compatible`. Eso es exactamente lo que lo hace adecuado para LeanToken.

## BYOK en leantoken.tech: que Significa en Kilo Code

En Kilo Code, Bring Your Own Key con `leantoken.tech` significa que:

1. creas tu propia API key de LeanToken
2. conectas LeanToken en Kilo como un provider separado
3. eliges `OpenAI Compatible`
4. configuras el endpoint `https://api.leantoken.tech/v1`
5. pegas tu propia key
6. eliges un model ID que encaje con tu workflow

Segun la documentacion oficial del provider OpenAI-compatible, Kilo Code solo necesita cuatro ajustes base para la configuracion inicial:

- `API Provider`
- `Base URL`
- `API Key`
- `Model`

Eso basta para la primera conexion correcta. Campos adicionales como `Max Output Tokens`, `Context Window` o `Image Support` pueden ajustarse mas tarde, cuando la conexion base ya sea estable.

## Que Necesitas Antes de Empezar

Antes de empezar, prepara:

- Kilo Code instalado en VS Code, JetBrains u otro entorno soportado
- una cuenta en `https://leantoken.tech`
- una API key de LeanToken
- el endpoint `https://api.leantoken.tech/v1`
- al menos un model ID que quieras usar mediante LeanToken

El flujo principal de abajo esta escrito para la UI de la extension, porque eso es lo que muestra tu captura.

## Como Obtener una API Key en leantoken.tech

Antes de configurar Kilo Code, consigue tu API key y tu endpoint en LeanToken.

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
8. Trata la key como un secreto. Si aparece en git, logs, historial del shell, exportaciones o capturas, rotala.

![Pantalla actual de Kilo Settings -> Providers con el dialogo Custom provider usado para conectar LeanToken mediante una Base URL OpenAI-compatible](../../images/kilo/kilo.png)

## Como Instalar Kilo Code

La documentacion oficial de Kilo soporta varios entornos: VS Code, VS Code Preview, JetBrains, CLI, Slack y otras IDEs. Para la documentacion de LeanToken, el flujo de VS Code es la referencia mas clara porque coincide con la UI de settings que aparece en tu captura.

### Instalacion en VS Code

1. Abre VS Code.
2. Ve a `Extensions`.
3. Busca `Kilo Code`.
4. Haz clic en `Install`.
5. Confirma que el icono de Kilo Code aparece en la barra lateral despues de la instalacion.

La documentacion oficial tambien incluye esta via de instalacion por linea de comandos:

```bash
code --install-extension kilocode.kilo-code
```

### Que Pasa Despues de la Instalacion

Si quieres usar el provider integrado de Kilo, la extension te pedira iniciar sesion con una cuenta de Kilo. Para LeanToken, eso no es necesario: puedes conectar tu propio provider mediante settings.

## Como Anadir LeanToken como Custom Provider en Kilo Code

Tu captura muestra la ruta mas util para LeanToken: la pestana `Providers` y el dialogo `Custom provider`.

Segun la documentacion oficial, los settings de Kilo se abren mediante el icono de engranaje en el panel de la extension.

### Configuracion Paso a Paso

1. Abre el panel de Kilo Code en la barra lateral del editor.
2. Haz clic en el icono de engranaje para abrir `Settings`.
3. Ve a la pestana `Providers`.
4. Si el provider no aparece enseguida, abre la lista ampliada de providers y elige la ruta para anadir un provider custom u OpenAI-compatible.
5. Abre el dialogo `Custom provider` que aparece en la captura.
6. En `Provider ID`, introduce un identificador estable sin espacios ni mayusculas. Un valor practico es `lean-token`.
7. En `Display name`, introduce un nombre legible como `Lean Token`.
8. En `Base URL`, introduce `https://api.leantoken.tech/v1`.
9. En `API key`, pega tu API key de LeanToken.
10. Confirma la conexion del provider.
11. Cuando el provider este conectado, elige el modelo que quieras en el selector de modelos de Kilo.
12. Envia un prompt pequeno para verificar que el provider funciona.

### Que Significa Cada Campo en el Dialogo `Custom provider`

Conviene entender el proposito de cada campo para que la configuracion no quede fragil.

#### `Provider ID`

Este es el identificador tecnico interno del provider dentro de Kilo. Normalmente debe ser:

- en minusculas
- sin espacios
- formado por letras, numeros, guiones o guiones bajos

Para LeanToken, un valor seguro es:

```text
lean-token
```

#### `Display name`

Esta es la etiqueta visible en la UI. Un valor legible es:

```text
Lean Token
```

#### `Base URL`

Este es el campo clave para la configuracion OpenAI-compatible. Para LeanToken, usa:

```text
https://api.leantoken.tech/v1
```

Kilo tambien documenta soporte para endpoint URLs completas. Eso es util para proxies custom y configuraciones enterprise, pero para una integracion normal con LeanToken basta la base URL estandar.

#### `API key`

Aqui pegas la API key de LeanToken. La captura indica que el campo puede dejarse vacio si la autenticacion se gestiona mediante headers, pero eso no aplica al flujo normal de LeanToken. En el escenario estandar, la API key debe ir rellena directamente aqui.

### Configuracion Minima Funcional

Para la mayoria de casos, esta configuracion es suficiente:

```text
API Provider: OpenAI Compatible
Base URL: https://api.leantoken.tech/v1
API Key: <tu_LeanToken_API_key>
Model: <tu_model_id>
```

## Como Elegir un Model ID para Kilo Code mediante LeanToken

El error mas comun en los AI coding assistants es elegir un modelo que suena bien en chat pero rinde mal en un workflow real de ingenieria con varios pasos. La documentacion de Kilo tiene una idea practica util: en lugar de memorizar una lista fija de ganadores, sigue principios estables de seleccion de modelos.

### Logica Practica de Seleccion

- para tareas complejas de coding, suelen funcionar mejor modelos premium de la clase Claude Sonnet/Opus, GPT-5 o variantes fuertes tipo Gemini
- para desarrollo cotidiano, los modelos mid-tier suelen dar el mejor equilibrio entre velocidad, coste y calidad
- para trabajo sensible al presupuesto, familias modernas como Qwen, DeepSeek y parecidas pueden servir si son estables en tus tareas reales

### Por que Importa el Context Window

La guia de modelos de Kilo indica que el tamano del contexto afecta mucho a la usabilidad:

- `32K-64K` suele bastar para proyectos pequenos y tareas localizadas
- `128K` funciona bien para aplicaciones normales y varios archivos relacionados
- `256K+` resulta util para repositorios grandes y razonamiento amplio entre archivos

Si trabajas en sesiones largas, evita configurar `Max Tokens` demasiado alto sin motivo. La documentacion advierte expresamente que un presupuesto grande de salida reduce el espacio disponible para el historial de conversacion.

### Recomendacion Practica para el Primer Arranque

1. Empieza con un modelo en el que ya confies para tareas de coding.
2. Validalo no solo con preguntas, sino con trabajo real sobre archivos.
3. Si pierde el hilo en tareas largas, cambia a un modelo mas fuerte o a mas contexto.
4. Si la calidad es buena pero el workflow es demasiado lento o caro, prueba una opcion mid-tier mas rapida.

## Como Trabajar en Kilo Despues de Conectar LeanToken

Una vez conectado el provider, no empieces de inmediato con una tarea de medio dia. Primero valida el bucle base.

### Primera Verificacion

1. Abre Kilo Code.
2. Selecciona el provider respaldado por LeanToken y el modelo que quieras.
3. Envia un prompt corto como:

```text
Explica que archivos de este proyecto son responsables del arranque de la aplicacion.
```

4. Luego dale a Kilo una tarea pequena real:

```text
Mira @problems y propone el plan de correccion mas pequeno razonable.
```

5. Despues de eso, prueba el flujo de edicion:

```text
Corrige un problema concreto de @problems y ensename el diff antes de aplicar el cambio.
```

### Que Agent Usar

- `ask` cuando quieres una respuesta sin cambios
- `plan` cuando quieres estrategia o un plan de implementacion
- `code` cuando ya toca editar archivos
- `debug` cuando ya estas trabajando sobre un bug, logs o salida de terminal

Ese workflow dividido suele funcionar mejor que intentar hacerlo todo dentro de un solo modo.

## Funciones Utiles de Kilo que Conviene Conocer Despues de la Configuracion

### 1. Custom Agents

Kilo te permite crear tus propios agents mediante:

- `Settings -> Agent Behaviour -> Agents`
- archivos Markdown en `.kilo/agents/`
- entradas en la configuracion

Esto resulta especialmente util si quieres un:

- `docs-writer`
- `reviewer`
- `test-engineer`
- `security-checker`

Para LeanToken, esto ayuda porque la misma configuracion del provider puede servir a varios roles especializados.

### 2. Configuracion Compartida entre UI y CLI

La documentacion indica expresamente que la extension y el CLI comparten la misma base de configuracion. En la practica, esto significa:

- puedes configurar el provider en la UI
- luego usar esos mismos settings en el CLI
- o hacer el camino inverso

Esto es util para desarrolladores que alternan entre editor y terminal.

### 3. Context Mentions

Las menciones con `@` hacen que Kilo sea mas preciso:

- `@/src/app.ts` para un archivo concreto
- `@terminal` para la salida del ultimo comando
- `@problems` para diagnosticos del editor
- `@git-changes` para cambios sin commit

Si quieres resultados mas precisos de un modelo conectado por LeanToken dentro de Kilo, esta es una de las herramientas mas practicas que puedes usar.

### 4. Autocomplete

Kilo tiene una funcion separada de autocomplete. En la documentacion actual se describe como Fill-in-the-Middle completion impulsado por Codestral.

Eso no es lo mismo que la configuracion del chat o del provider. Por eso, lo mejor es estabilizar primero el workflow principal del agent mediante LeanToken y solo despues evaluar por separado el comportamiento del autocomplete.

### 5. Auto-Approve

Auto-Approve acelera el trabajo, pero Kilo tambien advierte del riesgo. Si estas trabajando:

- en un repositorio de produccion importante
- cerca de secretos
- con comandos de shell
- con ediciones automaticas en varios archivos

no concedas permisos demasiado amplios salvo que realmente quieras hacerlo.

## Seguridad y Operacion

Para Kilo + LeanToken, unas pocas reglas importan de verdad en la practica.

### No Guardes Claves en Git

Kilo usa archivos de configuracion como `kilo.jsonc`, y la documentacion advierte de forma explicita que no debes hacer commit de API keys ni de otros secretos.

### Cuidado con Export e Import

La documentacion de Kilo tambien advierte que los settings exportados contienen perfiles de provider y API keys en plaintext. Trata esos archivos exportados como secretos sensibles.

### Separa Configuraciones de Prueba y Produccion

Si trabajas con varios modelos o con varios equipos, merece la pena separar providers de prueba y providers de trabajo para que los experimentos no afecten al flujo principal.

### Rota la Key Tras una Exposicion

Si la key de LeanToken aparece en:

- el historial de git
- un issue tracker
- una captura
- un archivo de settings exportado enviado al lugar equivocado
- el historial del shell

debe rotarse y reemplazarse en todas las configuraciones de Kilo.

## Problemas Comunes y Como Revisarlos

La pagina oficial de Kilo para providers OpenAI-compatible enumera varios casos de fallo habituales. El troubleshooting de LeanToken sigue el mismo patron.

### `Invalid API Key`

- asegurate de pegar la key actual de LeanToken sin espacios extra
- confirma que la key procede de la pagina `API Keys` que realmente quieres usar

### `Model Not Found`

- verifica el model ID
- confirma que el modelo seleccionado esta disponible mediante tu endpoint de LeanToken

### Errores de Conexion

- vuelve a revisar la `Base URL`
- para la configuracion estandar de LeanToken, usa `https://api.leantoken.tech/v1`
- pasa a una endpoint URL completa solo si tu entorno lo requiere de verdad

### Hay Respuestas pero la Calidad es Mala

- el problema puede ser el modelo elegido y no LeanToken en si
- prueba un modelo mas fuerte orientado a coding
- revisa si el context window actual es demasiado pequeno para la tarea

### El Contexto se Llena Demasiado Rapido

- reduce `Max Tokens`
- usa sesiones mas cortas
- cambia a un modelo con mas contexto
- inicia una task nueva despues de terminar un bloque grande de trabajo

## Un Workflow Practico de Kilo + LeanToken

Este flujo simple suele demostrar que la configuracion es real y util:

1. Conecta LeanToken como custom provider.
2. Elige un modelo para trabajo de coding.
3. En `ask`, pide a Kilo que resuma la estructura actual del proyecto.
4. En `plan`, pide un plan de implementacion.
5. En `code`, permite un cambio pequeno.
6. En `debug`, investiga un problema mediante `@terminal` o `@problems`.
7. Despues evalua si la calidad, velocidad y coste son aceptables con tu model ID elegido.

## Que Verificar Despues de la Configuracion

- Kilo Code responde sin errores de autenticacion
- el modelo seleccionado esta realmente disponible mediante tu key de LeanToken
- las respuestas llegan a traves del custom provider o provider OpenAI-compatible conectado, no del provider integrado de Kilo
- la key no esta expuesta en `kilo.jsonc`, settings exportados, archivos publicos o notas del equipo
- el agent seleccionado y el modelo seleccionado encajan con el tipo de tarea
- `@problems`, `@terminal` y el flujo normal de contexto de archivos se comportan de forma predecible

## Resumen

Kilo Code encaja bien con BYOK mediante LeanToken porque el producto oficial ya tiene una ruta directa y bien documentada mediante el provider `OpenAI Compatible`. Para la configuracion inicial, lo que mas importa es `Base URL`, `API key`, `Model` y permisos de accion bien pensados.

El despliegue mas practico suele ser este:

1. primero conectar LeanToken como custom provider
2. despues elegir un modelo que encaje con tu workflow de coding
3. solo despues ajustar agents, reglas de auto-approve y automatizacion mas avanzada
