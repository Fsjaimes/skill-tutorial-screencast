# Sonido: modos, biblioteca y narración

## Contenido
1. Los tres modos
2. Biblioteca de música (catálogo)
3. Efectos de sonido (incluidos en la skill)
4. Volúmenes y mezcla
5. Cómo se escribe el sonido en la composición
6. Modo narración: guion de locución → audio de la persona → ajuste
7. Requisitos y problemas frecuentes

## 1. Los tres modos

El modo se decide en el paso 1 del flujo y se anota en `GUION.md` (`**Audio:** ambiente`). Si el usuario no lo dice, usa **ambiente**.

| Modo | Qué lleva | Cuándo |
|---|---|---|
| `ambiente` *(por defecto)* | Música de la biblioteca a nivel de fondo, efectos de clic y éxito, y whoosh en intro y cierre | La mayoría de tutoriales y funcionalidades nuevas |
| `narración` | La voz que entrega la persona (un audio por paso), la música como cama bajo la voz y los mismos efectos, más bajos | Procesos largos, capacitaciones, onboarding |
| `silencio` | Nada | Videos incrustados en una página, un manual o un sistema donde el audio estorba |

La música y la voz **no se generan**: la música sale de una biblioteca curada (sonido de marca consistente y licencia verificada una vez) y la voz la produce la persona a partir del guion de locución que escribe el agente (su propia voz, Kokoro, HeyGen, ElevenLabs: la herramienta la elige ella).

## 2. Biblioteca de música (catálogo)

Las pistas son de [Pixabay](https://pixabay.com/service/license-summary/): uso comercial gratis y sin atribución, **pero no se pueden redistribuir «tal cual»**. Por eso los MP3 **no viven en esta skill** (su repositorio es público): cada persona los tiene en una carpeta local.

**Dónde buscarlos**, en este orden:
1. La carpeta que indique la variable de entorno `TUTORIAL_AUDIO_DIR`.
2. `~/Desktop/PROYECTOS/videos/audio/musica/` (la biblioteca del equipo de Sódeker).
3. Si no aparecen, pide la ruta al usuario o dale el enlace de descarga de la pista elegida.

Los archivos conservan el nombre con que los descarga Pixabay (`autor-titulo-ID.mp3`), así que el **ID** identifica la pista en cualquier máquina.

| ID | Pista | Autor | Duración | Carácter | Modo y uso recomendado | Archivo |
|---|---|---|---|---|---|---|
| 568180 | [Corporate Explainer Video](https://pixabay.com/music/corporate-corporate-explainer-video-568180/) | alex-morgan | 3:29 | Corporativa explicativa, constante | **Tutorial por defecto** (ambiente); alcanza para videos largos | `alex-morgan-corporate-explainer-video-568180.mp3` |
| 603150 | [Corporate Calm](https://pixabay.com/music/corporate-corporate-calm-603150/) | leberch | 2:44 | Corporativa calmada | Tutoriales y capacitaciones | `leberch-corporate-calm-603150.mp3` |
| 164833 | [Minimal](https://pixabay.com/music/corporate-minimal-164833/) | PaulYudin | 1:57 | Minimalista, poco protagonismo | **Bajo narración** (cama) | `paulyudin-minimal-164833.mp3` |
| 585951 | [Background Calm](https://pixabay.com/music/modern-classical-background-calm-585951/) | VelarioMusic | 1:47 | Clásica moderna, suave | Bajo narración; procesos sensibles (nómina, finanzas) | `velariomusic-background-calm-585951.mp3` |
| 345194 | [Minimal Technology](https://pixabay.com/music/beats-minimal-technology-345194/) | NastelBom | 1:59 | Beat tech discreto | Funcionalidades nuevas | `nastelbom-minimal-technology-345194.mp3` |
| 631954 | [Technology Background](https://pixabay.com/music/corporate-technology-background-631954/) | AtlasAudio | 2:10 | Tech corporativa | Funcionalidades nuevas, novedades | `atlasaudio-technology-background-631954.mp3` |
| 559977 | [Corporate Startup Promo Music](https://pixabay.com/music/corporate-corporate-startup-promo-music-559977/) | alex-morgan | 0:39 | Promo enérgica, corta | Shorts y reels de ≤ 40 s | `alex-morgan-corporate-startup-promo-music-559977.mp3` |
| 593075 | [Tech Corporate](https://pixabay.com/music/corporate-tech-corporate-593075/) | The_Mountain | 3:07 | Tech optimista | Promos y lanzamientos | `the_mountain-tech-corporate-593075.mp3` |
| 573936 | [Technology Future Innovation Pulse](https://pixabay.com/music/techno-trance-technology-future-innovation-pulse-573936/) | alex-morgan | 1:34 | Pulso electrónico intenso | Lanzamientos con energía | `alex-morgan-technology-future-innovation-pulse-573936.mp3` |

**Elegir una pista:**
- Tiene que durar **al menos lo que el video**. Si es más corta, elige otra; no la repitas en bucle (se nota el corte).
- Copia la elegida al proyecto como `assets/audio/musica.mp3` (el render solo lee archivos del proyecto) y anota su ID en `GUION.md`.
- Si la pista tiene una intro larga o un arranque fuerte, sáltalo con `data-media-start="<segundos>"`.
- Algunas pistas de Pixabay están registradas en Content ID de YouTube y pueden recibir un reclamo allí (se puede disputar con la licencia de Pixabay). En la web, Instagram o una intranet no suele pasar.

## 3. Efectos de sonido (incluidos en la skill)

En `assets/template/assets/sfx/`. Son **CC0 (dominio público)**: se pueden redistribuir, por eso sí van en el repositorio. El origen de cada uno está en `CREDITS.md`.

| Archivo | Cuándo suena | Volumen |
|---|---|---|
| `click.mp3` | En cada `click()` del cursor, en el mismo instante | 0,5 (narración: 0,35) |
| `click-seco.mp3` | Alternativa si la música es muy suave y el clic estándar sobra | 0,5 |
| `pop.mp3` | Aparece algo importante sin clic: un indicador, un punto de «sin guardar» | 0,4 |
| `exito.mp3` | El mensaje de confirmación de la app (alerta, toast) | 0,6 |
| `exito-largo.mp3` | La marca de verificación del cierre | 0,5 |
| `whoosh.mp3` | Barrido de la portada (intro, t 5,9) y del cierre (t 0) | 0,45 |

Las etiquetas, los rótulos de paso, los zooms y el scroll **no llevan sonido**: con clic, éxito y whoosh basta. Más efectos cansan en un tutorial de 60–90 s.

## 4. Volúmenes y mezcla

| Pista | ambiente | narración |
|---|---|---|
| Voz | — | 1,0 |
| Música en el cuerpo del tutorial | 0,35 | 0,12 (≈ −18 dB, cama bajo la voz) |
| Música en la intro y el cierre | 0,35 | 0,35 |
| Clic | 0,5 | 0,35 |
| Éxito | 0,6 | 0,5 |
| Whoosh | 0,45 | 0,45 |

- La música entra en **1 s** desde 0 y sale en **1,5 s** al final, con la línea de automatización de volumen (`data-automation`), no con un tween de GSAP (si hay las dos, gana la línea).
- En narración, la línea de la música sube a 0,35 en la intro, baja a 0,12 en 0,6 s justo antes de la primera frase, y vuelve a 0,35 en 0,6 s al empezar el cierre.
- Nunca dos efectos en el mismo instante. Si un clic dispara un éxito, el éxito va 0,3 s después.

## 5. Cómo se escribe el sonido en la composición

Todo el sonido son etiquetas `<audio>` **escritas en el HTML**, cada una con `id`, `src`, `data-start`, `data-volume` y `data-track-index`. HyperFrames solo mezcla los `<audio>` con `id`, y escritos en el HTML se pueden mover en Studio.

Los `<audio>` que van dentro de una escena (intro, tutorial, cierre) usan tiempos **locales a esa escena** y llevan además:
- `data-hf-media-start-basis="local"`, para que no se confundan con tiempos globales (`lint` avisa `nested_media_start_basis_ambiguous` si falta);
- `data-duration` con la duración del efecto (clic 0,3; pop 0,6; éxito 0,8; éxito largo 1,3; whoosh 0,9). Sin ella, HyperFrames lo cuenta hasta el final de la escena y `lint` avisa `duplicate_audio_track`.

```html
<audio id="sfx-click-4" data-sfx="click" src="assets/sfx/click.mp3" data-start="12.3" data-duration="0.3"
  data-hf-media-start-basis="local" data-volume="0.5" data-track-index="20"></audio>
```

| Dónde | Qué |
|---|---|
| `index.html` (root) | `<audio id="musica">` con su línea de volumen; dura todo el video |
| `compositions/intro.html` | El whoosh de la portada |
| `compositions/outro.html` | El whoosh y el éxito del cierre |
| `compositions/tutorial.html`, bloque `#sound` | Un `<audio data-sfx="click">` por cada `click()`, los éxitos, los pops y las voces de la narración. Tiempos locales a la escena del tutorial |

El kit guarda los tiempos de cada `click()` y, al construir la timeline, compara con los `<audio data-sfx="click">` del bloque `#sound`. Si falta alguno, `hyperframes check` lo reporta en sus hallazgos de runtime: `clics sin sonido en t = …`.

**Modo silencio:** borra el `<audio id="musica">`, los whoosh y éxitos de intro y cierre, y vacía el bloque `#sound`.

## 6. Modo narración

### 6.1 El agente escribe el guion de locución → `LOCUCION.md`
Usa la plantilla `assets/template/LOCUCION.md`. Por cada paso:
- **Texto** que se dice, en español neutro y en segunda persona («Abre…», «Marca…»). Acompaña la imagen; no la describe palabra por palabra.
- **Duración objetivo**, calculada a **2,3 palabras por segundo** (ritmo pausado de tutorial). Una frase de 18 palabras dura unos 8 s.
- **Pronunciación** de los nombres de la interfaz y de las siglas («cero cuatro, clasificación»; «DOMA» como palabra).
- **Archivo esperado:** `paso-01.mp3`, `paso-02.mp3`…, y `intro.mp3` y `cierre.mp3` si el texto también se narra.

La intro y el cierre pueden narrarse o no: si el guion los incluye, son una frase corta cada uno.

Entrega `LOCUCION.md` al usuario y explícale cómo grabarlo:
- Mejor un archivo por bloque, con los nombres de la plantilla. También se acepta un único archivo con todo, leído en orden.
- Formato MP3 o WAV, sin música (la música la pone el video).
- Un medio segundo de silencio antes y después de cada bloque.

### 6.2 La persona entrega el audio
La herramienta es suya: su voz, Kokoro (`npx hyperframes tts "<texto>" --voice ef_dora --speed 0.95`), HeyGen, ElevenLabs u otra. Copia los archivos a `assets/audio/voz/`.

### 6.3 El agente ajusta el video a la voz
1. **Mide** cada archivo (`ffprobe -v error -show_entries format=duration -of csv=p=0 paso-01.mp3`).
   - **Un solo archivo:** transcríbelo con `npx hyperframes transcribe voz.mp3 --model small --language es` (nunca un modelo `.en`, que traduce al inglés) y ubica el inicio de cada bloque por sus primeras palabras.
2. **Reajusta los tiempos:** cada paso dura `max(duración de su voz + 0,6 s, lo que necesitan sus acciones)`. La imagen sigue a la voz:
   - el zoom y el clic de una acción caen mientras se nombra esa acción, no antes;
   - el rótulo del paso entra con la primera palabra;
   - si la voz es más larga que las acciones, alarga las pausas entre ellas (sin congelar: el cursor descansa y la cámara se mantiene);
   - si las acciones son más largas, deja silencio al final del paso, nunca a mitad de una frase.
   Escribe la coreografía con tiempos relativos al inicio de cada paso (`const P2 = 7.6; camTo(P2 + 0.4, …)`), así reajustar es cambiar el inicio de cada paso.
3. **Coloca** un `<audio id="voz-paso-N" src="assets/audio/voz/paso-0N.mp3" data-start="<inicio del paso>" data-duration="<duración medida>" data-hf-media-start-basis="local" data-volume="1" data-track-index="30">` por paso en el bloque `#sound`, y las voces de intro y cierre en sus escenas.
4. **Baja la música** a cama (0,12) con su línea de volumen (sección 4).
5. **Revisa** en la vista previa que cada frase coincida con su acción, y que ningún clic tape una palabra (si coincide, mueve el clic 0,2 s o baja su volumen a 0,25).

## 7. Requisitos y problemas frecuentes

- **ffmpeg / ffprobe:** HyperFrames los necesita para leer cualquier audio. Sin ellos, `check` marca `audio_source_unreadable` («ffprobe not found») en cada pista y el render sale mudo o falla. Instálalos antes de trabajar con sonido (macOS con Apple Silicon: Homebrew nativo en `/opt/homebrew` y `brew install ffmpeg`), o renderiza con `--docker`.
- **`audio_src_not_found`:** la pista no se copió al proyecto (`assets/audio/musica.mp3`) o la ruta de la voz está mal.
- **Render mudo:** a algún `<audio>` le falta `id`, o se creó por script en vez de escribirse en el HTML.
- **La música tapa la voz:** el nivel bajo la narración es 0,12; si la pista es muy densa, elige una de las marcadas «bajo narración» en el catálogo.
