# Prompt para Claude Code — Mejoras UX/UI y Animaciones

Necesito que apliques las siguientes mejoras al archivo `index.html` de mi web de casamiento. Es un single-page con todas las secciones en un solo archivo HTML (CSS inline en `<style>`, JS inline en `<script>`). Mantené esa estructura. Aplicá los cambios de forma incremental y no rompas nada existente.

---

## 1. HERO — Video en recuadro con bordes redondeados

El video actualmente ocupa toda la mitad inferior del hero a full-bleed. Quiero cambiarlo para que se vea dentro de un recuadro elegante centrado:

- Cambiar el layout del hero: en vez de que `.hero__media-wrapper` ocupe todo el espacio restante con `flex: 1`, hacer que el video esté **centrado** dentro del hero, debajo del texto, con un tamaño contenido.
- El video debe tener `border-radius: 16px` y un `box-shadow` sutil tipo `0 8px 40px rgba(0,0,0,0.25)`.
- Agregar un borde dorado fino: `border: 2px solid var(--gold)` con `padding: 4px` (doble borde, similar a la tarjeta de regalo).
- El contenedor del video debe tener `max-width: 600px` en desktop y `max-width: 90vw` en mobile.
- Mantener el aspect-ratio del video con `aspect-ratio: 16/9` y `overflow: hidden` en el contenedor.
- Aplicar un filtro CSS sutil al video: `filter: brightness(0.95) saturate(0.9)` para que no se vea tan crudo y tenga un tono más cálido/cinematográfico.
- Eliminar el `hero__media-fade` (el gradiente overlay) ya que al ser recuadro ya no es necesario.
- El chevron debe quedar debajo del recuadro del video, no superpuesto.
- En mobile el video debe verse bien centrado con margen lateral.

## 2. Animación de entrada del Hero (stagger)

Agregar animación de entrada escalonada al hero cuando carga la página:
- `.hero__pretitle` → aparece primero con fade-in + translateY(-20px), delay 0.3s
- `.hero__title` → aparece segundo, delay 0.6s
- `.hero__date` → aparece tercero, delay 0.9s
- El recuadro del video → aparece cuarto con fade-in + scale(0.95→1), delay 1.2s
- Usar `@keyframes` CSS con `animation-fill-mode: both`
- Respetar `prefers-reduced-motion`

## 3. Variedad en animaciones de scroll

Actualmente todos los `.fade-in` hacen lo mismo (translateY + opacity). Agregar variedad:
- Las tarjetas del timeline: las impares (ceremonia) entran desde la izquierda (`translateX(-30px)`), las pares (fiesta) desde la derecha (`translateX(30px)`). Agregar clases `.fade-in-left` y `.fade-in-right` y aplicarlas en el HTML.
- Los títulos de sección (`h2.section-title`): que tengan un efecto de clip-path reveal, de `clip-path: inset(0 50% 0 50%)` a `clip-path: inset(0 0 0 0)`.
- Mantener el `.fade-in` base para los subtítulos y elementos genéricos.

## 4. Hover en tarjetas del timeline (desktop)

Agregar al `.timeline__card`:
```css
transition: transform 0.3s ease, box-shadow 0.3s ease;
```
Y en hover:
```css
transform: translateY(-4px);
box-shadow: 0 8px 24px rgba(61,58,53,0.12);
```

## 5. Sección Dress Code — Eliminar repetición y mejorar contenido

El texto "Elegante / Semi-formal" aparece 3 veces. Corregir:
- El `h2.section-title` queda como "Dress Code"
- El `.section-subtitle` cambiarlo a: "Queremos que estén cómodos y elegantes"
- Dentro de la tarjeta, cambiar el `<h3>` a: "Elegante / Semi-formal"
- Agregar debajo del texto de la tarjeta una fila de 5 círculos de colores sugeridos (la paleta de colores tierra/neutros) usando divs circulares con los colores: `#CB997E`, `#DDBEA9`, `#B7B7A4`, `#A3A58D`, `#6B705C`. Cada uno de 28px, con un label tipo "Colores sugeridos" arriba en font-sans pequeño.

## 6. Agregar botón "Cómo llegar" a la Ceremonia

En el timeline, la tarjeta de Ceremonia no tiene botón de ubicación. Agregar un `<a>` con la misma clase `.timeline__btn` apuntando a:
`https://maps.google.com/?q=Iglesia+Catedral+San+Luis+Argentina`
Con el mismo ícono de pin y texto "Cómo llegar".

## 7. Contraste en la sección de regalo

El texto blanco sobre el fondo `var(--sage-fern)` (#A3A58D) tiene bajo contraste. Mejorar:
- Cambiar el `.section-subtitle` de la sección gift a `font-weight: 400` (en vez de 300).
- Agregar `text-shadow: 0 1px 2px rgba(0,0,0,0.15)` a los textos blancos de esa sección.

## 8. Chevron del hero: fade out al scrollear

Agregar un IntersectionObserver en el JS que observe el hero. Cuando el usuario empiece a scrollear y el hero empiece a salir del viewport, aplicar un fade-out al chevron (opacity: 0 con transition).

## 9. Botón de música — Mover a la izquierda

Para que no compita con la dot-nav de la derecha, mover `.music-btn` de `right: 1.25rem` a `left: 1.25rem`.

## 10. Micro-animación en botón "Copiar alias"

Al momento de copiar exitosamente, agregar un micro-scale al botón:
- `transform: scale(1.08)` que vuelve a `scale(1)` en 200ms.
- Usar una clase temporal que se agrega junto con `is-copied`.

## 11. Transiciones suaves entre secciones

Agregar pseudo-elementos `::after` a cada sección (excepto la primera y última) que generen un degradado sutil de 60px de alto entre los colores de fondo de secciones consecutivas. Esto suaviza el cambio abrupto de colores entre secciones.

Las transiciones específicas:
- Timeline (dusty-rose) → Dresscode (soft-apricot): gradiente de dusty-rose a soft-apricot
- Dresscode (soft-apricot) → Contact (peach-cream): gradiente de soft-apricot a peach-cream
- Contact (peach-cream) → Gift (sage-fern): gradiente de peach-cream a sage-fern

---

## REGLAS GENERALES

- Todo en un solo archivo `index.html`
- No agregar dependencias externas (no GSAP, no AOS, etc). Solo CSS y JS vanilla.
- Respetar la paleta de colores existente (variables CSS en :root).
- Respetar `prefers-reduced-motion` en todas las animaciones nuevas.
- Mantener la accesibilidad (aria-labels, focus-visible, etc).
- No tocar la lógica de música, copy alias, ni dot-nav existente (salvo lo indicado).
- Asegurate de que todo se vea bien en mobile (390px) y desktop (1440px+).
