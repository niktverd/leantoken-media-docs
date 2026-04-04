# Obsidian

## Sobre el Servicio

Obsidian es una aplicacion local-first de notas y una base de conocimiento personal construida sobre archivos Markdown normales. Su fortaleza no es una unica funcion de IA integrada, sino el ecosistema de community plugins: mantienes tu vault localmente, eliges tu propio workflow y conectas solo las herramientas de IA que realmente necesitas.

Para un flujo BYOK en Obsidian conviene separar dos roles:

- `AI Providers` guarda y reutiliza la configuracion de proveedores de IA en un solo lugar
- un plugin consumidor como `Local GPT` usa despues ese proveedor para acciones sobre texto, RAG, vision y flujos relacionados con IA

Esa distincion importa: `AI Providers` no es un chat por si solo y no edita notas directamente. Es una capa de configuracion que evita volver a introducir la misma `API key`, `Provider URL` y el mismo modelo en cada plugin.

En los materiales oficiales, `AI Providers` se presenta como un hub centralizado para configuraciones de proveedores de IA con soporte para `20+` proveedores, incluidos `OpenAI`, `Anthropic`, `OpenRouter`, `Ollama`, `Google Gemini`, `Groq`, `LM Studio` y `OpenAI compatible API`. Esto encaja bien con `leantoken.tech`, porque LeanToken expone un endpoint OpenAI-compatible en `https://api.leantoken.tech/v1`.

Esta guia usa la siguiente combinacion como ejemplo principal:

1. `Obsidian`
2. `AI Providers`
3. `Local GPT`
4. `leantoken.tech` como endpoint BYOK OpenAI-compatible

## Qué Necesitas

- Obsidian Desktop instalado
- `Community plugins` activados
- el plugin `AI Providers`
- el plugin `Local GPT` si quieres acciones de IA directamente dentro de las notas
- una cuenta en `https://leantoken.tech`
- una API key de LeanToken
- la base URL `https://api.leantoken.tech/v1`
- un modelo disponible en tu cuenta de LeanToken
- opcionalmente, un proveedor de embeddings separado si quieres funciones RAG en `Local GPT`

## Casos de Uso con IA

Obsidian es especialmente util cuando la IA debe trabajar dentro de una base de conocimiento viva y no fuera de tus notas.

Casos de uso practicos:

- resumir notas largas, meeting notes, material de research y articulos
- extraer action items, tesis, checklists y listas estructuradas a partir de texto
- continuar borradores, reescribir parrafos y corregir estilo o gramatica
- hacer Q&A sobre notas enlazadas, backlinks y archivos PDF mediante plugins con RAG
- generar notas nuevas, resumenes intermedios, paginas tipo wiki y digest de investigacion
- combinar modelos locales y en la nube dentro del mismo vault, donde algunas tareas quedan offline y otras van a un endpoint externo
- guardar los resultados generados de vuelta en el vault para que cada nueva investigacion fortalezca tu base de conocimiento personal

Un buen ejemplo de este workflow aparece en el post de Andrej Karpathy sobre `LLM Knowledge Bases`: recopila material bruto en Markdown e imagenes, usa un LLM para compilar gradualmente una wiki, hace preguntas complejas sobre esa base de conocimiento y guarda los resultados otra vez en Obsidian. Enlace al post: [LLM Knowledge Bases by Andrej Karpathy](https://x.com/karpathy/status/2039805659525644595).

Llevado a Obsidian, el flujo se ve asi:

1. recopilar articulos, notas, repositorios, PDF e imagenes en un vault
2. usar IA para resumir, categorizar y generar paginas wiki internas
3. hacer preguntas sobre el corpus acumulado
4. guardar respuestas, diagramas, slides y notas derivadas de vuelta en el vault

## Por Qué BYOK Es Especialmente Util en Obsidian

BYOK es util en Obsidian por varias razones practicas:

- mantienes una sola key controlada y un solo endpoint reutilizable entre plugins compatibles
- eliges el modelo tu mismo en lugar de depender solo del proveedor integrado del plugin
- puedes mezclar cloud y local setup, por ejemplo `Ollama` para tareas locales y `LeanToken` para modelos remotos mas fuertes
- la logica del proveedor se vuelve mas facil de reutilizar entre vaults y plugins porque esta separada en un hub dedicado
- resulta mas sencillo controlar costos, rotar secretos y gestionar credenciales

Para `leantoken.tech`, la configuracion normalmente se reduce a dos valores:

- `API key`
- `Provider URL` o `base URL`: `https://api.leantoken.tech/v1`

## Cómo Obtener una API Key en leantoken.tech

Antes de configurar Obsidian, consigue tu key y endpoint en LeanToken.

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
8. Vuelve a Obsidian y pega la key en el campo `API key` del proveedor seleccionado.
9. Pega `https://api.leantoken.tech/v1` en `Provider URL` si el plugin espera un campo de endpoint separado.
10. Trata la key como un secreto. Si se filtra en git, notas, issues, capturas, shell history o logs, rotala.

## Qué Plugins Usar Para Este Flujo

Hay muchos plugins de IA en Obsidian, pero para un flujo BYOK documentado y repetible resulta mas facil usar una pareja con responsabilidades separadas:

- `AI Providers` gestiona la configuracion del proveedor
- `Local GPT` usa ese proveedor para acciones concretas de IA

Por que esta pareja:

- el README de `AI Providers` enumera explicitamente `Local GPT` como un plugin que usa este hub
- el README de `Local GPT` indica de forma explicita que necesitas `AI Providers` para configurar proveedores
- `Local GPT` ya soporta acciones comunes sobre texto, soporte de imagenes, action palette y flujos relacionados con RAG

Si mas adelante añades otros plugins de IA, la logica sigue siendo la misma: primero comprueba si el plugin puede consumir configuraciones desde `AI Providers`, y luego reutiliza el proveedor LeanToken que ya creaste.

## Cómo Instalar AI Providers y Local GPT

### 1. Activa community plugins

1. Abre `Settings`.
2. Ve a `Community plugins`.
3. Si `Restricted mode` esta activado, desactivalo para poder instalar plugins de terceros.

### 2. Instala AI Providers

1. En `Community plugins`, haz clic en `Browse`.
2. Busca `AI Providers`.
3. Instala el plugin y activalo.
4. Despues de la instalacion, abre su pagina de ajustes en `Settings`.

### 3. Instala Local GPT

1. Vuelve a abrir `Browse` en `Community plugins`.
2. Busca `Local GPT`.
3. Instala el plugin y activalo.
4. Si quieres, configura hotkeys para `Local GPT: Show context menu` y `Local GPT: Action Palette` en `Hotkeys`.

## Cómo Agregar LeanToken en AI Providers

Debajo se muestra la pantalla de ajustes de `AI Providers` en Obsidian con una lista de proveedores ya configurados.

![Pantalla de AI Providers en Obsidian con proveedores configurados y el boton para agregar un nuevo proveedor](../../images/obsidian/obsidian-ai-providers-settings.png)

Flujo tipico para LeanToken:

1. Abre `Settings | Community plugins | AI Providers`.
2. Haz clic en `+` para añadir un proveedor nuevo.
3. Elige el tipo de proveedor que corresponda a una API OpenAI-compatible en tu version del plugin.
4. Si la interfaz incluye una opcion explicita `OpenAI compatible API`, usala.
5. Si las etiquetas cambian un poco, sigue el requisito central: el proveedor debe permitir definir manualmente `Provider URL` y trabajar con un endpoint OpenAI-compatible.
6. Pon un nombre a la conexion, por ejemplo `LeanToken`.
7. Pega `https://api.leantoken.tech/v1` en `Provider URL`.
8. Pega tu key de LeanToken en `API key`.
9. Haz clic en el boton de refresco de modelos si tu version lo muestra.
10. Selecciona uno de los modelos disponibles en tu cuenta de LeanToken.
11. Guarda los ajustes.

Despues de eso, el proveedor LeanToken deberia aparecer en la lista compartida de AI Providers junto a tus otros proveedores.

Notas practicas:

- si quieres usar varios modelos, crea varias entradas de proveedor con nombres distintos
- si necesitas un backend de embeddings separado para RAG, guardalo como una entrada aparte en lugar de mezclarlo con la configuracion de chat
- si necesitas un modelo separado con capacidad de vision, guardalo tambien como otra entrada de proveedor

## Cómo Usar LeanToken en Local GPT

Una vez guardado el proveedor en `AI Providers`, puedes pasar a las acciones dentro de las notas mediante `Local GPT`.

Flujo basico:

1. Abre `Settings | Community plugins | Local GPT`.
2. Busca el selector principal de proveedor de IA o el ajuste que consume configuraciones desde `AI Providers`.
3. Selecciona el proveedor LeanToken que creaste antes.
4. Si quieres funciones de vision, apunta Local GPT a una entrada separada con un modelo capaz de procesar imagenes.
5. Si quieres RAG o enhanced actions, configura el proveedor de embeddings por separado.
6. Abre cualquier nota, selecciona texto y llama a `Local GPT` mediante el context menu o `Action Palette`.

Acciones utiles para empezar en `Local GPT`:

- `Continue writing`
- `Summarize text`
- `Fix spelling and grammar`
- `Find action items`
- `General help`

Con eso basta para hacer una prueba rapida de la conexion con LeanToken.

## Workflow Recomendado de Obsidian Para Bases de Conocimiento

Si usas Obsidian no como un simple cuaderno sino como una base de conocimiento asistida por IA, un workflow practico se ve asi:

1. recopila material fuente en el vault: articulos, notas, PDF, imagenes y enlaces
2. usa Obsidian Web Clipper cuando quieras guardar paginas web como Markdown
3. usa `Local GPT` para resumir, reescribir y extraer estructura de las notas
4. guarda el material derivado cerca del origen: summary notes, comparison notes, paginas wiki y esquemas de slides
5. añade gradualmente mas enlaces entre notas en vaults grandes para que los plugins de IA puedan aprovechar mejor links y backlinks
6. añade un proveedor de embeddings dedicado solo donde RAG realmente ayude

Esto coincide bastante bien con el workflow de Karpathy:

- las fuentes en bruto se recopilan en una base de conocimiento local
- un LLM ayuda a compilar una wiki a partir de archivos Markdown
- las preguntas mas complejas se hacen sobre el corpus acumulado
- los resultados vuelven a la base de conocimiento en lugar de perderse en una ventana de chat

## Qué Comprobar Despues de la Configuracion

Smoke test minimo:

1. abre una nota con algunos parrafos de texto
2. selecciona un fragmento
3. llama a `Local GPT`
4. elige `Summarize text`
5. confirma que la respuesta llega sin error de autenticacion ni error del proveedor

Si no funciona:

- verifica que `Provider URL` sea `https://api.leantoken.tech/v1`
- verifica que el campo `API key` tenga una key valida de LeanToken
- refresca la lista de modelos
- confirma que el modelo seleccionado realmente esta disponible en tu cuenta
- si falla especificamente RAG, revisa el proveedor de embeddings por separado porque los chat models y los embeddings no son lo mismo

## Limitaciones y Notas Importantes

- `AI Providers` no es un asistente de IA independiente. Solo guarda y expone configuracion para otros plugins.
- Los nombres exactos de campos y presets pueden variar un poco segun la version del plugin.
- `Local GPT` puede combinar modelos locales mediante `Ollama` con proveedores cloud u OpenAI-compatible, por lo que LeanToken funciona bien en setups mixtos.
- Los escenarios de vision y RAG suelen funcionar mejor con entradas de proveedor separadas para tareas diferentes.
- Si tu vault contiene notas sensibles, decide de antemano que datos pueden enviarse a un endpoint externo de IA y que datos deben quedarse en modelos locales.

## Enlaces Utiles

- [AI Providers: documentacion](https://pfrankov-obsidian-ai-providers.mintlify.app/)
- [AI Providers: repositorio en GitHub](https://github.com/pfrankov/obsidian-ai-providers)
- [AI Providers: pagina del community plugin](https://obsidian.md/plugins?id=ai-providers)
- [Local GPT: documentacion](https://pfrankov-obsidian-local-gpt.mintlify.app/)
- [Local GPT: repositorio en GitHub](https://github.com/pfrankov/obsidian-local-gpt)
- [Local GPT: pagina del community plugin](https://obsidian.md/plugins?id=local-gpt)
- [Obsidian Web Clipper](https://obsidian.md/clipper)
- [Andrej Karpathy: LLM Knowledge Bases](https://x.com/karpathy/status/2039805659525644595)
