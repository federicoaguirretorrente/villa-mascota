# Cómo publicar Villa Mascota online

## Visitas a amigos reales (Firebase) — configuración única

Desde que se agregó "Visitar amigos" con código, el juego necesita un proyecto
gratuito de Firebase para guardar/leer las casas en vivo (sin esto, el botón
"Generar mi código" falla con un aviso, pero el resto del juego sigue
funcionando normal). Se configura **una sola vez**:

1. Entrá a **https://console.firebase.google.com/**, click en "Agregar proyecto" (o "Add project"), ponele un nombre (ej. `villa-mascota`), podés desactivar Google Analytics (no hace falta), y creá el proyecto.
2. Dentro del proyecto, click en el ícono **`</>`** ("Web") para registrar una app web. Ponele un apodo y continuá. Te va a mostrar un objeto `firebaseConfig` con 6 campos (`apiKey`, `authDomain`, `projectId`, `storageBucket`, `messagingSenderId`, `appId`) — **copiá ese objeto completo y pegámelo en el chat**. No es información secreta: la config web de Firebase está pensada para ser pública, lo que protege los datos son las reglas de seguridad (paso 4), no ocultar esto.
3. En el menú lateral: **Build → Authentication → Comenzar (Get started)**, pestaña "Sign-in method", y activá el proveedor **"Anónimo" (Anonymous)**. Esto deja que cada jugador tenga una identidad sin pedirle registrarse con email/contraseña.
4. **Build → Firestore Database → Crear base de datos**. Elegí cualquier ubicación, modo producción. Una vez creada, andá a la pestaña **"Reglas" (Rules)** y reemplazá todo el contenido por esto:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /players/{code} {
         allow read: if true;
         allow create: if request.auth != null && request.auth.uid == request.resource.data.uid;
         allow update: if request.auth != null && request.auth.uid == resource.data.uid;
         allow delete: if false;
       }
     }
   }
   ```
   Click en **"Publicar" (Publish)**.
5. Pasame el `firebaseConfig` del paso 2 acá en el chat. Yo lo pego en el código, hago el `git push`, y Netlify lo publica — quedando las visitas a amigos funcionando en vivo.

---

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

Hay dos maneras de pensar esto:

- **Publicar una vez** (secciones "Opción 1-4" más abajo): subís la carpeta a mano cada vez que querés actualizar algo.
- **Despliegue continuo** (sección siguiente, recomendada): lo conectamos una sola vez a GitHub + Netlify, y de ahí en adelante, cada vez que le pida a Claude "actualizá el juego online", Claude hace el cambio y lo publica solo — vos no volvés a tocar nada.

---

## Despliegue continuo (recomendado): GitHub + Netlify

Esto se configura **una sola vez**. Después, cualquier actualización que le pidas a Claude queda online en 1-2 minutos sin que vos tengas que subir nada a mano.

### Paso 1 — Creá tu cuenta de GitHub (gratis)
1. Entrá a **https://github.com/signup** y creá una cuenta (con tu email).
2. Confirmá el email que te manden.

### Paso 2 — Creá un repositorio vacío
1. Entrá a **https://github.com/new**
2. Nombre del repositorio: `villa-mascota` (o el que prefieras).
3. Dejalo en **Public** (Netlify gratis lo necesita público, o Private si tenés plan pago — Public está bien para esto).
4. **No** marques "Add a README" ni ningún otro archivo inicial — tiene que quedar completamente vacío.
5. Click en **"Create repository"**.
6. En la pantalla que aparece, copiá la URL que te muestra arriba, algo como:
   `https://github.com/tu-usuario/villa-mascota.git`
7. **Pegame esa URL en el chat** — con eso yo conecto el proyecto y subo el código por vos.

### Paso 3 — Conectá ese repositorio con Netlify (para que publique solo)
1. Creá una cuenta gratis en **https://app.netlify.com/signup** — podés elegir "Sign up with GitHub" para que quede todo conectado de una.
2. Click en **"Add new site" → "Import an existing project"**.
3. Elegí **GitHub**, autorizá el acceso si te lo pide, y seleccioná el repositorio `villa-mascota` que creaste.
4. Dejá la configuración por defecto (no hace falta build command ni carpeta especial, es un sitio estático) y click en **"Deploy site"**.
5. Netlify te da un link fijo como `https://villa-mascota-tuusuario.netlify.app`. Ese es el link definitivo para compartir.

### Listo — ¿cómo sigue de acá en adelante?
Una vez hecho esto, cada vez que quieras un cambio en el juego, simplemente pedímelo en el chat (por ejemplo "subí esta actualización" o "dejalo en productivo"), y yo:
1. Hago el cambio en `index.html`.
2. Subo el cambio al repositorio de GitHub (`git push`).
3. Netlify detecta el push y publica la nueva versión automáticamente — sin que tengas que entrar a ningún lado.

---

## Publicación manual (si preferís subir vos mismo cada vez)

4 formas de dejarlo online sin despliegue automático, de la más rápida a la más "profesional". Con cualquiera de las 4 alcanza — elegí una.

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
