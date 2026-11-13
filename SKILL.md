---
name: tutorial-screencast
description: Produce videos tutoriales de software como grabación de pantalla simulada (HyperFrames, 1920×1080), con réplica fiel de la interfaz, cursor animado, pulso en cada clic, zooms con zona resaltada, rótulos «Paso N», etiquetas cortas, intro y cierre de marca. Úsala siempre que el usuario pida un video tutorial, un paso a paso, un «cómo configurar / cómo crear / cómo usar X» en un sistema o app, un video de onboarding, de capacitación o de soporte que enseñe a usar una pantalla, o diga «quiero un video como el de Bodegas de clasificación», aunque no diga «screencast». No la uses para anuncios, shorts promocionales, launch films ni motion graphics de marca, que son otros formatos.
compatibility: Requiere las skills de HyperFrames (npx skills add heygen-com/hyperframes; como mínimo hyperframes y hyperframes-cli), Node.js 22+ y ffmpeg (o Docker) para leer audio y exportar el MP4. La música de fondo sale de una biblioteca local de pistas de Pixabay (catálogo en references/audio.md).
---

# tutorial-screencast

Convierte el tema de un tutorial (o un guion borrador) en un video de 60–90 s que enseña a hacer una tarea en un software, de forma que quien lo vea pueda hacerla solo. El video es una **grabación de pantalla simulada**: la interfaz real se replica en HTML, una cámara hace zoom sobre cada acción, un cursor se mueve y hace clic, y unos rótulos explican cada paso. Lleva sonido en uno de tres modos: **ambiente** (música de fondo y efectos), **narración** (la voz que entrega la persona, a partir de un guion de locución que escribe el agente) o **silencio**. Todo es una composición HyperFrames determinista que se renderiza a MP4.

El video de referencia es **«Cómo configurar Bodegas de clasificación en Suite - DOMA»**, en `assets/example/`. Ábrelo cuando necesites ver cómo se resolvió algo en concreto.

## Antes de empezar

- Carga la skill `/hyperframes`: es el contrato de render (clips, `data-start` y `data-duration`, timelines pausadas, determinismo). Esta skill se apoya en él y no lo repite. **Si no está instalada**, díselo al usuario y pídele que la instale con `npx skills add heygen-com/hyperframes -g` (o sin `-g`, en el mismo alcance que esta skill) antes de seguir. Sin ese contrato, el video puede verse bien en la vista previa y fallar en el render.
- Comprueba la máquina con `npx hyperframes doctor`. Para exportar hacen falta ffmpeg o Docker; si no hay ninguno, el video se puede construir y previsualizar igual, pero avisa antes de llegar al render.
- Lee `references/audio.md` antes de poner sonido: modos, catálogo de música, volúmenes y el flujo de narración.
- Lee `references/style.md` antes de escribir movimiento. Tiene todos los valores (colores, tamaños, tiempos, curvas). El resumen de abajo es solo para orientarse.
- Copia `assets/template/` como punto de partida. Trae el kit de cámara, cursor, clic, resaltado, rótulos y etiquetas ya probado. No lo reescribas: adáptalo.

## Principios

1. **La pantalla real manda sobre el guion.** Los pedidos suelen traer una plantilla genérica («clic en Nuevo, llenar campos, guardar»). Antes de guionizar, verifica la pantalla real: capturas, código fuente o la app. En el ejemplo, el pedido hablaba de crear bodegas con un formulario, y la pantalla real era una selección por casillas de *tipos* de bodega, sin campos. Corrige la ruta, los nombres de menús, los botones y los mensajes, y documenta las correcciones en el guion. *Por qué:* un tutorial que muestra botones que no existen enseña mal y le resta credibilidad a todo el video.
2. **No inventes interfaz.** Si un elemento no aparece en la captura ni en el código, no lo dibujes. Usa un esqueleto gris (la «pantalla anterior») o señálalo con una etiqueta en vez de simular su contenido. Los datos de ejemplo sí pueden ser ficticios, pero que sean realistas y coherentes (las sumas cuadran y los conteos coinciden).
3. **Una cámara continua.** Todo el tutorial es una sola escena con una cámara (`#cam`) que se acerca y se aleja. Ese movimiento es lo que da la sensación de grabación de pantalla. La intro y el cierre son escenas aparte que la tapan o la destapan con un barrido.
4. **Una acción, un foco.** Cada acción importante se hace con zoom y zona resaltada. Antes de pasar a otra región de la pantalla se vuelve a la vista general, para que el espectador nunca pierda dónde está.
5. **El cursor anticipa.** El cursor llega al objetivo, se detiene 0,5 s y luego hace clic. El espectador tiene que poder ver adónde va antes de que pase algo.
6. **Pocas palabras, en el sitio.** Cada etiqueta tiene 8 palabras como máximo y está pegada al elemento con una flecha. El rótulo «Paso N» resume el paso. No hay narración en pantalla que repita lo que el cursor ya muestra.
7. **El estado sigue al clic.** El cambio de interfaz (casilla marcada, menú desplegado, botón habilitado, punto de «sin guardar») ocurre en el mismo instante del clic, más 0,05 s, y con la duración de una UI real (0,12–0,2 s).

## Sistema de estilo (resumen; valores completos en `references/style.md`)

| Elemento | Valor clave |
|---|---|
| Formato | 1920×1080, render a 60 fps, duración total 60–90 s |
| Tipografía | La de la marca (en DOMA, Saira 400/500/600). Rótulos y etiquetas en 600 a 28 px, título de intro 76 px. |
| Colores | Los tokens de la app replicada. Lo que añade el video: HUD en tinta `#16202a`, acento = el primario de la marca, aviso `#f7b84b`, oscurecido `rgba(15,25,35,.34)` |
| Zoom | 1,3× (modal) · 1,45× (grupo de tarjetas) · 1,6× (columna o encabezado) · 1,7× (menú) · 1,8× (botón o icono). 1,0 s con `power2.inOut`, recortado a los bordes. |
| Zona resaltada | Rectángulo de radio 14 que oscurece el resto. Entra en 0,6 s y sale en 0,5 s, siempre junto con el zoom. |
| Cursor | Flecha de 30×40 en tinta con borde blanco, dentro de la cámara. Recorrido de `0,35 + dist/1400` s (entre 0,5 y 1,1 s) con `power2.inOut`. |
| Clic | Pulsación a 0,86 de escala (0,08 s) y suelta (0,14 s), más un anillo de 64 px en el primario que crece de 0,25 a 1,5 y se desvanece en 0,55 s. |
| Etiqueta | Píldora blanca con borde izquierdo de 6 px y flecha. Entra en 0,4 s con `expo.out`, se lee 2,1–2,8 s y sale en 0,3 s. |
| Rótulo de paso | Píldora oscura abajo a la izquierda (48 px, 44 px) con la insignia «Paso N». Entra en 0,5 s y se queda todo el paso. |
| Intro / cierre | 6 s / 7 s. La portada sube (`yPercent` -100, 0,6 s, `power3.inOut`) y el cierre sube desde abajo. |
| Sonido | Modo ambiente por defecto: música a 0,35 con entrada de 1 s y salida de 1,5 s, clic a 0,5, éxito a 0,6, whoosh a 0,45. En narración: voz a 1,0 y música a 0,12. |

## Flujo de trabajo

Cada paso deja un archivo o una comprobación hecha. No avances con dudas sobre la interfaz real.

### 1. Recibir el tema → aclarar lo mínimo
De la petición saca la tarea, la audiencia, la ruta en el sistema, la marca, la duración y el **modo de audio** (ambiente, narración o silencio). Si falta la **pantalla real** (una captura, el código o un enlace), pídela: es lo único imprescindible. Lo demás se infiere: audiencia básica, 60–90 s, tono cercano y didáctico, español si el pedido está en español, y modo **ambiente** si no se habla de sonido.

### 2. Investigar la interfaz real
- Con **código fuente** (por ejemplo, un proyecto Laravel + Vue): busca la página (`grep` del título que se ve en pantalla) y toma de ahí los nombres exactos de menús, pestañas, pistas, botones, textos vacíos, estados (deshabilitado, «sin cambios», indicadores) y el **mensaje de confirmación** literal (busca `showAlert`, `toast`, `Swal`, `success`). Los comentarios del código suelen explicar *para qué* sirve cada opción: úsalos para las etiquetas.
- Con **captura**: mide las coordenadas y la escala (si la captura mide 2000 px de ancho, multiplica por 0,96 para llevarla a 1920). Anota los textos tal cual aparecen.
- Fíjate en qué abre por defecto la pantalla (la primera pestaña, por ejemplo): el tutorial tiene que llevar al usuario desde ahí.
Lee `references/ui-replica.md` para las técnicas de réplica.

### 3. Escribir el guion → `GUION.md`
Usa la plantilla `assets/template/GUION.md`. La estructura narrativa está en `references/narrative.md`:
- **Intro** (≈6 s): título «Cómo ‹tarea›» y una sola línea de propósito.
- **Pasos** (3–6, de 8 a 22 s cada uno): cada uno con su rótulo «Paso N: ‹verbo› ‹objeto›», sus acciones, su zoom y sus etiquetas.
- **Cierre** (≈7 s): «¡Listo! Ya ‹logro›.» más un resumen de una línea y la marca.
Incluye la tabla de **correcciones** (el pedido contra la pantalla real), los datos de ejemplo y la línea de **audio** (modo y pista). Enséñale el guion al usuario si hubo correcciones importantes; si el pedido era claro, sigue.

**En modo narración**, escribe también `LOCUCION.md` (plantilla en `assets/template/LOCUCION.md`, reglas en `references/audio.md` §6): el texto de cada paso a 2,3 palabras por segundo, la duración objetivo y la pronunciación de los nombres de la interfaz. Entrégaselo al usuario para que lo convierta en audio con la herramienta que prefiera, y sigue construyendo el video mientras tanto: la coreografía se reajusta a la voz cuando llegue.

### 4. Crear el proyecto
```bash
cp -R <skill>/assets/template <carpeta-de-videos>/proyectos/AAAA-MM-tutorial-<tema>
```
Ajusta las duraciones en `index.html` (intro 0–6,6, tutorial desde 6, cierre en la duración del tutorial + 5,6, y la `data-duration` y la línea de volumen de la música al total) y los textos de `intro.html` y `outro.html`. Si la marca no es DOMA, cambia la fuente en `assets/fonts/` y los tokens.

Copia la pista de música elegida desde la biblioteca local (`TUTORIAL_AUDIO_DIR` o `~/Desktop/PROYECTOS/videos/audio/musica/`) a `assets/audio/musica.mp3`. Si no la encuentras, pide la ruta o da el enlace de Pixabay del catálogo. En modo silencio, borra los `<audio>` de la plantilla.

### 5. Replicar la interfaz en `compositions/tutorial.html`
Dentro de `#cam`, construye la pantalla a 1920×1080 con posiciones absolutas: barra superior, menú lateral, página. Las listas repetidas (pestañas, tarjetas, filas) se generan con JS a partir de arrays de datos copiados de la fuente. Todo estado que vaya a cambiar empieza en su valor inicial, y el cambio se hace con tweens.

### 6. Coreografiar con el kit
Escribe las acciones paso a paso usando los helpers del template: `camTo`, `wide`, `spot`, `move`, `click`, `type`, `step` y `label`. El patrón de una acción es siempre el mismo (los tiempos son locales al tutorial):
```js
camTo(t, cx, cy, 1.7);                 // 1. zoom a la región (1,0 s)
spot(t, tFin, x, y, w, h);             //    con la zona resaltada
move(t + 0.2, objX, objY);             // 2. el cursor viaja (0,5–1,1 s)
click(t + 1.7);                        // 3. pausa de 0,5 s tras llegar y clic
tl.fromTo(estado, {...}, {...}, t + 1.75); // 4. la UI reacciona en 0,05 s
label(t + 1.9, t + 4.3, "≤ 8 palabras", ax, ay, "right");
wide(tFin);                            // 5. vuelta a la vista general
```
Las etiquetas calculan su posición en pantalla desde la cámara activa, así que llama a `label()` *después* del `camTo()` de esa toma.

### 7. Intro y cierre
Edita solo los textos y la marca. Mantén los tiempos del template, que ya están calibrados.

### 8. Sonido
Sigue `references/audio.md`. Todo el sonido son `<audio>` **escritos en el HTML**, con `id`. HyperFrames no mezcla los que no tienen `id`, y escritos a mano se pueden mover en Studio.
- **Ambiente:** la plantilla ya trae la música con su línea de volumen y el whoosh y el éxito de la intro y el cierre. Agrega en el bloque `#sound` de `tutorial.html` un `<audio data-sfx="click">` por cada `click()` (mismo instante, volumen 0,5) y un `exito` cuando aparezca el mensaje de confirmación de la app.
- **Narración:** cuando la persona entregue el audio, mide cada archivo (o transcríbelo si viene en uno solo), reajusta el inicio y la duración de cada paso a su frase, coloca un `<audio id="voz-paso-N">` por paso y baja la música a 0,12 con su línea de volumen. La imagen sigue a la voz.
- **Silencio:** sin `<audio>`.
El kit compara los `click()` con los `<audio data-sfx="click">`. Si falta alguno, `hyperframes check` lo reporta en sus hallazgos de runtime: `clics sin sonido en t = …`.

### 9. Revisar
1. `npx hyperframes check --json`: corrige todos los errores. Un `audio_source_unreadable` con «ffprobe not found» no es un error del video sino de la máquina: falta ffmpeg (ver `references/audio.md` §7). Los avisos de contraste sobre la marca real (texto blanco sobre el primario) y los de texto pequeño de la UI replicada son esperables; anótalos.
2. `npx hyperframes snapshot --at <t1,t2,…>`: un fotograma por acción (en el zoom, con la etiqueta visible, y en el clic +0,2 s), más la intro, una transición y el cierre. Revisa la hoja de contacto con la checklist de abajo.
3. Corrige y vuelve a sacar los fotogramas de lo que cambiaste.
4. Con sonido, confirma que `check` no reporte `clics sin sonido` y escucha el video completo una vez en la vista previa: que la música no tape la voz y que cada clic suene con su pulso.

### 10. Vista previa
`npx hyperframes preview --background`, y abre la URL en el panel del navegador para que el usuario la vea y pueda pedir ajustes.

### 11. Exportar
Renderiza solo cuando el usuario apruebe o pida el archivo:
```bash
npx hyperframes render --fps 60 --quality delivery -o renders/<nombre>.mp4
```
Si falta ffmpeg en la máquina, usa `--docker` (necesita Docker corriendo) o pide al usuario que lo instale. Entrega la ruta del MP4 y su duración.

## Errores conocidos y cómo evitarlos

- **Fantasmas de texto o estado.** Un `tl.to()` guarda su valor inicial la primera vez que se dibuja. Si la vista previa salta en la línea de tiempo, ese valor puede quedar mal y dejar, por ejemplo, la palabra anterior semitransparente. Usa siempre `fromTo` con los valores explícitos de inicio y fin e `immediateRender: false` en los que no son el primero de su propiedad. Para elementos que solo existen un rato, fuerza también `opacity: 0` antes y después de su ventana.
- **Medir después de las fuentes.** Las posiciones que salen del layout (`offsetTop` de una pestaña, el ancho de un texto) se miden cuando la fuente ya cargó. El template construye la timeline dentro de `document.fonts.load(...).then(build)`.
- **La zona resaltada tiene que cubrir lo que se mira.** Si un submenú se despliega dentro de la toma, dimensiona el resaltado para el estado desplegado. Si tras un clic aparece algo en otra parte (un mensaje o una alerta), cierra el resaltado anterior y abre uno nuevo sobre eso; si no, lo importante queda oscurecido.
- **La timeline se construye dentro de `build()`**, después de cargar las fuentes, y termina con `window.__hfForceTimelineRebind()` para que HyperFrames tome la timeline que se registró tarde (ya está así en el template).
- **Nada de template literals en `querySelector`.** El bundler de HyperFrames falla con `querySelector(\`#x${i}\`)`. Usa `getElementById("x" + i)` o una concatenación. En los selectores de GSAP sí se pueden usar.
- **No animes `left`, `top`, `width` ni `height`.** Para desplegar, recortar o hacer scroll usa `clipPath` y transforms (`x`, `y`, `scale`).
- **Audio creado por script.** Un `<audio>` que crea el JavaScript de la composición puede no entrar en la mezcla del render y no aparece en Studio. Escribe todo el sonido en el HTML.
- **La música no viaja con la skill.** La licencia de Pixabay no permite redistribuir las pistas tal cual, así que no están en el repositorio. Cópialas desde la biblioteca local del usuario; si no la tiene, dale los enlaces del catálogo.
- **El cursor va dentro de la cámara** (crece con el zoom, como en una grabación real). Los rótulos y las etiquetas van fuera (en `#hud`), para que tengan siempre el mismo tamaño en pantalla.

## Checklist de calidad

Revisa cada video nuevo contra esta lista antes de mostrarlo. Si algo no se cumple, corrígelo o explica por qué no aplica.

**Contenido**
- [ ] La ruta, los menús, los botones y el mensaje de confirmación coinciden con la interfaz real, y las correcciones al pedido están en `GUION.md`.
- [ ] No hay ningún elemento de interfaz inventado; lo desconocido es un esqueleto o una etiqueta.
- [ ] Los datos de ejemplo son realistas y coherentes entre sí.
- [ ] La intro dice para qué sirve la tarea en una línea, y el cierre dice «¡Listo! Ya ‹logro›.» más un resumen.

**Ritmo**
- [ ] La duración total está entre 60 y 90 s; la intro dura unos 6 s y el cierre unos 7 s.
- [ ] Cada paso dura entre 8 y 22 s y lleva su rótulo «Paso N: verbo + objeto».
- [ ] Cada etiqueta se lee al menos 2 s y nunca hay dos a la vez.
- [ ] Tras cada clic la UI reacciona en ≤ 0,05 s.

**Cámara y cursor**
- [ ] Cada acción importante tiene un zoom de 1,3–1,8× con zona resaltada y vuelve a la vista general antes de cambiar de región.
- [ ] El cursor se mueve suave, sin saltos, y se detiene 0,5 s antes de cada clic.
- [ ] Cada clic tiene su pulso; el objetivo del clic queda dentro de la zona resaltada.
- [ ] Lo que señala cada etiqueta está dentro de la zona resaltada activa; cuando el foco pasa a otro elemento (del botón al mensaje), el resaltado pasa con él.

**Texto y estilo**
- [ ] Las etiquetas tienen ≤ 8 palabras, están junto al elemento con flecha y no tapan la acción ni el cursor.
- [ ] El rótulo de paso no tapa la acción (abajo a la izquierda).
- [ ] La fuente y los colores son los de la marca, con la marca en la intro y en el cierre.

**Sonido**
- [ ] El modo de audio (ambiente, narración o silencio) y la pista están anotados en `GUION.md`.
- [ ] La pista dura al menos lo que el video y entra y sale con su línea de volumen (1 s y 1,5 s).
- [ ] Cada `click()` tiene su `<audio data-sfx="click">` (`check` no reporta clics sin sonido), y el mensaje de confirmación tiene su `exito`.
- [ ] En narración: `LOCUCION.md` entregado; cada paso empieza con su frase y dura al menos lo que ella; la música está a 0,12 bajo la voz.
- [ ] Todos los `<audio>` tienen `id` y están escritos en el HTML.

**Técnica**
- [ ] `hyperframes check` no tiene errores, salvo los esperables, que están anotados.
- [ ] Revisaste la hoja de contacto con un fotograma por acción y no hay fantasmas, saltos ni estados a medias.
- [ ] Todos los cambios de estado usan `fromTo` y nada depende del orden de reproducción.
