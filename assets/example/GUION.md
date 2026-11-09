# Guion: Cómo configurar Bodegas de clasificación en Suite - DOMA

Video tutorial de 1920×1080 a 60 fps, de unos 76 s. Simula una grabación de pantalla con cursor, pulso en cada clic, zooms de 160–180 % y el resto de la pantalla oscurecido.

## Correcciones sobre el pedido original (verificadas en SUITE)

| El pedido decía | La pantalla real dice (`resources/js/Pages/ProductionProcessSettings/Index.vue`) |
|---|---|
| Ruta: Inicio → Configuración Producción → Bodegas de clasificación | **⚙ Configuración DOMA (barra superior) → menú lateral: Compartido → Configuración → Configuración Producción → índice «Clasificación · Bodegas» → Bodegas Clasificación** |
| Crear una bodega con «Nuevo» / «Agregar» | **No se crean bodegas aquí.** La pantalla muestra una tarjeta por **tipo de bodega** (`código · nombre`, con cuántas bodegas trae cada uno) y se **marca la casilla** de los tipos que verán los clasificadores. |
| Diligenciar campos | **No hay campos de texto.** Solo hay casillas, así que no hay escritura letra a letra. |
| Guardar | Los botones **«Descartar»** y **«Guardar cambios»** van arriba de la tarjeta. Sin cambios pendientes muestran «No hay cambios por guardar» y quedan deshabilitados. |
| Mensaje de confirmación | Alerta de éxito: **«Éxito» · «Configuración guardada correctamente»**. |
| Verificar en el listado | Tras guardar, la tarjeta sigue marcada, desaparece el **punto azul** de «cambios sin guardar» de la pestaña y vuelve «No hay cambios por guardar». |

**Propósito (una frase):** define qué tipos de bodega se listan a los clasificadores, por ejemplo la bodega de clasificación. Si no se marca ningún tipo, clasificación no lista ninguna bodega y no se pueden asignar equipos.

**Datos de ejemplo** (ficticios, de la captura): empresa Santos; tipos 01 · Normal (sin bodegas), 02 · GALPÓN (72 bodegas), 03 · BODEGA (1 bodega) y 04 · CLASIFICACIÓN (1 bodega).

## Escenas

| # | Tiempo | Rótulo | Qué pasa | Zoom | Etiquetas (≤ 8 palabras) |
|---|---|---|---|---|---|
| 1 | 0–6 s | — | Intro con la marca DOMA. Título «Cómo configurar Bodegas de clasificación» y la línea «Define qué tipos de bodega ven los clasificadores». | — | — |
| 2 | 6–21 s | Paso 1: Abre Configuración Producción | El cursor va al ⚙ de la barra superior (clic) y luego al menú lateral: despliega **Configuración** y hace clic en **Configuración Producción**. Carga la página. | ⚙ 180 % · menú 170 % | «Configuración DOMA: el engranaje de arriba» · «Despliega Configuración en Compartido» · «Elige Configuración Producción» |
| 3 | 21–30 s | Paso 2: Abre Bodegas Clasificación | Se abre en «Bodegas Producción» (la primera pestaña). El índice baja hasta «Clasificación · Bodegas» y el cursor hace clic en **Bodegas Clasificación**. | índice 160 % · encabezado 160 % | «Baja hasta Clasificación · Bodegas» · «Define qué bodegas ve el clasificador» |
| 4 | 30–51 s | Paso 3: Marca los tipos de bodega | Explica las tarjetas y marca **04 · CLASIFICACIÓN**. Aparece el punto de cambios sin guardar. | tarjetas 145 % | «Código y nombre del tipo» · «Cuántas bodegas trae ese tipo» · «Atenuado: tipo sin bodegas» · «Ojo: Galpón sumaría sus 72 bodegas» · «Marca solo donde se clasifica» · «Sin marcar: clasificación no lista bodegas» |
| 5 | 51–65 s | Paso 4: Guarda los cambios | Señala el punto azul y «Descartar»; clic en **Guardar cambios** y alerta «Configuración guardada correctamente». | punto 180 % · botones 180 % · alerta 130 % | «Punto azul: cambios sin guardar» · «Descartar: vuelve a lo guardado» · «Mensaje de confirmación» |
| 6 | 65–70 s | Paso 5: Verifica la configuración | Vuelve «No hay cambios por guardar» y 04 · CLASIFICACIÓN queda marcada. | encabezado 160 % · tarjeta 170 % | «Sin cambios pendientes: quedó guardado» · «04 · CLASIFICACIÓN queda marcada» |
| 7 | 70–76 s | — | Cierre: «¡Listo! Ya configuraste tu bodega de clasificación.», el resumen «Marca los tipos y guarda los cambios.» y la marca DOMA · Suite empresarial. | — | — |

Antes de cada clic el cursor se detiene 0,5 s. Cada acción se ve con zoom y se vuelve a la vista general antes de cambiar de zona.
