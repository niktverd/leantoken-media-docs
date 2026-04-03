# OpenClaw

![Pagina principal guardada de OpenClaw con el logotipo del producto y su eslogan](../../images/openclaw/openclaw.png)

## Descripcion del servicio

OpenClaw es un asistente personal de IA que ejecutas en tus propios dispositivos. En los materiales guardados, el servicio se describe como un asistente self-hosted con Gateway como control plane: a traves de el, OpenClaw conecta modelos, canales, tools, skills, sesiones y WebChat.

OpenClaw puede funcionar en los canales que ya usas, incluidos WhatsApp, Telegram, Slack, Discord, Google Chat, Signal, iMessage, BlueBubbles, Microsoft Teams, Matrix, WeChat, WebChat y otros. Los materiales guardados tambien mencionan flujos de voz en macOS, iOS y Android, live Canvas, browser control, skills y multi-agent routing.

La ruta de configuracion recomendada es la misma en los materiales guardados de OpenClaw: primero instala la CLI, luego ejecuta `openclaw onboard`, y dentro del onboarding configura Gateway, workspace, acceso al modelo, canales y skills.

## Que necesitas

- se recomienda Node.js 24; Node.js 22.16+ esta soportado
- macOS o Linux; los materiales de OpenClaw recomiendan WSL2 para Windows
- una cuenta en `https://leantoken.tech`
- una API key de LeanToken y, si hace falta, una base URL compatible con OpenAI
- una maquina donde se ejecutara Gateway: un ordenador local o un host Linux remoto

## Casos de uso con IA

Segun los materiales guardados, OpenClaw es especialmente util en estos escenarios:

- un asistente personal de IA en apps de mensajeria y WebChat que responde en tus canales habituales en lugar de una interfaz web separada
- ayuda para desarrollo: comprension de codigo, ejecucion de pruebas, investigacion de errores, trabajo con GitHub y preparacion de pull requests
- tareas de DevOps y operaciones: inspeccion de logs, monitoreo de servicios, alertas y tareas repetidas de runbooks
- flujos de investigacion: busqueda, sintesis, trabajo con notas y tareas orientadas a conocimiento
- flujos de voz y acciones locales del dispositivo mediante companion apps y nodes
- automatizacion del navegador, flujos de Canvas y otras tareas guiadas por tools

Si usas BYOK mediante `leantoken.tech`, un solo gateway y un solo conjunto de credenciales se pueden reutilizar en todos esos escenarios en lugar de depender solo de una suscripcion OAuth incluida del proveedor.

## BYOK en leantoken.tech

En el contexto de OpenClaw, BYOK significa conectar tu propia API key para el modelo o el gateway en lugar de depender solo de la suscripcion OAuth del proveedor. En el material guardado de GitHub, OpenClaw separa expresamente dos rutas de autenticacion:

- subscriptions mediante OAuth
- API keys

Los mismos materiales tambien mencionan:

- soporte para muchos providers y models
- rotacion de perfiles de autenticacion para OAuth y API keys
- model failover
- la recomendacion de usar el modelo actual mas fuerte al que tengas acceso

Para `leantoken.tech`, esto es util cuando quieres:

- mantener el acceso al modelo separado de tu suscripcion OAuth personal
- gestionar la key y el endpoint de forma centralizada
- cambiar de modelo sin modificar el patron general de integracion
- usar un solo gateway para OpenClaw, canales, skills y flujos de automatizacion

Los materiales guardados de OpenClaw recomiendan construir esta configuracion alrededor de `openclaw onboard`, por eso el flujo practico de abajo se basa en onboarding y no en ediciones manuales no documentadas de archivos internos.

## Como obtener una API key en leantoken.tech

Antes de configurar OpenClaw, consigue tu key y endpoint en LeanToken.

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
8. Mantén privados tanto el endpoint como la key. Si la key se expone, rotala y sustituyela en OpenClaw.

## Como agregar BYOK

El flujo siguiente es el que mejor encaja con los materiales guardados de OpenClaw.

### 1. Instala OpenClaw

```bash
npm install -g openclaw@latest
```

Si quieres instalar el daemon de inmediato y seguir el flujo de onboarding recomendado, usa:

```bash
openclaw onboard --install-daemon
```

En los materiales fuente, esta se describe como la ruta de configuracion preferida: onboarding te guia paso a paso por Gateway, workspace, canales y skills.

### 2. Elige la ruta de API key durante model/auth setup

Durante el onboarding, llega a la etapa de acceso al modelo. Si quieres BYOK mediante `leantoken.tech`, usa la ruta de API key en lugar de una suscripcion OAuth.

En la practica, eso significa:

1. Prepara tu API key de LeanToken por adelantado.
2. Si el asistente de configuracion pide un endpoint o base URL para un proveedor compatible con OpenAI, usa `https://api.leantoken.tech/v1`.
3. Pega tu API key de LeanToken en el paso de autenticacion del proveedor.
4. Elige un modelo disponible a traves de tu endpoint de LeanToken.

Los materiales guardados de OpenClaw no muestran los nombres exactos de todas las pantallas de onboarding en cada version, asi que la redaccion en el asistente puede variar un poco. Pero la logica general en los materiales fuente es estable: primero autenticacion, luego seleccion del modelo, despues canales y skills.

### 3. Termina la configuracion de Gateway y de los canales

Despues de model/auth setup, termina el onboarding:

- instala el daemon de Gateway si todavia no esta instalado
- configura el workspace
- conecta los canales que necesites, como Telegram, Slack, Discord, WhatsApp o WebChat
- agrega solo las skills que realmente piensas usar

Los materiales guardados de OpenClaw indican expresamente que Gateway es el control plane, mientras que el asistente funciona a traves de los canales, apps y tools conectados.

### 4. Verifica que OpenClaw responde mediante tu BYOK

Para la primera comprobacion, usa el dashboard y el Gateway local.

Los materiales guardados mencionan el dashboard local en:

```text
http://localhost:18789/
```

Despues del onboarding, verifica:

1. que Gateway este en ejecucion
2. que el acceso al modelo funcione sin error de autenticacion
3. que el asistente responda en la CLI, en WebChat o en un canal conectado

Para una comprobacion manual desde la CLI, puedes usar:

```bash
openclaw agent --message "Haz una autoverificacion corta de la configuracion y confirma que el modelo responde" --thinking high
```

Si necesitas una ejecucion de depuracion en foreground, los materiales guardados tambien incluyen:

```bash
openclaw gateway --port 18789 --verbose
```

## Escenarios recomendados de BYOK

BYOK mediante LeanToken es especialmente util para OpenClaw cuando:

- un solo asistente atiende varios canales y el acceso al modelo esta centralizado detras de una unica key y endpoint
- quieres controlar la facturacion en lugar de depender solo de una suscripcion OAuth
- necesitas cambiar de modelo rapidamente para programacion, investigacion, resumenes o automatizacion en segundo plano
- quieres conservar el flujo general de Gateway en OpenClaw mientras usas tu propia ruta de acceso a modelos

## Recomendaciones practicas

Los materiales guardados de OpenClaw remarcan por separado la seguridad y la higiene operativa. Eso importa todavia mas en una configuracion BYOK.

- No expongas Gateway directamente a internet publico. Los materiales recomiendan mantenerlo en loopback y usar Tailscale o tuneles SSH para acceso remoto.
- No guardes tu API key en un repositorio ni la pegues en configuraciones publicas. Guardala como secreto y rotala si se filtra.
- Trata las skills como codigo con acceso a tus datos y tools. Instala solo las skills en las que confies.
- Usa un aislamiento mas estricto para escenarios de grupos y canales si el asistente tiene acceso al shell o a tools de automatizacion.
- Ejecuta `openclaw doctor` despues de la configuracion si quieres una manera rapida de detectar configuraciones rotas o arriesgadas.

## Que comprobar despues de la configuracion

- OpenClaw arranca sin errores en tu version de Node.js
- Gateway esta disponible en local y abre el dashboard en `localhost:18789`
- el modelo seleccionado realmente responde mediante tu key de LeanToken
- el asistente es visible en el canal que conectaste
- la key y el endpoint se guardan como secretos y no terminaron en git, notas compartidas o el historial del shell
