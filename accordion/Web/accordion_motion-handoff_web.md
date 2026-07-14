# Motion Handoff — Accordion (Web)

| | |
|---|---|
| **Componente** | Accordion |
| **Plataforma** | Web |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token expand/collapse** | `motion-curve-sm` |
| **Token header estados** | `motion-curve-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-30 |
| **Figma** | [Core — Web Components](https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=10180-45) |

---

## Motion Specification

El Accordion es un **ítem único** expandible: fila de header (título + chevron, 48px) y slot de contenido oculto cuando está colapsado. No documenta comportamiento de grupo (múltiples ítems, solo uno abierto, etc.).

**Expand/collapse** (tap en header): el slot revela contenido con `grid-template-rows` 0fr→1fr, fade del inner (`opacity` 0→1) y rotación del chevron 0→180deg en paralelo. Token: `motion-curve-sm` (150ms). Colapsar invierte las mismas propiedades con el mismo token. **Sin** `motion-exit` — no hay entrada/salida del viewport.

**Header** (hover, pressed, focus-visible): transiciones de `background-color`, `transform` y `opacity` con `motion-curve-sm`.

**Type Default vs Container:** cambio de variante visual (fondo transparente vs surface con radius 8px; padding 16px en slot expandido en Container) **sin animación**.

Sin stagger. Sin scrim.

> **Nota Figma:** este handoff documenta solo motion para implementación en código.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Tap en header (expandir) | — | — | — | — | — | — | * | — |
| 2 | Response | Slot expande | `.accordion-body` | `grid-template-rows` | 0fr | 1fr | `motion-curve-sm` | 150ms | * | *+150 |
| 3 | Response | Contenido visible | `.accordion-body-inner` | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms | * | *+150 |
| 4 | Response | Chevron rota | `.accordion-chevron` | `transform` | 0deg | 180deg | `motion-curve-sm` | 150ms | * | *+150 |
| 5 | Trigger | Tap en header (colapsar) | — | — | — | — | — | — | * | — |
| 6 | Response | Reverse filas 2–4 | slot, inner, chevron | — | expandido | colapsado | `motion-curve-sm` | 150ms | * | *+150 |
| 7 | Trigger | Hover header | — | — | — | — | — | — | * | — |
| 8 | Response | Header hover | `.accordion-header` | `background-color` | default | hovered | `motion-curve-sm` | 150ms | * | *+150 |
| 9 | Trigger | Press header | — | — | — | — | — | — | * | — |
| 10 | Response | Header pressed | `.accordion-header` | `transform`, `opacity` | 1, 1 | scale ~0.985, ~0.92 | `motion-curve-sm` | 150ms | * | *+150 |
| 11 | Trigger | Focus visible (Tab) | — | — | — | — | — | — | * | — |
| 12 | Response | Focus ring | `.accordion-header` | outline | none | visible | `motion-curve-sm` | 150ms | * | *+150 |
| 13 | Trigger | Cambio Type Default ↔ Container | — | — | — | — | — | — | * | — |
| 14 | Response | Variante visual | root | — | — | — | **sin animación** | — | — | — |

> `*` = momento variable según interacción.

---

## Token Mapping

| Momento | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor ms |
|---|---|---|---|---|---|
| Expand / collapse | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |
| Header hover / pressed / focus | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |

---

## Implementación CSS

```css
:root {
  --motion-curve-sm: cubic-bezier(0.4, 0, 0.2, 1);
  --duration-150: 150ms;
}

.accordion-body {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows var(--duration-150) var(--motion-curve-sm);
}

.accordion.is-expanded .accordion-body {
  grid-template-rows: 1fr;
}

.accordion-body-clip {
  overflow: hidden;
  min-height: 0;
}

.accordion-body-inner {
  opacity: 0;
  transition: opacity var(--duration-150) var(--motion-curve-sm);
}

.accordion.is-expanded .accordion-body-inner {
  opacity: 1;
}

.accordion-chevron {
  transition: transform var(--duration-150) var(--motion-curve-sm);
}

.accordion.is-expanded .accordion-chevron {
  transform: rotate(180deg);
}

.accordion-header {
  transition-property: background-color, transform, opacity;
  transition-duration: var(--duration-150);
  transition-timing-function: var(--motion-curve-sm);
}

@media (prefers-reduced-motion: reduce) {
  .accordion-body,
  .accordion-body-inner,
  .accordion-chevron,
  .accordion-header {
    transition: none !important;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| vs Inline Message Expand | Mismo patrón grid 0fr/1fr + opacity + chevron; Accordion es ítem persistente sin dismiss |
| Ítem único | No animar comportamiento de accordion group en este handoff |
| Type | Default / Container: swap visual instantáneo |
| Viewport | Sin enter/exit; componente siempre en DOM |
| Accesibilidad | `prefers-reduced-motion`: estado final sin transición; `aria-expanded` en header |
| Figma | Referencia visual; implementación en código según este handoff |
