# Motion Handoff — Text Area (Web)

| | |
|---|---|
| **Componente** | Text Area |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-md` |
| **Categoría** | Default |
| **Fecha** | 2026-06-22 |

---

## Motion Specification

Text Area en Web usa motion de estado interno. No aplica entrada/salida de viewport.

Estados cubiertos: `Enabled`, `Hovered`, `Focused`, `Typing`, `Filled`, `Error`, `Disabled`.

La transición principal sucede en el anillo de foco externo y en los cambios de opacidad/color del contenedor y helper para mantener legibilidad durante escritura extendida.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.text-area` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.text-area` | `border-color` | default | hover | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Trigger | Focus visible | `.text-area` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focus ring in | `.text-area__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 300 |
| 5 | Trigger | Input typing | `.text-area__control` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Typing settle | `.text-area` | `border-color` | focus | active | `motion-curve-md` | 300ms | 0 | 300 |
| 7 | Trigger | Validation error | `.text-area` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Error feedback | `.text-area__ring`, `.text-area__helper` | `border-color`, `opacity` | focus/default | error | `motion-curve-md` | 300ms | 0 | 300 |
| 9 | Trigger | Toggle disabled | `.text-area` | — | — | — | — | — | 0 | 0 |
| 10 | Response | Disabled state | `.text-area` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | `duration-300` | 300ms |

---

## Implementación CSS

```css
.text-area {
  transition-property: border-color, opacity;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.text-area__ring {
  transition-property: opacity, border-color;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.text-area:disabled {
  transition: none;
}

@media (prefers-reduced-motion: reduce) {
  .text-area,
  .text-area__ring {
    transition: none;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo motion de estado interno |
| Focus ring | anillo externo único |
| Disabled | sin transición |
| A11y | `prefers-reduced-motion` aplica estado final |
| Coherencia | mismo patrón que `Field Text` |
