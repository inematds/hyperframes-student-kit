# HyperFrames Student Kit

**🇧🇷 [Português](README.md) · 🇺🇸 [English](README.en.md) · 🇪🇸 [Español](README.es.md)**

Kit reutilizable de edición de video para **Codex y Claude Code**.
Trae tu propia grabación. Corta el aire muerto, revisa los errores, planifica la historia y
construye motion graphics con HyperFrames y GSAP.

![Se acabó. Es el fin de una era: la edición compleja, las horas de trabajo y el software tradicional dan paso al agente](docs/images/capa-acabou.jpg)

## 📖 Guía de uso

Guía completa (landing + paso a paso): **https://inematds.github.io/hyperframes-student-kit/guia/es/**

**¿Empezando ahora?** Lee la [guía rápida](docs/GUIA-RAPIDO.md) (en portugués): instalación, apertura del
proyecto en Codex o en Claude Code, clave de ElevenLabs, el prompt del primer video,
ciclo de feedback y cómo convertir el resultado en skill. La
[transcripción del tutorial en video](docs/TRANSCRICAO-TUTORIAL.md) (en portugués) está traducida.

**El método:** las cinco etapas (transcribir, cortar, planificar los beats, usar y crear skills,
verificar en ciclo), el prompt anclado en el habla, una grabación en tres estilos, sizzle de
carpeta, video de producto y el ciclo referencia → skill están en [docs/METODO.md](docs/METODO.md) (en portugués)
y las recetas listas en [docs/PROMPTS.md](docs/PROMPTS.md) (en portugués).

## Ejemplos de video corto

Los cuadros de abajo son fotos simuladas de presentador, usadas como referencia de encuadre 9:16.

### Reel de curiosidad: destraba tu proyecto

![Reel de curiosidad: destraba tu proyecto](docs/images/exemplo-1.jpg)

### Reel de curiosidad: construye un mejor sistema de IA

![Reel de curiosidad: construye un mejor sistema de IA](docs/images/exemplo-2.jpg)

### Anuncio en vivo

![Anuncio en vivo](docs/images/exemplo-3.jpg)

## Qué incluye

- **15 skills**, espejadas para los dos asistentes, con sus scripts auxiliares y referencias.
- **406 cards de motion graphics en borrador** en dos estilos, con manifiestos, tokens CSS y slots editables.
- **Dos plantillas de escena:** papel cuadriculado oscuro y un popout de vidrio a la izquierda.
- Herramientas de transcripción, corte de silencios, detección de errores, renderizado de los cortes revisados,
  re-temporización de transcripción, revisión de EDL, validación de sincronía de beats y preflight.
- **Edición de video corto:** reels, YouTube Shorts, planificación de gancho y recompensa, subtítulos precisos, B-roll en movimiento y revisión de audio.
- **12 proyectos de enseñanza existentes**, preservados del kit original (quedan solo en la copia local; `video-projects/` no se versiona en este repositorio).
- Una composición inicial sintética y una fixture de edición que no necesitan grabación ni clave de API.

## Herramientas y servicios opcionales

**El kit usa ElevenLabs Scribe por defecto para transcribir y Kie.ai para generar videos e
imágenes.** Trae tus propias claves de API y créditos al usar esos servicios.
Puedes pedirle al asistente que use OpenAI Whisper o Whisper local, o
proporcionar una transcripción existente con marcas de tiempo por palabra. Los assets generados son opcionales.

El starter local no necesita ninguna API de pago de transcripción o generación. Consulta la
[guía de herramientas, cuentas y claves de API](docs/TOOLS-AND-API-KEYS.md) (en portugués) para las herramientas obligatorias,
servicios opcionales, detalles de configuración y prompts que puedes copiar.

## Instalación

Instala Node.js **22 o más reciente**, Git, FFmpeg (incluido ffprobe) y Chrome o
Chromium. Deja `node`, `ffmpeg` y `ffprobe` disponibles en tu terminal.
Luego ejecuta estos comandos en PowerShell, en la Terminal de macOS o en un shell de Linux:

```sh
git clone https://github.com/inematds/hyperframes-student-kit.git
cd hyperframes-student-kit
npm ci
npm run setup
npm test
```

El setup verifica las herramientas y crea el `.env` solo si no existe. Agrega solo las claves
de los servicios que elijas. El auxiliar de transcripción incluido usa ElevenLabs;
las integraciones con Whisper y Kie.ai necesitan la configuración descrita en la
[guía de herramientas](docs/TOOLS-AND-API-KEYS.md) (en portugués). [Configuración y solución de problemas](docs/SETUP.md) (en portugués).

## Renderiza tu primer ejemplo

```sh
npm run demo
cd video-projects/demo
npx hyperframes lint
npx hyperframes preview
```

Recorre la animación de ocho segundos en el Studio. Después de revisarla, detén el preview
con Ctrl+C y renderiza:

```sh
npx hyperframes render --quality draft --output renders/demo.mp4
```

La demo usa GSAP local. HyperFrames puede descargar y almacenar en caché sus sustituciones de fuentes en el primer renderizado. No tiene narración. La
[transcripción sintética](examples/editing/source.json) separada ejercita las herramientas de corte;
son datos de prueba ficticios, no una transcripción de la animación de título.

## Edita tu grabación

Abre la carpeta de este repositorio en Codex o en Claude Code y di:

> Usa edit-video para editar mi grabación en [ruta local]. Mantén mis ejemplos y las
> lecciones centrales. Recorta el aire muerto, muéstrame los cortes de errores propuestos y usa el estilo de
> papel cuadriculado oscuro con cards de vidrio ocasionales. Produce un borrador revisado.

Codex: `$edit-video`. Claude Code: `/edit-video`. Para una sola operación usa
`cut-silences`, `cut-mistakes`, `video-storytelling` o `style-library`.
Consulta el [flujo paso a paso](docs/WORKFLOW.md) (en portugués), las [recetas de prompt](docs/PROMPTS.md) (en portugués)
y el [cuaderno de storytelling](docs/STORYTELLING-WORKBOOK.md) (en portugués).

## Crea un reel o YouTube Short

> Usa short-form-edit para convertir mi grabación en [ruta local] en un reel 9:16.
> Construye un gancho y una recompensa verdaderos, preserva mi sentido, recorta los errores y
> agrega subtítulos precisos, imágenes en movimiento con propósito y diseño de sonido. Prepara
> un borrador para revisión. Usa mi grabación existente antes de proponer assets generados.

Codex: `$short-form-edit`. Claude Code: `/short-form-edit`.
La skill incluye referencias de planificación y validadores para el timing de subtítulos, el mapeo
de fuentes, la cobertura de escenas y la reutilización de imágenes. Es un flujo guiado por agente;
revisa el movimiento y el audio reales antes de publicar.
[Paso a paso de video corto y comandos de validación](docs/SHORT-FORM.md) (en portugués).

## Crea un showreel de motion design

> Usa motion-showreel para crear un showreel de 15 segundos para [marca]. Estudia mi reel
> de referencia en [ruta local], si te doy uno. Elige un motivo que se transforme en
> todos los capítulos, corta en la grilla de tiempos de la música y termina en el logo. Muéstrame el
> storyboard y la hoja de tiempos antes de construir, y confirma antes de cualquier generación de pago.

Codex: `$motion-showreel`. Claude Code: `/motion-showreel`.
La skill incluye el análisis medido del reel que la originó, una biblioteca de técnicas por
capítulo, una plantilla de HUD y herramientas Node para analizar un video de referencia,
medir la grilla de tiempos de una música, empalmarla en la grilla de cortes y premezclar efectos
de sonido. La música y los SFX pueden ser gratuitos; para el objeto héroe, prefiere tu suscripción
Kling AI (CLI `kling`). Kie.ai y ElevenLabs quedan como alternativas de pago opcionales.

## Ejemplos existentes y migración

Este es el repositorio principal del student kit. El kit de pipeline de video más nuevo se
fusionó aquí con los dos historiales Git preservados. Los 12 proyectos originales siguen
en `video-projects/`, junto con los ejemplos de marca compartidos originales y las
skills `make-a-video`, `short-form-video` y `website-to-hyperframes`.
Usa `short-form-edit` para reels nuevos; `short-form-video` documenta las composiciones más antiguas
de los Shorts de mayo. [Notas de migración y compatibilidad](docs/MIGRATION.md) (en portugués).

## Explora y personaliza

| Recurso | Empieza aquí |
| --- | --- |
| Guía rápida paso a paso | [docs/GUIA-RAPIDO.md](docs/GUIA-RAPIDO.md) (en portugués) |
| Transcripción del tutorial en video | [docs/TRANSCRICAO-TUTORIAL.md](docs/TRANSCRICAO-TUTORIAL.md) (en portugués) |
| Instrucciones compartidas de los agentes | [AGENTS.md](AGENTS.md) |
| Vocabulario de movimiento y transición | [MOTION_PHILOSOPHY.md](MOTION_PHILOSOPHY.md) |
| Biblioteca de cards y tokens de diseño | [Guía de la biblioteca](style-library/GUIDE.md) |
| Metadatos buscables de los cards | [registry.json](style-library/registry.json) |
| Plantillas de escena completa | [Guía de plantillas](style-templates/README.md) |
| Configuración y espejado de Codex | [.codex/README.md](.codex/README.md) |
| Verificaciones de release y límites | [Verificación](docs/VERIFICATION.md) (en portugués) |
| Recursos de terceros | [Avisos](THIRD_PARTY_NOTICES.md) |

Los cards son **assets de borrador** reutilizables. Prueba los cards elegidos con tu texto
y tu grabación. Las plantillas de la biblioteca pueden cargar GSAP y Google Fonts desde sus
CDNs públicas; localiza esas dependencias al armar un proyecto final.

Las imágenes de ejemplo son fotos simuladas; no se incluyó ninguna grabación privada, transcripción, credencial
ni configuración personal. Los ejemplos que ya eran públicos siguen en el
repositorio. Las carpetas nuevas en `video-projects/` y `raw-media/` se ignoran automáticamente;
los archivos ya rastreados por Git siguen rastreados. Crea un proyecto nuevo para tu propia grabación.
