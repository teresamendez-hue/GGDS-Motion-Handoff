# Motion Handoff — Switch (Web)

| | |
|---|---|
| **Componente** | Switch |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-11 |

---

## Motion Specification

Switch en Web usa motion de estados internos (sin enter/exit de viewport): `Enabled`, `Hovered`, `Pressed`, `Focused`, `Disabled`; con variantes `Active` (`False/True`) y `Skeleton` (`False/True`).

Se anima `transform` del thumb, `background-color` del track y `opacity` del focus ring con `motion-curve-sm`.

`Disabled` es instantáneo. En `prefers-reduced-motion` se aplica estado final sin transición.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Tap/click | `.switch` | — | — | — | — | — |
| 2 | Response | Active toggle | `.thumb` | `transform` | x=3px | x=21px | `motion-curve-sm` | 150ms |
| 3 | Response | Active toggle | `.switch` | `background-color` | off track | on track | `motion-curve-sm` | 150ms |
| 4 | Trigger | Hover state | `.switch` | — | — | — | — | — |
| 5 | Response | Hover | `.switch` | `background-color` | enabled | hovered | `motion-curve-sm` | 150ms |
| 6 | Trigger | Pressed state | `.switch` | — | — | — | — | — |
| 7 | Response | Pressed | `.switch` | `transform` | `scale(1)` | `scale(0.98)` | `motion-curve-sm` | 150ms |
| 8 | Trigger | Focus state | `.switch` | — | — | — | — | — |
| 9 | Response | Focus ring | `.ring` | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms |
| 10 | Trigger | Skeleton state | `.switch` | — | — | — | — | — |
| 11 | Response | Skeleton | `.switch/.thumb` | `background/opacity` | normal | skeleton | `motion-curve-sm` | 150ms |
| 12 | Trigger | Disabled state | `.switch` | — | — | — | — | — |
| 13 | Response | Disabled | `.switch` | todas | — | final | — | 0ms |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |

---

## Implementación CSS

```css
.switch {
  transition-timing-function: var(--motion-curve-sm);
  transition-duration: var(--motion-duration-from-motion-curve-sm);
}

.switch:disabled {
  transition: none;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-curve-sm` para estados internos |
| Active | thumb y track sincronizados |
| Exit | no aplica |
| Disabled | instantáneo |
| A11y | `prefers-reduced-motion` |
