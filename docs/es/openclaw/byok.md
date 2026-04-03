# OpenClaw

![Página principal guardada de OpenClaw con el logotipo del producto y su eslogan](../../images/openclaw/openclaw.png)

## Sobre el Servicio

OpenClaw es un asistente personal de IA que ejecutas en tus propios dispositivos. En los materiales guardados, el servicio se describe como un asistente self-hosted con Gateway como control plane: a través de él, OpenClaw conecta modelos, canales de comunicación, tools, skills, sesiones y WebChat.

OpenClaw puede funcionar en los canales que ya usas: WhatsApp, Telegram, Slack, Discord, Google Chat, Signal, iMessage, BlueBubbles, Microsoft Teams, Matrix, WeChat, WebChat y otros. Los materiales también mencionan escenarios de voz en macOS, iOS y Android, live Canvas, browser control, skills y multi-agent routing.

La ruta de configuración recomendada es la misma en los materiales guardados de OpenClaw: primero instala la CLI, luego ejecuta `openclaw onboard`, y dentro del onboarding configura Gateway, workspace, acceso al modelo, canales y skills.

## Qué Necesitas

- se recomienda Node.js 24; Node.js 22.16+ es compatible
- macOS o Linux; los materiales de OpenClaw recomiendan WSL2 para Windows
- una cuenta en `https://leantoken.tech`
- una API key de LeanToken y, si hace falta, una base URL compatible con OpenAI
- una máquina donde se ejecutará Gateway: un ordenador local o un host Linux remoto

## Casos de Uso con IA

Según los materiales guardados, OpenClaw es especialmente útil en estos escenarios:

- un asistente personal de IA en mensajería y WebChat que responde en tus canales habituales en lugar de un chat web aparte
- ayuda para desarrollo: revisión de la base de código, ejecución de pruebas, depuración, trabajo con GitHub y preparación de pull requests
- tareas de DevOps y operaciones: inspección de logs, monitorización de servicios, alertas y procesos repetitivos de runbooks
- trabajo de investigación: búsqueda y síntesis de información, trabajo con notas y knowledge workflows
- escenarios de voz y acciones locales del dispositivo mediante companion apps y nodes
- automatización del navegador, flujos de Canvas y otros procesos guiados por tools

Si usas BYOK mediante `leantoken.tech`, un solo gateway y un solo conjunto de credenciales se pueden reutilizar en todos esos escenarios en lugar de depender solo de una suscripción OAuth incluida del proveedor.

## BYOK en leantoken.tech

En el contexto de OpenClaw, BYOK significa conectar tu propia API key para el modelo o el gateway en lugar de depender solo de la suscripción OAuth del proveedor. En el material guardado de GitHub, OpenClaw separa explícitamente dos rutas de autenticación:

- subscriptions mediante OAuth
- API keys

Los mismos materiales también mencionan:

- soporte para muchos providers y models
- rotación de perfiles de autenticación para OAuth y API keys
- model failover
- la recomendación de usar el modelo actual más potente al que tengas acceso

Para `leantoken.tech`, esto es útil cuando quieres:

- mantener el acceso al modelo separado de tu suscripción OAuth personal
- gestionar la key y el endpoint de forma centralizada
- cambiar de modelo sin modificar el patrón general de integración
- usar un solo gateway para OpenClaw, canales, skills y flujos de automatización

Los materiales guardados de OpenClaw recomiendan construir esta configuración alrededor de `openclaw onboard`, por eso el flujo práctico de abajo se basa en onboarding y no en ediciones manuales no documentadas de archivos internos.

## Cómo Obtener una API Key en leantoken.tech

Antes de configurar OpenClaw, consigue tu key y endpoint en LeanToken.

![Página pública de LeanToken con Sign up, Log in y Dashboard](../../images/leantoken/leantoken.tech.1.landing.png)

1. Abre `https://leantoken.tech`.
2. Haz clic en `Sign up` si todavía no tienes cuenta, en `Log in` si ya tienes una, o en `Dashboard` si ya has iniciado sesión.

![Panel principal de LeanToken con el endpoint API y accesos rápidos](../../images/leantoken/leantoken.tech.2.dashboard.png)

3. Después de iniciar sesión, abre el dashboard o la página de resumen de la API.
4. Copia el endpoint API. En las capturas aparece `https://api.leantoken.tech/v1`.
5. Ve a la gestión de claves mediante `Manage keys` o `API Keys`.

![Página API Keys de LeanToken con la acción Create key](../../images/leantoken/leantoken.tech.3.key-managment.png)

6. Haz clic en `Create key`.
7. Copia la API key generada.
8. Mantén privados tanto el endpoint como la key. Si la key se expone, rótala y sustitúyela en OpenClaw.

## Cómo Añadir BYOK

El flujo siguiente es el que mejor encaja con los materiales guardados de OpenClaw.

### 1. Instala OpenClaw

```bash
npm install -g openclaw@latest
```

Si quieres instalar el daemon de inmediato y seguir el flujo de onboarding recomendado, usa:

```bash
openclaw onboard --install-daemon
```

En los materiales fuente, esta se describe como la ruta de configuración preferida: onboarding te guía paso a paso por Gateway, workspace, channels y skills.

### 2. Elige la ruta de API key durante model/auth setup

Durante el onboarding, llega a la etapa de acceso al modelo. Si quieres BYOK mediante `leantoken.tech`, usa la ruta de API key en lugar de una suscripción OAuth.

En la práctica, eso significa:

1. Prepara tu API key de LeanToken por adelantado.
2. Si el asistente de configuración pide un endpoint o base URL para un proveedor compatible con OpenAI, usa `https://api.leantoken.tech/v1`.
3. Pega tu API key de LeanToken en el paso de autenticación del proveedor.
4. Elige un modelo disponible a través de tu endpoint de LeanToken.

Los materiales guardados de OpenClaw no muestran los nombres exactos de todas las pantallas de onboarding en cada versión, así que la redacción en el asistente puede variar un poco. Pero la lógica general en los materiales fuente es estable: primero autenticación, luego selección del modelo, después canales y skills.

### 3. Termina la configuración de Gateway y de los canales

Después de model/auth setup, termina el onboarding:

- instala el daemon de Gateway si todavía no está instalado
- configura el workspace
- conecta los canales que necesites, como Telegram, Slack, Discord, WhatsApp o WebChat
- añade solo las skills que realmente piensas usar

Los materiales guardados de OpenClaw indican explícitamente que Gateway es el control plane, mientras que el asistente funciona a través de los canales, apps y tools conectados.

### 4. Verifica que OpenClaw responde mediante tu BYOK

Para la primera comprobación, usa el dashboard y el Gateway local.

Los materiales guardados mencionan el dashboard local en:

```text
http://localhost:18789/
```

Después del onboarding, verifica:

1. que Gateway esté en ejecución
2. que el acceso al modelo funcione sin error de autenticación
3. que el asistente responda en la CLI, en WebChat o en un canal conectado

Para una comprobación manual desde la CLI, puedes usar:

```bash
openclaw agent --message "Haz una autoverificación corta de la configuración y confirma que el modelo responde" --thinking high
```

Si necesitas una ejecución de depuración en foreground, los materiales guardados también incluyen:

```bash
openclaw gateway --port 18789 --verbose
```

## Escenarios Recomendados de BYOK

BYOK mediante LeanToken es especialmente útil para OpenClaw en estas tareas:

- un solo asistente atiende varios canales y el acceso al modelo está centralizado detrás de una única key y endpoint
- quieres controlar la facturación en lugar de depender solo de una suscripción OAuth
- necesitas cambiar de modelo rápidamente para programación, investigación, resúmenes y automatización en segundo plano
- quieres conservar el flujo general de Gateway en OpenClaw mientras usas tu propia ruta de acceso a modelos

## Recomendaciones Prácticas

Los materiales guardados de OpenClaw remarcan por separado la seguridad y la higiene operativa. Eso importa todavía más en una configuración BYOK.

- No expongas Gateway directamente a internet público. Los materiales recomiendan mantenerlo en loopback y usar Tailscale o túneles SSH para acceso remoto.
- No guardes tu API key en un repositorio ni la pegues en configuraciones públicas. Guárdala como secreto y rótala si se filtra.
- Trata las skills como código con acceso a tus datos y tools. Instala solo las skills en las que confíes.
- Usa un aislamiento más estricto para escenarios de grupos y canales si el asistente tiene acceso al shell o a tools de automatización.
- Ejecuta `openclaw doctor` después de la configuración si quieres una forma rápida de detectar configuraciones rotas o arriesgadas.

## Qué Comprobar Después de la Configuración

- OpenClaw arranca sin errores en tu versión de Node.js
- Gateway está disponible en local y abre el dashboard en `localhost:18789`
- el modelo seleccionado realmente responde mediante tu key de LeanToken
- el asistente es visible en el canal que conectaste
- la key y el endpoint se guardan como secretos y no terminaron en git, notas compartidas o el historial del shell
