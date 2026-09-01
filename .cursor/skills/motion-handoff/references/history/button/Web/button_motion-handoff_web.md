# Motion Handoff — Button (Web)

| | |
|---|---|
| **Componente** | Button |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-sm` |
| **Categoría** | Default |
| **Variantes** | Size S, M |
| **Fecha** | 2026-06-05 |

---

## Motion Specification

Button en Web usa motion de **estados internos** (sin enter/exit de viewport): `hover`, `pressed`, `focus`, `loading`, `disabled`, `skeleton`.

Las transiciones principales animan `background-color`, `color`, `transform` (solo pressed), `opacity` (focus ring, loading/skeleton crossfade) con `motion-curve-sm`.

`disabled` es instantáneo. En `prefers-reduced-motion` se aplica estado final sin transición.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.button` | — | — | — | — | — |
| 2 | Response | Hover | `.button` | `background-color` | default | hover | `motion-curve-sm` | 150ms |
| 3 | Trigger | Pointer down | `.button` | — | — | — | — | — |
| 4 | Response | Pressed | `.button` | `transform` | `scale(1)` | `scale(0.98)` | `motion-curve-sm` | 150ms |
| 5 | Trigger | Focus visible | `.button` | — | — | — | — | — |
| 6 | Response | Focus ring | `.button__ring` | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms |
| 7 | Trigger | Loading on/off | `.button` | — | — | — | — | — |
| 8 | Response | Loading crossfade | `.label/.spinner` | `opacity` | 1/0 | 0.4/1 | `motion-curve-sm` | 150ms |
| 9 | Trigger | Skeleton resolve | `.button` | — | — | — | — | — |
| 10 | Response | Skeleton -> content | `.skeleton/.label` | `opacity` | 1/0 | 0/1 | `motion-curve-sm` | 150ms |
| 11 | Trigger | Disabled on | `.button` | — | — | — | — | — |
| 12 | Response | Disabled | `.button` | todas | — | final | — | 0ms |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |

---

## Implementación CSS

```css
/* Consumir token semántico, no primitivo hardcodeado */
.component {
  transition-timing-function: var(--motion-curve-sm);
  transition-duration: var(--motion-duration-from-motion-curve-sm);
}

.component:disabled {
  transition: none;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-curve-sm` para todos los estados internos |
| Pressed | scale sutil + color para affordance táctil |
| Disabled | instantáneo |
| A11y | `prefers-reduced-motion` obligatorio |
