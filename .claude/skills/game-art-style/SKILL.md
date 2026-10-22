---
name: game-art-style
description: Villa Mascota's in-game art direction — use whenever designing a NEW visual object for the game (furniture, decor, toys, minigame props, UI icons drawn in SVG). Keeps new art consistent with the established illustrated, non-geometric style instead of reinventing conventions each time.
---

# Villa Mascota — guía de arte

Esta skill documenta cómo está dibujado el juego hoy, para que cualquier objeto
nuevo (mueble, juguete, prop de minijuego) encaje visualmente con lo que ya
existe en `index.html`, sin inventar una paleta o técnica distinta cada vez.

**Regla de oro del proyecto**: nada de formas geométricas planas y genéricas.
Todo objeto nuevo debe transmitir algo de volumen/profundidad, aunque sea
simple — un mueble "plano" sin ninguna sombra/pliegue se va a sentir fuera de
lugar al lado de todo lo demás.

El juego usa **dos lenguajes visuales distintos**, a propósito, según qué tipo
de objeto es. Primero hay que identificar cuál aplica.

## 1. Mobiliario y decoración de la casa/jardín ("estilo plano con bloques")

Usado por: sofá, cama, silla, planta, flores, cuadro, etc. (objeto `ICONS` +
`ITEMS` con `t:'furn'`).

Técnica: formas simples (`rect` con `rx` redondeado, `circle`, algún `path`)
en 2-3 tonos de UN MISMO color — el tono base y una variante **oscura** para
marcar pliegues, uniones o sombra de contacto (nunca gradientes ni brillos).
El volumen sale de la silueta y del recorte de sombra, no de shading.

Paleta compartida (objeto `F`, ya definido en `index.html` — reusar estos
tokens en vez de inventar hex nuevos):
```js
const F={
  wood:'#6b4c35',woodDark:'#4a3426',woodLight:'#8a6346',
  cream:'#f3ead3',creamDark:'#e2d4b0',creamDarker:'#d6c39c',
  mustard:'#d9a441',mustardDark:'#b9842c',mustardLight:'#e8bd6e',
  sage:'#8a9a74',sageDark:'#6b7f5a',
  teal:'#6fa8a0',tealDark:'#4f8178',
  blush:'#e2a0a0',blushDark:'#c97c7c',
  gold:'#c9a227',goldLight:'#e3c766',
  charcoal:'#3a434a',steel:'#8a94a0',
};
```
Si el objeto necesita un color que no está en `F`, agregar un par `nombre` /
`nombreDark` (y `nombreLight` si hace falta) siguiendo el mismo patrón de
saturación/luminosidad que los existentes, no un color suelto.

Ejemplo real (sofá, `index.html` ~L627): patas y almohadones en
`mustardDark`, cuerpo principal en `mustard`, botones como círculos chicos en
`mustardDark`. Mirar también `cama`, `planta`, `flores`, `cuadro` como
referencia directa de proporciones (viewBox `0 0 100 100` siempre).

### Checklist para agregar un mueble/juguete nuevo
1. Dibujarlo en `ICONS` como una función que devuelve `<svg viewBox="0 0 100 100">...</svg>`, con 100x100 como lienzo estándar.
2. Registrarlo en `ITEMS`: `id:{n:'Nombre',e:'🪄',t:'furn',shop:'furn',p:precio,s:escala}` — `e` es el emoji de respaldo que se muestra si `ICONS[id]` no existiera (ver `itemIcon()`), `s` es la escala relativa al tamaño base de mueble (`FURN_BASE=9`, referencia: la mascota mide 15).
3. Si va colgado en la pared (como `cuadro`/`reloj`): agregar el `id` a `WALL_ITEMS`.
4. Si es un mueble grande que no flota sobre otros (como `sofa`/`cama`): agregarlo a `BIG_ITEMS`.
5. Si algo se puede apoyar ENCIMA de este mueble (como sentarse en el sofá): agregar una entrada en `SURFACES` con `lift`/`span`.
6. Probar el ícono en dos tamaños: chico (tienda/inventario) y grande (colocado en la casa) — tiene que leerse bien en ambos.

## 2. Mascota y props de minijuego ("estilo ilustrado con profundidad")

Usado por: el dibujo de la mascota (`petSVG`), y objetos de los minijuegos
como el disco de arcilla (`claySVG`), la pistola de agua (`waterGunSVG`), el
splash (`splashSVG`).

Técnica: capas apiladas que simulan luz desde arriba-izquierda —
sombra/base oscura abajo, cuerpo en el tono principal, una capa más clara
arriba simulando el lado iluminado, y opcionalmente un brillo/reflejo
pequeño y blanco/claro en la esquina superior-izquierda de la forma.  Esto es
lo que da la sensación de "objeto con volumen" en vez de un sticker plano.

Reusar el helper `shade(hex, amt)` ya definido (`amt` negativo oscurece,
positivo aclara) en vez de elegir tonos a mano:
```js
const line  = shade(base, -0.5);  // contorno
const dark2 = shade(base, -0.2);  // sombra/pliegue
const light = shade(base, 0.62);  // cara iluminada
```
Contorno típico: `stroke="${line}" stroke-width="3.5" stroke-linejoin="round"`
(ver `petSVG`) — nunca negro puro.

Ejemplo real del disco de arcilla (`claySVG`, `index.html` ~L1798), 5 capas
sobre un viewBox `0 0 100 100`:
1. Sombra proyectada (elipse ancha, oscura, `opacity:.3`, debajo de todo)
2. Aro oscuro de base (el "grosor" del disco)
3. Cuerpo principal en el color base
4. Cúpula superior en un tono más claro (cara iluminada)
5. Brillo pequeño, ovalado, semi-transparente, arriba a la izquierda

Para un prop nuevo de minijuego: replicar este patrón de 4-5 capas en vez de
una sola forma plana. 2-3 colores está bien (incluyendo el acento), no hace
falta un realismo complejo — lo que importa es que tenga sombra + cuerpo +
luz, en ese orden de capas.

## Convenciones generales (aplican a ambos estilos)

- **Unidad responsive**: toda medida en CSS debe usar `calc(var(--u)*N)`
  (1 `u` ≈ 1% del ancho del contenedor), nunca píxeles fijos — así escala
  igual en mobile y desktop.
- **Paleta general del juego**: fondos pasteles/saturación media (cielos,
  pasto, arena), props y acentos en colores más saturados para que se lean
  bien sobre fondo pastel y sean fáciles de tocar en pantallas chicas.
- **Tamaño mínimo tocable**: cualquier objeto interactivo (plato, botón,
  juguete) tiene que verse y tocarse cómodo para un chico de 4-9 años en
  mobile — no miniaturizar de más.
- **No usar un color suelto sin su variante oscura/clara** — si se introduce
  un tono nuevo, definir el par `base`/`baseDark` (y `baseLight` si aplica)
  junto a los existentes, para que quede reutilizable.

## Qué esta skill NO puede hacer

Esta skill cubre arte dibujado en código (SVG inline), que es como está
armado hoy el 100% del juego interactivo (mascota, props, minijuegos) y el
mobiliario. Los **íconos del dock y los edificios del mapa** son imágenes
PNG ilustradas que el usuario proveyó desde afuera (carpeta `assets/`) — no
fueron generadas por Claude. No hay ningún conector de generación de
imágenes (tipo DALL·E/Midjourney/Figma) conectado en este entorno, así que
si en algún momento se quiere ese mismo look "pintado"/raster para un
objeto nuevo en vez de SVG, hace falta que el usuario lo traiga como
archivo (hecho a mano o con una herramienta de imágenes externa) para
integrarlo — Claude puede acomodarlo en el juego, pero no generar el PNG
desde cero.
