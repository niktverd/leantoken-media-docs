# JetBrains IDEs

## Descripcion del servicio

JetBrains IDEs son herramientas de desarrollo de JetBrains, incluidas [CLion](https://blog.jetbrains.com/clion/), [DataGrip](https://blog.jetbrains.com/datagrip/), [DataSpell](https://blog.jetbrains.com/dataspell/), [GoLand](https://blog.jetbrains.com/go/), [IntelliJ IDEA](https://blog.jetbrains.com/idea/), [PhpStorm](https://blog.jetbrains.com/phpstorm/), [PyCharm](https://blog.jetbrains.com/pycharm/), [RustRover](https://blog.jetbrains.com/rust/), [Rider](https://blog.jetbrains.com/dotnet/tag/rider/), [RubyMine](https://blog.jetbrains.com/ruby/), y [WebStorm](https://blog.jetbrains.com/webstorm/).

El soporte BYOK de JetBrains te permite conectar tu propio proveedor y tus propios modelos de IA directamente dentro del IDE. Segun la entrada guardada del blog de JetBrains AI, BYOK esta disponible en AI chat y en agentes de IA, incluidos Junie y Claude Agent, siempre que tu proveedor ofrezca los modelos necesarios.

## Casos de uso con IA

- explicar codigo desconocido, trazas de error y la estructura del proyecto sin salir del IDE
- redactar refactorizaciones, pruebas, scripts y codigo repetitivo con mayor rapidez
- usar AI chat o flujos con agentes con el modelo que prefieras para revision de codigo, implementacion y depuracion
- conectar endpoints locales o autoalojados mediante APIs compatibles con OpenAI cuando la privacidad o el control de costos importan
- generar documentacion tecnica, notas de cambios y resumenes de revision mientras trabajas en el repositorio

## BYOK en leantoken.tech

Bring Your Own Key en `leantoken.tech` significa usar tus propias credenciales del proveedor en lugar de depender solo de una configuracion compartida incluida. En JetBrains IDEs, esto es util cuando quieres controlar el modelo, el proveedor, el endpoint, la facturacion y el limite de privacidad por tu cuenta.

Segun el anuncio de JetBrains, BYOK admite claves API de Anthropic, OpenAI y otros proveedores compatibles con OpenAI. JetBrains tambien destaca que las claves se almacenan localmente en tu maquina y no se comparten con JetBrains.

JetBrains tambien incluyo un video corto en la entrada original: [Bring Your Own Key in JetBrains IDEs](https://www.youtube.com/watch?v=JcJyoFyLEOs).

![AI chat de JetBrains usando modelos BYOK de terceros](../../images/jetbrains-ides-byok-chat.webp)

El selector de modelos del chat puede mostrar los modelos disponibles a traves del proveedor conectado, para que puedas cambiar de modelo directamente dentro del IDE.

## Como obtener una API key en leantoken.tech

Usa este flujo antes de configurar JetBrains IDEs:

![Pagina publica de LeanToken con Sign up, Log in y Dashboard](../../images/leantoken.tech.1.landing.png)

1. Abre `https://leantoken.tech`.
2. Haz clic en `Sign up` si todavia no tienes cuenta, en `Log in` si ya tienes una, o en `Dashboard` si ya has iniciado sesion.

![Panel principal de LeanToken con el endpoint API y accesos rapidos](../../images/leantoken.tech.2.dashboard.png)

3. Despues de iniciar sesion, abre el dashboard o la pagina de resumen.
4. Copia el endpoint API que aparece alli. En las capturas, el endpoint es `https://api.leantoken.tech/v1`.
5. Usa `Manage keys` o abre `API Keys` desde la barra lateral izquierda.

![Pagina API Keys de LeanToken con la accion Create key](../../images/leantoken.tech.3.key-managment.png)

6. En la pagina `API Keys`, haz clic en `Create key`.
7. Copia la API key generada que quieras usar en JetBrains IDEs.
8. Mantén privados tanto la clave como el endpoint. Si la clave se expone, revocala o rotala y sustituyela en la configuracion del IDE.

## Como agregar BYOK

La entrada guardada del blog de JetBrains muestra la ruta de configuracion como `Settings | Tools | AI Assistant | Models`.

![Configuracion BYOK anterior en la pagina Models](../../images/jetbrains-ides-byok-models-settings.webp)

En la interfaz mas reciente de tu captura, la configuracion aparece en `Settings | Tools | AI Assistant | Providers & API keys`.

![Pantalla actual Providers & API keys de JetBrains para configurar BYOK](../../images/jetbrains-ides-byok-providers-api-keys.jpg)

Flujo habitual en la interfaz actual:

1. Obtén tu API key de LeanToken y, si hace falta, la URL API correspondiente siguiendo la seccion anterior.
2. En JetBrains IDEs, abre `Settings | Tools | AI Assistant | Providers & API keys`.
3. En `Third-party AI providers`, elige el tipo de proveedor.
4. Si LeanToken esta expuesto como un endpoint compatible con OpenAI para tu configuracion, introduce la base URL `https://api.leantoken.tech/v1` y tu API key.
5. Haz clic en `Test Connection`.
6. Activa `Tool calling` si tu proveedor y tu flujo de trabajo lo admiten.
7. En `Model Assignment`, elige que modelos conectados deben usarse para las funciones principales y los helpers.
8. Abre AI chat y confirma que los modelos esperados aparecen en el selector.

## JetBrains AI y BYOK juntos

JetBrains indica expresamente que no todas las funciones de IA estan garantizadas cuando se usan solo modelos de proveedores externos. Para la finalizacion de codigo en la nube y otras funciones integradas en el editor, JetBrains recomienda activar tambien JetBrains AI.

La configuracion practica es:

- usar tu propia clave para el proveedor y los modelos que quieras controlar directamente
- mantener JetBrains AI activo cuando necesites una mejor cobertura para las funciones integradas del IDE
- elegir la combinacion proveedor-modelo en el chat cuando necesites una capacidad, un precio o un perfil de privacidad concretos

## Escenarios recomendados de BYOK

- Usa modelos de Anthropic u OpenAI cuando quieras una asistencia fuerte para programacion en chat y en flujos con agentes.
- Usa una pasarela compatible con OpenAI cuando tu equipo estandariza el acceso a varios proveedores detras de un solo endpoint.
- Usa un endpoint local cuando quieras experimentar de forma privada dentro del IDE sin enviar prompts a una API publica alojada.
- Usa `leantoken.tech` como el lugar operativo donde gestionas y reutilizas tus propias credenciales de proveedor en toda tu configuracion de IA.

## Notas

- JetBrains dice que se esperan mas proveedores, incluidos Google Gemini, Azure y Amazon Bedrock.
- Las etiquetas exactas del menu pueden variar segun la version del IDE. El articulo guardado usa `Models`, mientras que la captura que proporcionaste muestra `Providers & API keys`.
- El soporte de agentes sigue dependiendo de que tu proveedor ofrezca los modelos requeridos por Junie o Claude Agent.
