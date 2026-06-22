# Motion Handoff — Field Prefix (Web)

| | |
|---|---|
| **Componente** | Field Prefix |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-md` |
| **Categoría** | Default |
| **Fecha** | 2026-06-18 |

---

## Motion Specification

Field Prefix en Web usa motion de estado interno. No aplica entrada/salida de viewport.

Estados cubiertos: `Enabled`, `Hovered`, `Focused`, `Typing`, `Filled`, `Error`, `Disabled`.

El prefijo (ej. `+595`, `$`, `@`) se mantiene estable; la animación se aplica al contenedor, anillo de foco externo y helper/status para mantener legibilidad durante el ingreso.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.field-prefix` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.field-prefix` | `border-color`, `background-color` | default | hover | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Trigger | Focus visible | `.field-prefix__control` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focus ring in | `.field-prefix__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 300 |
| 5 | Trigger | Input typing | `.field-prefix__control` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Typing settle | `.field-prefix` | `border-color` | focus | active | `motion-curve-md` | 300ms | 0 | 300 |
| 7 | Trigger | Validation error | `.field-prefix` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Error feedback | `.field-prefix__ring`, `.field-prefix__helper` | `border-color`, `color` | focus/default | error | `motion-curve-md` | 300ms | 0 | 300 |
| 9 | Trigger | Toggle disabled | `.field-prefix` | — | — | — | — | — | 0 | 0 |
| 10 | Response | Disabled state | `.field-prefix` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | `duration-300` | 300ms |

---

## Implementación CSS

```css
.field-prefix {
  transition:
    border-color var(--motion-duration-from-motion-curve-md) var(--motion-curve-md),
    background-color var(--motion-duration-from-motion-curve-md) var(--motion-curve-md),
    opacity var(--motion-duration-from-motion-curve-md) var(--motion-curve-md);
}

.field-prefix__ring {
  transition:
    opacity var(--motion-duration-from-motion-curve-md) var(--motion-curve-md),
    border-color var(--motion-duration-from-motion-curve-md) var(--motion-curve-md);
}

.field-prefix__helper {
  transition:
    color var(--motion-duration-from-motion-curve-md) var(--motion-curve-md),
    opacity var(--motion-duration-from-motion-curve-md) var(--motion-curve-md);
}

.field-prefix.is-disabled,
.field-prefix[aria-disabled="true"] {
  transition: none;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo motion de estado interno |
| Prefix | estable, sin animación independiente |
| Focus ring | anillo externo único (sin doble ring) |
| Disabled | sin transición |
| A11y | `prefers-reduced-motion` aplica estado final |
