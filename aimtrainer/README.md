# Reticle — Entrenador de Aim

Un solo archivo (`index.html`), sin dependencias externas más que las fuentes de Google Fonts. Corre 100% en el navegador, a la frecuencia de tu monitor (144Hz incluido), y guarda tus récords en `localStorage`.

## Publicar en GitHub Pages (recomendado)

1. Crea un repositorio nuevo en GitHub (puede ser privado o público).
2. Sube `index.html` a la raíz del repo (arrastrar y soltar en la web de GitHub funciona).
3. Ve a **Settings → Pages**.
4. En "Source" elige la rama `main` y la carpeta `/ (root)`. Guarda.
5. En 1–2 minutos tu app estará en `https://tu-usuario.github.io/tu-repo/`.
6. Ábrela en Chrome/Edge con tu monitor a 144Hz activado en Windows (Configuración → Pantalla → Frecuencia de actualización).

También puedes simplemente abrir `index.html` haciendo doble clic — funciona igual de local, solo que no la puedes compartir con un link.

## Notas técnicas

- El bucle de juego usa `requestAnimationFrame` con delta-time, así que corre fluido y a velocidad correcta sin importar si tu monitor está a 60, 144 o 240Hz.
- El canvas se ajusta a `devicePixelRatio` para que se vea nítido en pantallas de alta densidad.
- Los datos (mejores puntajes, sesiones jugadas) se guardan solo en `localStorage` de tu navegador — si cambias de navegador o borras datos de sitio, se pierden. No hay backend ni tracking.
