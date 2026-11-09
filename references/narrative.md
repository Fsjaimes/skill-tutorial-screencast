# Narrativa explicativa

Un tutorial no es una demo: el espectador tiene que poder **repetir** lo que ve. Cada decisión de guion sirve a eso.

## Estructura
1. **Intro: el qué y el para qué** (≈6 s)
   - Título: «Cómo ‹verbo› ‹objeto›», partido en dos bloques. Ejemplo: «Cómo configurar» / «Bodegas de clasificación».
   - Una línea de propósito que responde «¿para qué sirve esto?» con el efecto que tiene en el trabajo de alguien. Si el código o la app tienen una descripción de la pantalla, adáptala: «Define qué tipos de bodega ven los clasificadores.»
2. **Pasos: el cómo** (3–6 pasos)
   - **Llegar:** la navegación desde un punto que el usuario reconoce (la barra superior o el menú lateral). Si la pantalla abre en otra pestaña o sección por defecto, se muestra y se navega desde ahí.
   - **Entender:** antes de actuar, una o dos etiquetas que explican qué muestra la pantalla (qué significa cada parte de una tarjeta o de un campo).
   - **Actuar:** el clic, la casilla o la escritura, con su reacción visible.
   - **Advertir:** una etiqueta de aviso (borde amarillo) sobre el error típico o la consecuencia de no hacerlo. Sale de los comentarios del código o de los textos vacíos de la pantalla.
   - **Guardar:** el botón real y el mensaje de confirmación literal.
   - **Verificar:** cómo saber que quedó bien (el estado guardado, el registro en el listado, el indicador que desaparece).
3. **Cierre: el logro** (≈7 s)
   - «¡Listo! Ya ‹logro›.» y un resumen de una línea con los verbos clave: «Marca los tipos y guarda los cambios.»

## Reglas de los textos
- **Rótulo de paso:** «Paso N: ‹imperativo› ‹objeto›», con 2–5 palabras después de los dos puntos.
- **Etiquetas:** como máximo 8 palabras, en tono cercano y de tú. No describas lo que el cursor ya muestra («Haz clic aquí»); di lo que el usuario necesita saber («Descartar: vuelve a lo guardado»).
- **Nombres de la UI:** tal cual aparecen, con sus mayúsculas, puntos medios y tildes («04 · CLASIFICACIÓN», «Guardar cambios»).
- **Datos de ejemplo:** ficticios pero realistas (empresa, códigos, cantidades) y coherentes en todos los fotogramas.

## Plantilla de guion (`GUION.md`)
```markdown
# Guion: Cómo ‹tarea› en ‹sistema›

Video tutorial de 1920×1080 a 60 fps, de unos N s.

## Correcciones sobre el pedido (verificadas en ‹fuente›)
| El pedido decía | La pantalla real dice |
|---|---|

**Propósito (una frase):** …
**Datos de ejemplo:** …

## Escenas
| # | Tiempo | Rótulo | Qué pasa | Zoom | Etiquetas (≤ 8 palabras) |
|---|---|---|---|---|---|
| 1 | 0–6 s | — | Intro | — | — |
| 2 | … | Paso 1: … | … | … | … |
| n | … | — | Cierre | — | — |
```

## Ejemplo de referencia: «Cómo configurar Bodegas de clasificación en Suite - DOMA»
Archivos completos en `assets/example/` (`GUION.md` y `compositions/tutorial.html`).

| Paso | Rótulo | Acciones | Etiquetas |
|---|---|---|---|
| Intro | — | Marca DOMA, «Cómo configurar» / «Bodegas de clasificación», propósito | — |
| 1 | Abre Configuración Producción | Zoom al ⚙ y clic · zoom al menú: despliega Configuración y clic en Configuración Producción · carga la página | «Configuración DOMA: el engranaje de arriba» · «Despliega Configuración en Compartido» · «Elige Configuración Producción» |
| 2 | Abre Bodegas Clasificación | Zoom al índice · scroll hasta «Clasificación · Bodegas» · clic · zoom al encabezado | «Baja hasta Clasificación · Bodegas» · «Define qué bodegas ve el clasificador» |
| 3 | Marca los tipos de bodega | Zoom a las tarjetas · se explican sus partes · aviso · clic en la casilla de 04 · aparece el punto de «sin guardar» | «Código y nombre del tipo» · «Cuántas bodegas trae ese tipo» · «Atenuado: tipo sin bodegas» · «Ojo: Galpón sumaría sus 72 bodegas» (aviso) · «Marca solo donde se clasifica» · «Sin marcar: clasificación no lista bodegas» (aviso) |
| 4 | Guarda los cambios | Zoom al punto azul · zoom a los botones (Descartar señalado) · clic en Guardar cambios · spinner · alerta «Éxito» | «Punto azul: cambios sin guardar» · «Descartar: vuelve a lo guardado» · «Mensaje de confirmación» |
| 5 | Verifica la configuración | Zoom al encabezado («No hay cambios por guardar») · zoom a la tarjeta marcada | «Sin cambios pendientes: quedó guardado» · «04 · CLASIFICACIÓN queda marcada» |
| Cierre | — | «¡Listo! Ya configuraste tu bodega de clasificación.» y «Marca los tipos y guarda los cambios.» | — |

Las correcciones de ese video son el modelo: el pedido pedía «Nuevo», campos y un listado, y la pantalla real usaba casillas por tipo, «Guardar cambios», una alerta y un indicador de cambios. El guion se ajustó a la pantalla real y lo dejó documentado.
