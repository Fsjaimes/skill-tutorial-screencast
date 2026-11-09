# Réplica de la interfaz

El objetivo es que un usuario real reconozca su pantalla. Fidelidad en la estructura, los textos y los estados; simplificación en los detalles que no se leen.

## Fuentes, en orden de preferencia
1. **Código fuente de la página.** Da los textos exactos, la estructura (qué se repite y con qué datos), los estados (`disabled`, indicadores, textos vacíos) y los mensajes. Búsquedas útiles:
   - el título que se ve: `grep -rIl "Configuración Producción" resources app`
   - las alertas: `grep -n "showAlert\|toast\|Swal\|success" Pagina.vue`
   - las pestañas o secciones: arrays de definición (`TABS`, `sections`, `menu`) con `title`, `hint` y `description`
   - el estado inicial: `activeTab: TABS[0].key`, el valor por defecto de los filtros
   - los tokens de color: `tokens.css` o `_variables.scss` del tema
2. **Captura de pantalla.** Da la disposición, las proporciones y los datos visibles. Úsala para medir.
3. **La app en vivo** (si el usuario da acceso). Sirve para capturar estados que no están en la captura (menú plegado, alerta).

## Medir y escalar
- Trabaja a 1920×1080. Escala las coordenadas de la captura por `1920 / anchoCaptura`.
- Anota las coordenadas de las zonas clave: barra superior (alto), menú lateral (ancho y `y` de cada ítem), contenedor de la página, tarjeta principal, botones de acción, lista o índice, panel.
- Guarda esas coordenadas como constantes en el script (`PAGE`, `CARD`, `panelX`) y deriva de ellas los objetivos del cursor y de la cámara. Así, si mueves una caja, todo la sigue.

## Construcción
- Todo con posiciones absolutas dentro de `#cam` (clase `.a`). Flexbox solo dentro de un componente (un botón o un ítem).
- **Listas repetidas** (pestañas, tarjetas, filas): un array de datos copiado de la fuente y un `forEach` que crea los nodos. Cada nodo que se va a animar lleva un `id` estable.
- **Iconos:** SVG en línea, simples, con `stroke: currentColor`, que se parezcan a los de la app (en DOMA, Remix Icons). Bastan unos pocos trazos; no hace falta la librería.
- **Esqueleto de la «pantalla anterior»:** bloques `#e6ebef` y `#eef2f5` cuando el tutorial arranca desde una pantalla que no conoces.
- **Estados:** construye el estado inicial en HTML o CSS y cambia con `fromTo` en la timeline. Para estados con varias capas (una tarjeta seleccionada), añade una capa `.tc-sel` con el estilo seleccionado y anima su `opacity`, en vez de cambiar clases.
- **Recortes y despliegues:** `clipPath: inset(...)` animado. Para listas con scroll, un contenedor con `overflow: hidden` y la lista con `y` animado.
- **Medidas que dependen del texto** (`offsetTop` de un ítem tras una lista de alto variable): mídelas dentro de `build()`, que corre después de cargar las fuentes.

## Patrones de la UI de DOMA (Velzon + doma-frontend)
| Pieza | Medidas a 1920 |
|---|---|
| Barra superior | 91 px de alto; bloque de marca de 327 px con borde derecho; marca «D» de 40 px con radio 7; botones de icono de 50×50 |
| Botón activo de la barra | fondo `--primary-soft` y borde `--primary-border` |
| Menú lateral | 327 px de ancho; cabecera de módulo de 90 px en `--light`; secciones a 14 px, 600, tracking 0.16em, en `--primary`; ítems de 46 px a 18 px y 500 |
| Submenú | sangría de 30 px, guía vertical punteada; ítem activo con fondo `--primary` y texto blanco |
| Encabezado de página | título en mayúsculas a 20 px y 600; migas a la derecha a 18 px |
| Tarjeta | radio 6, borde `--border`; cabecera de 101 px con título de 22 px y descripción de 18 px; botones de 50 px de alto a la derecha |
| Índice de pestañas | 343 px; rótulo de sección en mayúsculas a 15 px; ítem con icono, título de 18 px y pista de 15 px; activo con fondo `--primary-soft` |
| Tarjeta de opción con casilla | 528×106, radio 9; icono de 64 px en `--light`; casilla de 26 px |

Si la app es de otra marca o de otro framework, haz la misma tabla con sus medidas antes de construir.
