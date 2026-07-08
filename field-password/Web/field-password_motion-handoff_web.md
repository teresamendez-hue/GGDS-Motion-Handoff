# Motion Handoff — Field Password (Web)

| | |
|---|---|
| **Componente** | Field Password |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-md` |
| **Categoría** | Default |
| **Fecha** | 2026-06-23 |

---

## Motion Specification

Field Password en Web usa motion de estado interno. No aplica entrada/salida de viewport.

Estados cubiertos: `Enabled`, `Hovered`, `Focused`, `Typing`, `Filled`, `Error`, `Disabled`.

La transición principal sucede en el anillo de foco externo, el borde del contenedor y el ícono de visibilidad (mostrar/ocultar). Sin variantes `Loading` ni `Readonly`.

En error: stroke rojo siempre; ring rojo solo en focus.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.field-password` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.field-password__control` | `border-color` | default | hover | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Trigger | Focus visible | `.field-password__control` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focus ring in | `.field-password__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 300 |
| 5 | Trigger | Input typing | `.field-password__control` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Typing settle | `.field-password` | `border-color` | focus | active | `motion-curve-md` | 300ms | 0 | 300 |
| 7 | Trigger | Toggle visibility | `.field-password__toggle` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Icon settle | `.field-password__toggle` | `opacity` | default | pressed/active | `motion-curve-md` | 300ms | 0 | 300 |
| 9 | Trigger | Validation error | `.field-password` | — | — | — | — | — | 0 | 0 |
| 10 | Response | Error feedback | `.field-password__ring`, helper | `border-color`, `opacity` | focus/default | error | `motion-curve-md` | 300ms | 0 | 300 |
| 11 | Trigger | Toggle disabled | `.field-password` | — | — | — | — | — | 0 | 0 |
| 12 | Response | Disabled state | `.field-password` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | `duration-300` | 300ms |

---

## Implementación CSS

```css
.field-password__control,
.field-password__ring,
.field-password__toggle {
  transition-property: border-color, opacity;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.field-password:disabled {
  transition: none;
}

@media (prefers-reduced-motion: reduce) {
  .field-password__control,
  .field-password__ring,
  .field-password__toggle {
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
| Error | stroke rojo siempre; ring rojo solo en focus |
| Hover | solo stroke, sin fill |
| Disabled | sin transición |
| Haptics | no aplica en Web |
| Coherencia | mismo patrón que `Field Text` |
