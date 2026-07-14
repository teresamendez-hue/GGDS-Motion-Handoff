# Motion Handoff — Inline Message (Web)

| | |
|---|---|
| **Componente** | Inline Message |
| **Plataforma** | Web |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token entrada** | `motion-curve-md` |
| **Token salida** | `motion-exit` |
| **Token expand** | `motion-curve-sm` |
| **Categoría** | Guía |
| **Fecha** | 2026-06-29 |
| **Figma** | [Core — Web Components](https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=4698-346) |

---

## Motion Specification

El Inline Message comunica información contextual sobre la tarea actual. Vive **en el flujo del layout** (parte superior del contenido o junto al elemento relevante), no flota como un Toast. Ancho 320px, cuatro semánticas (Information, Success, Warning, Error), título, descripción, Action Link opcional. **Default** incluye cierre con X; **Expand** no es cerrable y no lleva X.

La **aparición** usa el patrón estándar de mensajes **en flujo** (M3 Banner / Compose `fadeIn + expandVertically`): el slot expande verticalmente (`grid-template-rows` 0fr→1fr) y el bloque hace fade (`opacity` 0→1) en paralelo. Token: `motion-curve-md` (300ms). **Sin translate** — el mensaje ocupa su lugar entre el contenido y desplaza a los hermanos.

La **salida** (solo Default, tap en X) invierte con `motion-exit` (200ms): collapse vertical + fade out.

La variante **Expand** muestra solo el título colapsado; al tap en el chevron, la región de descripción (+ Action Link si está activo) se revela con `motion-curve-sm` (150ms).

Las semánticas no cambian tokens ni duraciones. Sin scrim ni overlay.

> **Nota Figma:** este handoff documenta solo motion para implementación en código. No se modifican archivos ni prototipos en Figma.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Mensaje se muestra (mount o programmatic) | — | — | — | — | — | — | 0 | — |
| 2 | Response | Bloque entra | `.inline-message-slot` | `grid-template-rows` | 0fr | 1fr | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Response | Bloque entra | `.inline-message` | `opacity` | 0 | 1 | `motion-curve-md` | 300ms | 0 | 300 |
| 4 | Trigger | Tap en X (solo Default) | — | — | — | — | — | — | * | — |
| 5 | Response | Bloque sale (solo Default) | `.inline-message-slot` | `grid-template-rows` | 1fr | 0fr | `motion-exit` | 200ms | * | *+200 |
| 6 | Response | Bloque sale (solo Default) | `.inline-message` | `opacity` | 1 | 0 | `motion-exit` | 200ms | * | *+200 |
| 7 | Trigger | Tap en chevron (variante Expand) | — | — | — | — | — | — | * | — |
| 8 | Response | Details se expande | `.inline-message-details` | `grid-template-rows` | 0fr | 1fr | `motion-curve-sm` | 150ms | * | *+150 |
| 9 | Response | Details se expande | `.inline-message-details-inner` | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms | * | *+150 |
| 10 | Response | Chevron rota | `.im-chevron-svg` | `transform` | 0deg | 180deg | `motion-curve-sm` | 150ms | * | *+150 |
| 11 | Trigger | Tap en chevron (colapsar) | — | — | — | — | — | — | * | — |
| 12 | Response | Reverse filas 8–10 | `.inline-message-details` | height, opacity | expandido | colapsado | `motion-curve-sm` | 150ms | * | *+150 |

> `*` = momento variable según interacción.

---

## Token Mapping

| Momento | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor ms |
|---|---|---|---|---|---|
| Entrada bloque | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0, 0, 0.2, 1)` | `duration-300` | 300ms |
| Salida bloque | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | `duration-200` | 200ms |
| Expand / collapse | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |

---

## Implementación CSS

```css
:root {
  --motion-curve-md: cubic-bezier(0, 0, 0.2, 1);
  --motion-curve-sm: cubic-bezier(0.4, 0, 0.2, 1);
  --motion-exit: cubic-bezier(0.4, 0, 1, 1);
  --duration-300: 300ms;
  --duration-200: 200ms;
  --duration-150: 150ms;
}

.inline-message-slot {
  display: grid;
  grid-template-rows: 0fr;
  transition: none;
}

.inline-message-slot.is-entering {
  grid-template-rows: 1fr;
  transition: grid-template-rows var(--duration-300) var(--motion-curve-md);
}

.inline-message-slot.is-exiting {
  grid-template-rows: 0fr;
  transition: grid-template-rows var(--duration-200) var(--motion-exit);
}

.inline-message-clip {
  overflow: hidden;
  min-height: 0;
}

.inline-message {
  opacity: 0;
  transition: none;
}

.inline-message.is-entering {
  opacity: 1;
  transition: opacity var(--duration-300) var(--motion-curve-md);
}

.inline-message.is-exiting {
  opacity: 0;
  transition: opacity var(--duration-200) var(--motion-exit);
}

.inline-message-details {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows var(--duration-150) var(--motion-curve-sm);
}

.inline-message.is-expanded .inline-message-details {
  grid-template-rows: 1fr;
}

.inline-message-details-inner {
  overflow: hidden;
  min-height: 0;
  opacity: 0;
  transition: opacity var(--duration-150) var(--motion-curve-sm);
}

.inline-message.is-expanded .inline-message-details-inner {
  opacity: 1;
}

.im-chevron-svg {
  transition: transform var(--duration-150) var(--motion-curve-sm);
}

.inline-message.is-expanded .im-chevron-svg {
  transform: rotate(180deg);
}

@media (prefers-reduced-motion: reduce) {
  .inline-message-slot,
  .inline-message,
  .inline-message-details,
  .inline-message-details-inner,
  .im-chevron-svg {
    transition: none !important;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| vs Toast | Toast flotante: slide + `motion-curve-lg`. Inline Message en flujo: fade + expand vertical + `motion-curve-md` |
| Slot | `grid-template-rows: 0fr/1fr` en `.inline-message-slot` — empuja contenido hermano sin translate |
| Semánticas | Info / Success / Warning / Error: solo tokens visuales; motion idéntico |
| Action Link | Default: bajo descripción. Expand: solo cuando está expandido |
| Variante Expand | Sin X · sin dismiss · solo expand/collapse |
| Accesibilidad | `prefers-reduced-motion`: estado final sin transición; mantener mensaje en DOM accesible hasta fin de animación de salida o remover tras `transitionend` |
| Figma | Referencia visual únicamente; implementación en código según este handoff |
