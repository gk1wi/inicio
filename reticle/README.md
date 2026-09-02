# Reticle v1.4.1 — Entrenador de Aim
_created by gK1wi_

5 archivos: `index.html`, `manifest.json`, `service-worker.js`, `icon-192.png`, `icon-512.png`. Súbelos juntos, en la misma carpeta/raíz del repo — no muevas ni renombres ninguno, el service worker y el manifest los referencian por nombre.

## Changelog

- **v1.4.1** — Se quitó una migración automática de una sola vez que quedó de la v1.4.0. Debía correr solo una vez, pero si alguna vez usabas "Borrar mi progreso" (que reconstruye el store desde cero), la migración volvía a activarse y borraba de nuevo los récords de Tracking en la siguiente carga — dando la sensación de que un simple refresh (`Ctrl+Shift+R`) borraba el progreso. Ya no existe ese código: nada se borra automáticamente nunca más, solo con el botón "Borrar mi progreso" o limpiando `localStorage` a mano.
- **v1.4.0** — El puntaje de Tracking ahora escala según qué tan difícil es el blanco en esa sesión (30% del ritmo en el blanco más fácil, hasta 100% en el más difícil). Antes pagaba lo mismo sin importar la dificultad, lo que permitía llegar cerca del tope "de nivel élite" jugando en modo fácil. Flick y Switch no se tocaron — nunca tuvieron ese problema.
- **v1.3.2** — Arreglado un sesgo de redondeo en Tracking que inflaba el puntaje en monitores de alto refresh rate (144Hz+), porque se redondeaba la ganancia de puntos en cada fotograma en vez de una sola vez al final.
- **v1.3.1** — Arreglado un bug donde iniciar una sesión no cancelaba un bucle de juego anterior (posible con doble clic en "Empezar"), lo que podía duplicar la velocidad de acumulación de puntos. Se agregó el enlace **"Borrar mi progreso"** en el menú para limpiar `localStorage` sin usar herramientas de desarrollador.
- **v1.3.0** — La dificultad dejó de subir para siempre sin límite (asíntota) y pasó a tener un tope real: 100% está anclado al reflejo visual promedio de un jugador profesional de esports, citado en varios estudios en ~180ms (el grupo más rápido medido, por encima de pilotos de caza y de F1). Fuente: reactiontimetests.org / investigación comparativa IJES sobre jugadores profesionales de FPS.

Con ese número se calculó cuántos puntos produciría una sesión completa jugando a ese ritmo, y **esa cifra fija es el 100%** de cada modo:

- **Flick / Switch** — asumiendo ~95% de aciertos reaccionando a 180ms de forma casi continua durante 60s: ~250,000 pts.
- **Tracking** — asumiendo ~75% del tiempo sobre el blanco de forma sostenida contra el blanco más rápido posible durante 60s: ~11,700 pts (con el ritmo de puntos ya escalado por dificultad desde la v1.4.0).

(Para 30s/90s estas cifras se escalan proporcionalmente a la duración.)

Una vez que tu récord alcanza ese tope, **la dificultad deja de subir** — los blancos ya no se hacen más chicos ni más rápidos — pero tu puntaje puede seguir subiendo sin límite si sigues jugando y mejorando. El tope del modo Flick/Switch también quedó reflejado en los parámetros del juego mismo: el blanco más difícil dura ~250ms (180ms de reacción + un margen de ~70ms para el clic físico), el mínimo con el que técnicamente se puede reaccionar y hacer clic a tiempo.

La curva de progreso usa una raíz cuadrada (no lineal) para que el avance se sienta bien desde el principio y se vuelva más exigente cerca del tope — pero el tope en sí es siempre ese número fijo, no un límite arbitrario ni infinito.

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
