# Reticle v1.2.0 — Entrenador de Aim
_created by gK1wi_

5 archivos: `index.html`, `manifest.json`, `service-worker.js`, `icon-192.png`, `icon-512.png`. Súbelos juntos, en la misma carpeta/raíz del repo — no muevas ni renombres ninguno, el service worker y el manifest los referencian por nombre.

## Novedades de esta versión

- **Dificultad continua, no por niveles**: ya no hay Fácil/Normal/Difícil como botones. Cada modo tiene su propio nivel de dificultad (0–100%, mostrado en el menú) que sube solo, suavemente, en función de tu récord en ese modo — cuanto mejor tu marca, más chicos y rápidos son los blancos. Nunca baja, y como es una curva continua no hay saltos bruscos como en la versión anterior.
- Como el sistema de dificultad cambió por completo, **esta versión reinicia el progreso guardado** (récords y sesiones de la v1.1.0 no se migran). A partir de aquí todo lo que juegues se acumula sin volver a resetear.

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
