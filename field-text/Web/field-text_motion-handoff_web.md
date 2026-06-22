# Motion Handoff — Field Text (Web)

| | |
|---|---|
| **Componente** | Field Text |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-md` |
| **Categoría** | Default |
| **Fecha** | 2026-06-17 |

---

## Motion Specification

Field Text en Web se comporta como componente de estado interno. No usa entrada/salida de viewport.

Estados cubiertos: `Enabled`, `Hovered`, `Focused`, `Typing`, `Empty`, `Filled`, `Error`, `Disabled`.

La transición principal sucede en el anillo de foco externo y en el color de borde/superficie del contenedor. Se utiliza `motion-curve-md` para lectura y edición confortable.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.field-text` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.field-text` | `border-color`, `background-color` | default | hover | `motion-curve-md` | 300ms | 0 | 200 |
| 3 | Trigger | Focus visible | `.field-text` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focus ring in | `.field-text__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 200 |
| 5 | Trigger | Input typing | `.field-text__control` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Typing settle | `.field-text` | `border-color` | focus | active | `motion-curve-md` | 300ms | 0 | 200 |
| 7 | Trigger | Validation error | `.field-text` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Error emphasize | `.field-text__ring` | `border-color`, `opacity` | focus | error | `motion-curve-md` | 300ms | 0 | 200 |
| 9 | Trigger | Toggle disabled | `.field-text` | — | — | — | — | — | 0 | 0 |
| 10 | Response | Disabled state | `.field-text` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | `duration-300` | 300ms |

---

## Implementación CSS

```css
.field-text__ring {
  transition:
    opacity var(--motion-duration-from-motion-curve-md) var(--motion-curve-md),
    border-color var(--motion-duration-from-motion-curve-md) var(--motion-curve-md);
}

.field-text {
  transition:
    border-color var(--motion-duration-from-motion-curve-md) var(--motion-curve-md),
    background-color var(--motion-duration-from-motion-curve-md) var(--motion-curve-md);
}

.field-text.is-disabled,
.field-text[aria-disabled="true"] {
  transition: none;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo motion de estado interno |
| Focus ring | anillo externo con opacidad animada |
| Disabled | sin transición |
| A11y | `prefers-reduced-motion` aplica estado final |
| Coherencia | mismo ritmo que `Field Pin Code` (`md`) |
