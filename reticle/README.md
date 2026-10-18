# Reticle v1.12.0 — Entrenador de Aim
_created by gK1wi_

5 archivos: `index.html`, `manifest.json`, `service-worker.js`, `icon-192.png`, `icon-512.png`. Súbelos juntos, en la misma carpeta/raíz del repo — no muevas ni renombres ninguno, el service worker y el manifest los referencian por nombre.

## Changelog
- **v1.12.0** — Rediseño de la pantalla de inicio y de todas las demás para que compartan un solo lenguaje visual, de instrumento/HUD en lugar de panel genérico. **Inicio:** barra de estado superior con las tres métricas (mismo componente que el HUD del juego: valor arriba, etiqueta abajo, divisores finos); columna de controles a la izquierda y, a la derecha, un retículo grande dibujado en SVG con graduaciones calculadas (el anillo exterior gira una vuelta cada 90 s; se desactiva con `prefers-reduced-motion`). La lista de modos pasó de tres tarjetas idénticas a filas con separadores finos que muestran la descripción solo al pasar el cursor o seleccionarlas. **Teclado:** `1`/`2`/`3` eligen el modo, `Enter` empieza (o repite desde resultados), `Esc` sale de la partida o vuelve al menú. Enter se ignora cuando un botón ya tiene el foco para no contar dos sesiones. **Forma:** se quitaron las esquinas tipo corchete que se repetían en cada panel; ahora hay una sola forma, una esquina cortada (`clip-path`), usada en el botón principal y en los paneles de resultados/bloqueo. Los resultados pasan de cuatro cajitas a una tabla con líneas internas. **Tipografía:** Oswald (condensada) para títulos, modos y botones; Inter solo para texto de apoyo; JetBrains Mono solo para números. Los modos y duraciones ahora son `<button>` reales (foco visible con teclado). Misma paleta blanco/negro/gris con rojo solo en fallos y bloqueo. Sin cambios de lógica de juego, puntaje, límite de sesiones ni formato de `localStorage` — tu progreso se conserva. Íconos y `manifest.json` ajustados al nuevo negro (`#070707`).
- **v1.11.0** — Rediseño visual completo a blanco/negro/escala de grises. Se quitó el acento teal y el ámbar de toda la app (menú, HUD, canvas, resultados); el blanco pasa a ser el único "color vivo" (blancos activos, puntaje, selección, botón principal) y el rojo queda reservado exclusivamente para lo que de verdad es un fallo o un corte: el contador de fallos en el HUD, el flash de clic fallido en el canvas, y la pantalla de bloqueo de sesiones (tag, bordes de la esquina y el cronómetro). El emoji 🔓 del banner de "nivel desbloqueado" se reemplazó por un ícono SVG en `currentColor` — el emoji nativo se renderizaba en amarillo y rompía la paleta sin que el CSS pudiera controlarlo. `manifest.json` también se actualizó (`background_color`/`theme_color` a `#0a0a0a`) para que la pantalla de carga/instalación de PWA coincida. `icon-192.png` e `icon-512.png` (el retículo teal con punto ámbar) se recolorearon al mismo blanco/negro — quedaban fuera del CSS por ser PNGs ya renderizados, así que se remapeó el color de cada píxel preservando el antialiasing de los bordes en vez de regenerar el ícono desde cero. Cero cambios de lógica o de rendimiento — mismo HTML/JS salvo las clases y el ícono del candado.
- **v1.10.1** — Límite de 20 sesiones
- **v1.10.0** — Límite de 60 sesiones: cada clic en "Empezar sesión" (o "Repetir") suma al contador, sin importar el modo o la duración. Al llegar a 60, la app se bloquea y no deja iniciar más sesiones por 8 horas — se muestra una pantalla de bloqueo con cuenta regresiva. El bloqueo se guarda con una marca de tiempo en `localStorage`, así que sobrevive a recargar la página o cerrar y volver a abrir la pestaña; al cumplirse las 8 horas el contador se reinicia solo. En el menú se agregó un contador "antes de bloqueo" junto a las estadísticas existentes. Como todo en la app, esto vive solo en el navegador — no hay servidor ni cuenta, así que es por dispositivo/navegador, y borrar los datos del sitio (o abrir en otro navegador) también reinicia el contador.
- **v1.9.0** — Se desactivó la selección de texto en toda la app (ya no aparece el resaltado azul al arrastrar sobre botones/HUD/texto) y se quitó el flash azul de "tap highlight" en móvil. Es puro CSS (`user-select:none` + `-webkit-tap-highlight-color:transparent`), no afecta clicks ni el funcionamiento de ningún botón. Auditoría de código: se revisaron todas las funciones JS y clases CSS — no había código muerto que quitar.
- **v1.8.0** — El número de "fallos" en el HUD ahora hace un pulso visual (parpadeo rojo + escala) cada vez que sube, para que el salto se note como un evento claro en vez de un cambio seco. Es una animación CSS pura disparada solo en el cambio de valor — corre en el compositor del navegador, no en el loop de juego, así que no cuesta rendimiento en los frames donde el número no cambia.
- **v1.7.0** — Se deshabilitó el click derecho en toda la app (ya no abre el menú contextual del navegador, y tampoco cuenta como disparo dentro del juego — solo el click izquierdo registra hits/fallos). Limpieza de código: se quitó una llamada redundante que recalculaba dos veces el mismo valor al iniciar sesión.
- **v1.6.0** — Optimización de fluidez para pantallas de alto refresh rate (144Hz+):
  - Se dejó de leer `getBoundingClientRect()` en cada movimiento del mouse (forzaba layout síncrono en el hilo principal en cada evento); ahora se cachea una sola vez al iniciar sesión y al hacer resize.
  - El grid de fondo del canvas ya no se redibuja línea por línea en cada frame; se prerenderiza una sola vez a un canvas offscreen y se pega con un solo `drawImage()` por frame.
  - Los nodos del HUD (tiempo, puntaje, aciertos, fallos, precisión) se cachean una sola vez en vez de re-buscarse con `querySelector`/`getElementById` en cada frame, y solo se escriben al DOM cuando su valor realmente cambió.
  - Nada de esto cambia el timing ni el puntaje — el juego sigue corriendo en base a `dt`, frame-rate independiente. El único cambio es menos trabajo de layout/DOM compitiendo con el render, que es lo que se sentía como microstutters en 144Hz+.
- **v1.5.0** — Se quitó el botón "Borrar mi progreso" y el aviso de localStorage del menú (ya no son necesarios). Título y descripción del menú renovados. Se agregaron íconos de redes sociales (Twitch, YouTube, Threads) debajo del crédito, cada uno abre en pestaña nueva.
- **v1.4.1** — Se quitó una migración automática de una sola vez que quedó de la v1.4.0. Debía correr solo una vez, pero si alguna vez usabas "Borrar mi progreso" (que reconstruía el store desde cero), la migración volvía a activarse y borraba de nuevo los récords de Tracking en la siguiente carga — dando la sensación de que un simple refresh (`Ctrl+Shift+R`) borraba el progreso.
- **v1.4.0** — El puntaje de Tracking ahora escala según qué tan difícil es el blanco en esa sesión (30% del ritmo en el blanco más fácil, hasta 100% en el más difícil). Antes pagaba lo mismo sin importar la dificultad, lo que permitía llegar cerca del tope "de nivel élite" jugando en modo fácil. Flick y Switch no se tocaron — nunca tuvieron ese problema.
- **v1.3.2** — Arreglado un sesgo de redondeo en Tracking que inflaba el puntaje en monitores de alto refresh rate (144Hz+), porque se redondeaba la ganancia de puntos en cada fotograma en vez de una sola vez al final.
- **v1.3.1** — Arreglado un bug donde iniciar una sesión no cancelaba un bucle de juego anterior (posible con doble clic en "Empezar"), lo que podía duplicar la velocidad de acumulación de puntos.
- **v1.3.0** — La dificultad dejó de subir para siempre sin límite (asíntota) y pasó a tener un tope real: 100% está anclado al reflejo visual promedio de un jugador profesional de esports, citado en varios estudios en ~180ms (el grupo más rápido medido, por encima de pilotos de caza y de F1). Fuente: reactiontimetests.org / investigación comparativa IJES sobre jugadores profesionales de FPS.

Con ese número se calculó cuántos puntos produciría una sesión completa jugando a ese ritmo, y **esa cifra fija es el 100%** de cada modo:

- **Flick / Switch** — asumiendo ~95% de aciertos reaccionando a 180ms de forma casi continua durante 60s: ~250,000 pts.
- **Tracking** — asumiendo ~75% del tiempo sobre el blanco de forma sostenida contra el blanco más rápido posible durante 60s: ~11,700 pts (con el ritmo de puntos ya escalado por dificultad desde la v1.4.0).

(Para 30s/90s estas cifras se escalan proporcionalmente a la duración.)

Una vez que tu récord alcanza ese tope, **la dificultad deja de subir** — los blancos ya no se hacen más chicos ni más rápidos — pero tu puntaje puede seguir subiendo sin límite si sigues jugando y mejorando. El tope del modo Flick/Switch también quedó reflejado en los parámetros del juego mismo: el blanco más difícil dura ~250ms (180ms de reacción + un margen de ~70ms para el clic físico), el mínimo con el que técnicamente se puede reaccionar y hacer clic a tiempo.

La curva de progreso usa una raíz cuadrada (no lineal) para que el avance se sienta bien desde el principio y se vuelva más exigente cerca del tope — pero el tope en sí es siempre ese número fijo, no un límite arbitrario ni infinito.

## Cómo borrar tu progreso ahora

Ya no hay botón en el menú para esto. Si lo necesitas: abre las herramientas de desarrollador (F12) → pestaña **Console** → escribe `localStorage.clear()` → Enter → recarga la página.

## Publicar en GitHub Pages

1. Crea un repo en GitHub (puede ser privado o público).
2. Sube los 5 archivos a la raíz del repo.
3. **Settings → Pages** → Source: rama `main`, carpeta `/ (root)` → Guardar.
4. En 1–2 minutos: `https://tu-usuario.github.io/tu-repo/`.

> El service worker **solo funciona sobre http/https**, no abriendo el archivo con doble clic (`file://`). Para offline real necesitas GitHub Pages, o correr un servidor local (`python -m http.server` en la carpeta, y abrir `http://localhost:8000`).

## Instalarla como app de Windows y que abra al iniciar

1. Abre la URL de GitHub Pages en **Chrome o Edge**.
2. En la barra de direcciones aparece un ícono de instalar (o menú ⋮ → **Aplicaciones → Instalar Reticle**). Instálala.
3. Esto crea una entrada en el Menú Inicio de Windows como cualquier otra app, y queda disponible sin internet (el service worker ya cacheó todo en tu primera visita).
4. Para que abra automáticamente al encender la PC:
   - Presiona `Win + R`, escribe `shell:startup` y da Enter (se abre la carpeta de inicio de Windows).
   - Busca "Reticle" en el Menú Inicio, clic derecho → **Más → Abrir ubicación del archivo**.
   - Copia el acceso directo que aparece ahí y pégalo dentro de la carpeta `shell:startup`.
   - Listo — se abrirá solo la próxima vez que inicies sesión en Windows.

## Notas técnicas

- Bucle de juego con `requestAnimationFrame` + delta-time: fluido a 60, 144 o 240Hz.
- Canvas ajustado a `devicePixelRatio` para nitidez en pantallas de alta densidad.
- Todo se guarda solo en `localStorage` de tu navegador (récords, sesiones jugadas, nivel de dificultad desbloqueado). Sin backend, sin tracking. Si limpias los datos del sitio o cambias de navegador, la progresión se reinicia.
