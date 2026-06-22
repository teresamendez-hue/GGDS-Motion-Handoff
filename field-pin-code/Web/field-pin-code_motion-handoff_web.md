# Motion Handoff — Field Pin Code (Web)

| | |
|---|---|
| **Componente** | Field Pin Code |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-md` |
| **Categoría** | Default |
| **Fecha** | 2026-06-18 |

---

## Motion Specification

Field Pin Code en Web se comporta como componente de estado interno. No usa entrada/salida de viewport.

Estados cubiertos: `Enabled`, `Hovered`, `Focused`, `Typing`, `Filled`, `Error`, `Disabled`.

La animación principal está en el anillo de foco externo y en la transición de estado de cada celda. Se usa `motion-curve-md` para que el ritmo sea levemente más pausado que controles de acción rápida.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Focus visible | `.pin-field` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Focus ring in | `.pin-field__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 200 |
| 3 | Trigger | Input digit | `.pin-cell` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Cell filled | `.pin-cell` | `transform`, `background-color` | `scale(1)`, default | `scale(1.02)`, active | `motion-curve-md` | 300ms | 0 | 200 |
| 5 | Trigger | Validation error | `.pin-field` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Error emphasize | `.pin-field__ring` | `border-color`, `opacity` | focus | error | `motion-curve-md` | 300ms | 0 | 200 |
| 7 | Trigger | Toggle disabled | `.pin-field` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Disabled state | `.pin-field` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | `duration-300` | 300ms |

---

## Implementación CSS

```css
.pin-field__ring {
  transition:
    opacity var(--motion-duration-from-motion-curve-md) var(--motion-curve-md),
    border-color var(--motion-duration-from-motion-curve-md) var(--motion-curve-md);
}

.pin-cell {
  transition:
    transform var(--motion-duration-from-motion-curve-md) var(--motion-curve-md),
    background-color var(--motion-duration-from-motion-curve-md) var(--motion-curve-md);
}

.pin-field.is-disabled,
.pin-field[aria-disabled="true"] {
  transition: none;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo motion de estado interno |
| Focus ring | anillo externo, nunca doble ring interno |
| Disabled | sin transición |
| A11y | `prefers-reduced-motion` aplica estado final |
| Coherencia | mismo ritmo que `Field Text` (`md`) |
