# Estilo: valores concretos

Todos los números salen del video de referencia (`assets/example/`). Son el punto de partida; cambia alguno solo si tienes un motivo y anótalo en el guion.

## Contenido
1. Formato y lienzo
2. Color
3. Tipografía
4. Cámara y zoom
5. Zona resaltada
6. Cursor
7. Clic
8. Escritura en campos
9. Cambios de estado de la UI
10. Etiquetas
11. Rótulos de paso
12. Alerta de confirmación
13. Intro
14. Cierre
15. Ritmo y tiempos por escena
16. Curvas (easing)

## 1. Formato y lienzo
- 1920×1080. Se renderiza a 60 fps (`--fps 60`), porque a 30 fps el cursor y los zooms se ven a saltos.
- Duración total: 60–90 s. Referencia: 78,6 s.
- Root: `tutorial` (desde 6 s), `intro` (0–6,6 s, encima) y `outro` (desde el final del tutorial − 0,8 s, encima).
- Si la captura no mide 1920 de ancho, escala sus coordenadas: `x1920 = x · 1920 / anchoCaptura`. La referencia era de 2000 px, así que el factor fue 0,96.

## 2. Color
**Tokens de la app replicada.** Cópialos de su CSS. En DOMA vienen de `doma-frontend/styles/tokens.css`:

| Token | Valor | Uso |
|---|---|---|
| `--primary` | `#25a0e2` | botón primario, ítem activo, casilla marcada, pulso del clic, borde de etiqueta |
| `--primary-soft` | `#def1fb` | fondo del ítem activo y de la tarjeta seleccionada |
| `--primary-border` | `#a8d9f3` | borde del botón activo de la barra |
| `--primary-ink` | `#1f88c0` | texto sobre el fondo suave |
| `--heading` | `#495057` | títulos de la UI |
| `--text` | `#212529` | texto |
| `--muted` | `#878a99` | texto secundario |
| `--border` | `#e9ebec` | bordes |
| `--light` | `#f3f6f9` | fondos claros y botón secundario |
| `--success` | `#0ab39c` | icono de la alerta de éxito |
| aviso | `#f7b84b` | borde de las etiquetas de advertencia |

**Lo que añade el video:**

| Elemento | Valor |
|---|---|
| Fondo de intro y cierre | `#f4f7f9` |
| Tinta del HUD (rótulo de paso, título) | `#16202a` |
| Texto secundario de la intro y el cierre | `#5d6874` |
| Oscurecido de la zona resaltada | `rgba(15, 25, 35, 0.34)` |
| Fondo de la alerta | `rgba(33, 37, 41, 0.45)` |
| Esqueleto de la «pantalla anterior» | `#e6ebef` y `#eef2f5` |
| Sombra de etiquetas y rótulos | `0 16px 40px rgba(0,0,0,.22–.25)` |

Si la marca es otra, sustituye `--primary` y la familia de colores. La estructura no cambia.

## 3. Tipografía
La fuente de la marca, local en `assets/fonts/` con `@font-face`. En DOMA es Saira 400/500/600.

| Elemento | Tamaño | Peso | Otros |
|---|---|---|---|
| Título de intro | 76 px | 600 | tracking −0.015em, interlineado 1.08 |
| Subtítulo de intro | 34 px | 400 | `#5d6874` |
| Píldora «Tutorial» | 22 px | 600 | 44 px de alto, fondo `--primary-soft` |
| Lockup de intro | marca 64 px · palabra 34 px · tagline 24 px | 600 | palabra con tracking 0.16em |
| Rótulo de paso | 28 px | 600 | insignia de 22 px |
| Etiqueta | 28 px | 600 | `white-space: nowrap` |
| Título de cierre | 68 px | 600 | |
| Resumen de cierre | 34 px | 400 | |
| UI replicada | los tamaños de la app (15–22 px a 1920) | | no se agrandan: el zoom los hace legibles |

## 4. Cámara y zoom
`#cam` envuelve toda la pantalla replicada: `transform-origin: 0 0`, animando `x`, `y` y `scale`.

Para centrar un punto (cx, cy) con un zoom z: `x = 960 − cx·z`, `y = 540 − cy·z`, recortado a `[1920 − 1920z, 0]` y `[1080 − 1080z, 0]` para no salirse del borde.

| Qué se enfoca | Zoom | Ejemplo de referencia |
|---|---|---|
| Un botón o icono pequeño de la barra | 1,8× | engranaje ⚙, punto de «sin guardar», botones Guardar y Descartar |
| Un menú lateral o un grupo de ítems | 1,7× | Compartido → Configuración |
| Una columna, un encabezado o una tarjeta | 1,6–1,7× | índice de pestañas, encabezado del panel, tarjeta marcada |
| Un grupo de tarjetas o un formulario | 1,45× | las 4 tarjetas de tipos |
| Un modal o una alerta | 1,3× | alerta «Éxito» |

- Cada zoom dura **1,0 s** con `power2.inOut`. Hacia una alerta que acaba de aparecer, 0,8 s.
- Entre dos regiones distintas de la pantalla, la cámara vuelve a la vista general (`wide`, 1,0 s). Dentro de la misma región puede ir de un foco a otro sin pasar por la vista general (por ejemplo, del punto azul a los botones).
- El zoom se mantiene durante toda la acción y sus etiquetas.

## 5. Zona resaltada
- `div.spot` en coordenadas de la app (dentro de `#cam`), con `border-radius: 14px` y `box-shadow: 0 0 0 4000px rgba(15,25,35,.34)`.
- Entra con el zoom (`opacity` de 0 a 1 en 0,6 s, `power2.inOut`) y sale cuando empieza el siguiente movimiento de cámara (0,5 s).
- Margen de 8–14 px alrededor del elemento. Si algo se despliega dentro de la toma, dimensiona para el estado final.

## 6. Cursor
- Flecha SVG de 30×40: `M2 2 L2 31 L9.5 24 L14.5 36 L19.5 34 L14.5 22.5 L25 22.5 Z`, relleno `#16202a`, borde blanco de 2,4 px. El punto activo es la punta (2, 2).
- Va dentro de `#cam`, así que crece con el zoom igual que en una grabación real.
- Recorrido: `dur = clamp(0.35 + distancia/1400, 0.5, 1.1)` s con `power2.inOut`, en línea recta (la curva de velocidad ya lo hace natural).
- Posición inicial: cerca del centro de la página (1150, 640).
- Llega al objetivo y **se detiene 0,5 s** antes del clic.
- Cuando el cursor solo señala, sin hacer clic (para explicar), se apoya junto al elemento y no encima de su texto: unos 40–80 px a un lado.
- Al final de un paso se aparta del área que se va a leer (por ejemplo, 140 px más abajo) para no tapar el resultado.

## 7. Clic
- Pulsación: `#cursor-in` pasa a escala 0,86 en 0,08 s (`power2.out`) y vuelve a 1 en 0,14 s.
- Pulso: un anillo `div.rip` de 64 px en el punto del clic, con borde de 4 px en `--primary` y relleno `rgba(37,160,226,.18)`. Crece de 0,25 a 1,5 de escala con opacidad de 0,95 a 0, en 0,55 s con `power2.out`.
- La reacción de la UI empieza 0,05 s después del clic.

## 8. Escritura en campos
- Letra a letra, a **12 caracteres por segundo** (legible), escribiendo `textContent` con `Math.round` en `onUpdate` para que sea seguro aunque la vista previa salte en la línea de tiempo.
- Caret de 2 px en `--text` que parpadea cada 0,5 s mientras el campo tiene foco (con `tl.set` alternos, nunca con `repeat: -1`).
- Antes de escribir: clic en el campo, que muestra el borde de foco en `--primary`, y 0,3 s de pausa.
- Zoom de 1,6–1,8× sobre el campo mientras se escribe, con una etiqueta de qué significa el campo y el valor de ejemplo.

## 9. Cambios de estado de la UI
| Cambio | Tiempo | Técnica |
|---|---|---|
| Casilla, ítem activo, color de texto | 0,12–0,2 s | `fromTo` de `backgroundColor`, `color` u `opacity` |
| Indicador que aparece (un punto) | 0,25 s con `back.out(2)` | `scale` de 0,4 a 1 |
| Menú que se despliega | 0,45 s con `power2.out` | `clipPath: inset(0 0 H 0)` a `inset(0)` |
| Chevron | 0,3 s con `power2.inOut` | `rotation` de 0 a 90 |
| Scroll de una lista | 1,2 s con `power2.inOut` | `y` de la lista; recorte con `overflow: hidden` en el contenedor |
| Carga de página | 0,25 s de salida del esqueleto y 0,35 s de entrada del contenido | `opacity` |
| Botón con spinner | 0,55 s | icono → spinner que gira 360° → icono |
| Botones habilitados o deshabilitados | 0,15–0,2 s | `opacity` de 0,65 a 1 |

## 10. Etiquetas
- Píldora blanca: radio 12, padding de 14 px por 24 px, `border-left` de 6 px en `--primary` (o `#f7b84b` si es advertencia), sombra.
- Flecha: un cuadrado blanco de 18 px girado 45°, del lado del elemento.
- Se colocan a **30 px** del punto señalado, en `below`, `above`, `left` o `right`. Se calculan en coordenadas de pantalla con el estado de la cámara de esa toma.
- Entrada: `opacity` de 0 a 1 y un deslizamiento de 10 px, en 0,4 s con `expo.out`. Salida: 0,3 s con `power2.in`.
- Lectura de **2,1–2,8 s**, nunca menos de 2 s. Una sola etiqueta a la vez.
- Como máximo 8 palabras. Dos patrones: «‹Qué es›: ‹qué hace›» («Punto azul: cambios sin guardar») o un imperativo («Marca solo donde se clasifica»).

## 11. Rótulos de paso
- Abajo a la izquierda (`left: 48px; bottom: 44px`), fuera de la cámara.
- Píldora `#16202a` de 68 px de alto, con la insignia «Paso N» en `--primary` (46 px de alto) y el texto en blanco a 28 px y 600.
- Texto: «‹Verbo en imperativo› ‹objeto›» («Abre Configuración Producción», «Guarda los cambios»).
- Entra 0,2–0,4 s después de que empieza el paso (0,5 s con `expo.out`, desde y +24 px) y sale al terminar el paso (0,35 s).

## 12. Alerta de confirmación
Réplica del mensaje real de la app. En DOMA es SweetAlert:
- Fondo con `opacity` de 0 a 1 en 0,25 s.
- Modal de 512×376, radio 8, centrado, con `scale` de 0,7 a 1 en 0,35 s y `back.out(1.5)`.
- Icono: círculo de 96 px con borde `rgba(10,179,156,.3)` y la marca de verificación dibujada (`strokeDashoffset` de 44 a 0, 0,4 s) 0,3 s después.
- Título de 32 px y texto de 21 px en `#545454`, con el texto literal del código.
- Visible unos 3 s (la app real dura 1,5 s; en video se alarga para poder leerlo), con zoom de 1,3× y la etiqueta «Mensaje de confirmación» debajo.

## 13. Intro (6,6 s)
| t | Qué pasa |
|---|---|
| 0 | El lockup de la marca ya está en pantalla (fotograma 0 compuesto) y se asienta 8 px en 0,6 s con `expo.out`. |
| 0,4 | Entra la píldora «Tutorial» (0,5 s). |
| 0,7 | El título entra en 2 bloques («Cómo ‹verbo›» y «‹objeto›»), 0,18 s entre ellos, 0,6 s cada uno con `expo.out` desde y +30 px. |
| 1,8 | Entra la línea de propósito (0,6 s). |
| 1,9–5,9 | Empuje lento a escala 1,015. |
| 6,0 | La portada sube y destapa la interfaz (`yPercent` -100, 0,6 s, `power3.inOut`). |

## 14. Cierre (7 s)
| t | Qué pasa |
|---|---|
| 0 | El panel sube desde abajo (`yPercent` de 100 a 0, 0,6 s, `power3.inOut`). |
| 0,6 | El círculo de verificación en `--primary` aparece (`scale` de 0,6 a 1, `back.out(1.6)`, 0,45 s) y se dibuja la marca (0,4 s). |
| 1,0 | «¡Listo!» y «Ya ‹logro›.» con 0,25 s entre ellos, 0,6 s cada uno. |
| 2,0 | Resumen de una línea. |
| 2,6 | Lockup de la marca abajo (a 110 px del borde) y empuje lento a escala 1,02. |

## 15. Ritmo y tiempos por escena
| Bloque | Duración |
|---|---|
| Intro | 6 s |
| Navegación (llegar a la pantalla) | 12–15 s |
| Abrir el apartado | 8–10 s |
| Paso central (configurar o diligenciar) | 18–25 s |
| Guardar y confirmación | 12–14 s |
| Verificar | 6–8 s |
| Cierre | 7 s |

**Una acción tipo dura 4–6 s:** zoom de 1,0 s, recorrido de 0,5–1,1 s, pausa de 0,5 s, clic, reacción de 0,2 s, etiqueta de 2,1–2,8 s y vuelta de 1,0 s.
**Una explicación sin clic dura 2,6–2,8 s:** el cursor señala y la etiqueta se lee.
Deja 0,2–0,4 s de aire entre etiquetas consecutivas.

## 16. Curvas (easing)
| Uso | Curva |
|---|---|
| Cámara, cursor, zona resaltada, scroll | `power2.inOut` |
| Entradas de texto (etiquetas, rótulos, títulos) | `expo.out` |
| Salidas de texto | `power2.in` |
| Despliegues y reacciones | `power2.out` |
| Barridos de intro y cierre | `power3.inOut` |
| Elementos que aterrizan (indicador, alerta, verificación) | `back.out(1.5–2)` |
| Empujes lentos | `none` |
