# Cómo publicar Villa Mascota online

Tu juego es **un sitio 100% estático**: un solo archivo `index.html` + la carpeta `assets/` con las imágenes. No necesita servidor, base de datos ni instalación — cualquier hosting de archivos estáticos sirve. Los datos de cada jugador (mascota, monedas, progreso) se guardan en el navegador de esa persona (`localStorage`), no en un servidor.

Antes de subir, confirmá que tenés estos dos elementos juntos en una misma carpeta:
```
Villa Mascota/
  index.html
  assets/
    dock_armario.png
    map_cafe.png
    ... (el resto de las imágenes)
```

A continuación, 4 formas de dejarlo online, de la más rápida a la más "profesional". Con cualquiera de las 4 alcanza — elegí una.

---

## Opción 1 — Netlify Drop (la más rápida, sin cuenta)

Ideal si solo querés un link para mandarle a alguien ya mismo.

1. Entrá a **https://app.netlify.com/drop**
2. Arrastrá la carpeta completa `Villa Mascota` (la que contiene `index.html` y `assets/`) directo sobre la página.
3. Netlify sube los archivos y en segundos te da un link del tipo `https://nombre-random.netlify.app`.
4. Listo, ese link ya es jugable desde cualquier dispositivo.

**Limitación:** sin crear una cuenta, ese sitio es temporal y no lo podés volver a editar fácilmente. Si te gusta, te conviene crear una cuenta gratis en Netlify (podés hacerlo después de subirlo, y "reclamar" ese sitio para que quede permanente y vos lo puedas actualizar).

---

## Opción 2 — Netlify con cuenta (recomendada para uso real)

Gratis, permanente, y podés volver a subir una nueva versión del juego cuando lo actualices.

1. Creá una cuenta gratis en **https://app.netlify.com/signup** (con email, Google o GitHub).
2. Una vez adentro, click en **"Add new site" → "Deploy manually"**.
3. Arrastrá la carpeta `Villa Mascota` igual que en la Opción 1.
4. Te da un link fijo (editable) como `https://villa-mascota-tuusuario.netlify.app`. Desde el panel podés cambiarle el nombre al subdominio.
5. Cada vez que modifiques el juego, volvés a esa misma pantalla y arrastrás la carpeta de nuevo para actualizar.

---

## Opción 3 — GitHub Pages (si ya usás o querés usar GitHub)

Gratis y queda versionado (guarda el historial de cambios).

1. Creá un repositorio nuevo en **https://github.com/new** (puede ser público o privado, pero Pages gratis requiere que sea público).
2. Subí `index.html` y la carpeta `assets/` a ese repositorio (podés arrastrar los archivos directo en la web de GitHub, sección "Add file → Upload files").
3. En el repositorio, andá a **Settings → Pages**.
4. En "Source" elegí la rama `main` y la carpeta `/ (root)`, guardá.
5. GitHub te da un link como `https://tuusuario.github.io/nombre-repo/` (puede tardar 1-2 minutos en activarse).

---

## Opción 4 — itch.io (pensado para juegos, buena opción para compartir con otros chicos/familias)

itch.io es una plataforma hecha específicamente para alojar jueguitos como este, con su propia página de presentación.

1. Creá una cuenta gratis en **https://itch.io/register**
2. Comprimí la carpeta `Villa Mascota` completa en un `.zip` (seleccioná `index.html` y `assets/`, click derecho → "Comprimir").
3. Andá a **https://itch.io/game/new**, completá el título y la descripción.
4. En "Kind of project" elegí **HTML**, y subí el `.zip`.
5. Marcá la casilla **"This file will be played in the browser"**.
6. En "Embed options" asegurate de que el ancho/alto sea suficientemente grande (por ejemplo 900x700) para que se vea cómodo.
7. Publicá la página — te da un link tipo `https://tuusuario.itch.io/villa-mascota`.

---

## ¿Cuál elijo?

- **Solo quiero un link ya para probarlo con mi hija** → Opción 1 (Netlify Drop).
- **Quiero un link fijo que pueda seguir actualizando** → Opción 2 (Netlify con cuenta).
- **Ya uso GitHub o quiero tener el código versionado** → Opción 3.
- **Quiero que se vea como "un juego" con su propia página, ficha, comentarios, etc.** → Opción 4 (itch.io).

Cualquiera de las cuatro funciona igual de bien técnicamente — el juego corre entero en el navegador de quien lo abra, así que no hay diferencias de rendimiento entre opciones.
