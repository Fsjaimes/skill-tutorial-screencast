# Tutorial Screencast Skill

> Videos tutoriales de software como grabación de pantalla simulada: réplica fiel de la interfaz, cursor animado, pulso en cada clic, zooms con zona resaltada, rótulos «Paso N», etiquetas cortas, e intro y cierre de marca.

Una skill para [Claude Code](https://claude.com/claude-code) que convierte el tema de un tutorial (o un guion borrador) en un video de 60–90 s, 1920×1080 a 60 fps, que enseña a hacer una tarea en un sistema de forma que quien lo vea pueda repetirla solo. Construye el video con [HyperFrames](https://hyperframes.heygen.com) (HTML renderizado a MP4), así que cada video es editable y se renderiza siempre igual.

## Qué genera

| Parte | Qué incluye |
|---|---|
| **Guion** (`GUION.md`) | Correcciones del pedido frente a la pantalla real, propósito, datos de ejemplo y tabla de escenas con rótulos, zooms y etiquetas |
| **Intro** (≈6 s) | Marca, título «Cómo ‹tarea›» y una línea de para qué sirve |
| **Pasos** (3–6) | Navegación, explicación de la pantalla, acción (clic, casilla, escritura letra a letra), advertencias, guardado con el mensaje real y verificación |
| **Cierre** (≈7 s) | «¡Listo! Ya ‹logro›.», un resumen de una línea y la marca |
| **Proyecto HyperFrames** | Editable en la vista previa (Studio) y renderizable a MP4 |

El sistema visual completo (colores, tipografía, niveles de zoom, tiempos del cursor, curvas de animación y ritmo por escena) está en `references/style.md`, con valores concretos.

## Requisitos

### Skills de HyperFrames (obligatorio)

Esta skill se apoya en **`/hyperframes`**, que define el contrato de render (clips, timelines, determinismo) y los comandos `check`, `snapshot`, `preview` y `render`. Sin ella, Claude no tiene las reglas para que el video se renderice bien. Instálala antes, con el mismo alcance que vayas a usar para esta skill:

```bash
npx skills add heygen-com/hyperframes -g
```

Eso instala el conjunto de skills de HyperFrames (`hyperframes`, `hyperframes-cli`, `hyperframes-core`, `hyperframes-animation`, `media-use`, etc.). Como mínimo hacen falta `hyperframes` y `hyperframes-cli`.

### Herramientas en la máquina

| Herramienta | Para qué | Nota |
|---|---|---|
| Node.js 22+ | Ejecutar `npx hyperframes` | La CLI se descarga sola con `npx` |
| Chrome / Chromium | Vista previa y render | HyperFrames descarga su propio *headless shell* la primera vez |
| **ffmpeg** o **Docker** | Exportar el MP4 | Sin ffmpeg, renderiza con `npx hyperframes render --docker` (con Docker corriendo) |

Revisa todo de una vez con `npx hyperframes doctor`.

## Instalación

### Global, con `-g` (recomendado)

```bash
npx skills add Fsjaimes/tutorial-screencast -g
```

Con `-g` la skill se instala **a nivel de usuario** (en `~/.agents/skills/`, enlazada a `~/.claude/skills/`) y Claude Code la tiene disponible en **todos tus proyectos**. Es lo recomendado si haces tutoriales de varios sistemas o desde distintas carpetas.

### Local al proyecto, sin `-g`

```bash
cd ruta/de/tu/proyecto
npx skills add Fsjaimes/tutorial-screencast
```

Sin `-g` la skill se instala **solo en el proyecto donde ejecutas el comando** (en su carpeta `.claude/skills/`, y registrada en su `skills-lock.json`). Claude Code solo la ve cuando trabaja dentro de esa carpeta. Sirve para fijar la skill a un repositorio concreto (por ejemplo, la carpeta de videos del equipo) o para probar una versión sin afectar al resto de proyectos.

> Las skills de HyperFrames tienen que estar disponibles en el mismo alcance. Si instalas esta skill sin `-g` en un proyecto, instala también `heygen-com/hyperframes` ahí o de forma global.

### Actualizar

```bash
npx skills update tutorial-screencast        # o: npx skills update -g para las globales
```

### Manual (sin `npx skills`)

```bash
git clone https://github.com/Fsjaimes/tutorial-screencast.git ~/.claude/skills/tutorial-screencast
```

## Uso

Dentro de Claude Code, pídelo con naturalidad o invoca la skill:

```
/tutorial-screencast cómo configurar las Bodegas de clasificación en Suite - DOMA
hazme un video tutorial de cómo crear un usuario y asignarle un rol, el código está en ../SUITE
quiero un paso a paso en video de cómo exportar un reporte a Excel (te paso la captura)
```

Lo único imprescindible es **la pantalla real**: el código fuente de la página, una captura o acceso a la app. La skill verifica nombres, botones y mensajes contra ella antes de escribir el guion, y documenta las diferencias con el pedido. Después construye el proyecto, revisa los fotogramas con su checklist, abre la vista previa y renderiza el MP4 cuando lo apruebes.

## Estructura

```
tutorial-screencast/
├── SKILL.md                 # punto de entrada: principios, flujo de 10 pasos, errores conocidos y checklist
├── references/
│   ├── style.md             # valores concretos: color, tipografía, zoom, cursor, clic, etiquetas, ritmo y curvas
│   ├── narrative.md         # estructura intro → pasos → cierre, reglas de textos y plantilla de guion
│   └── ui-replica.md        # cómo replicar una pantalla desde código o captura
├── assets/
│   ├── template/            # proyecto base: kit (cámara, cursor, clic, escritura, resaltado, rótulos) + demo
│   └── example/             # video de referencia: «Cómo configurar Bodegas de clasificación»
└── evals/                   # casos de prueba de la skill
```

## Identidad visual por defecto

- **Marca DOMA:** primario `#25a0e2`, marca «D» y fuente Saira. Se cambia por la de cualquier otra marca en los tokens y en `assets/fonts/`.
- **HUD del video:** rótulos en tinta `#16202a`, etiquetas blancas con borde de acento y un oscurecido `rgba(15,25,35,.34)` fuera de la zona resaltada.
- **Movimiento:** cámara y cursor con `power2.inOut`, textos con `expo.out`, y reacciones de la UI en 0,12–0,2 s.

## Licencias

- Código y documentación de la skill: [MIT](LICENSE).
- La fuente **Saira** (`assets/template/assets/fonts/`) se distribuye bajo la [SIL Open Font License 1.1](assets/template/assets/fonts/OFL.txt). © The Saira Project Authors.
