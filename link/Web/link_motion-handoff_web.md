# Motion Handoff — Link (Web)

| | |
|---|---|
| **Componente** | Link |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-17 |

---

## Motion Specification

Link en Web usa motion de estado interno. No aplica entrada/salida de viewport.

Estados cubiertos: `Enabled`, `Hovered`, `Focused`, `Pressed`, `Disabled`, `Visited`.

`Visited` se modela como estado visual estable (color) sin protagonismo de motion.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.link` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.link` | `color` | enabled | hovered | `motion-curve-sm` | 150ms | 0 | 150 |
| 3 | Trigger | Pointer down | `.link` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Pressed | `.link` | `opacity`, `translateY` | 1, 0px | 0.85, 1px | `motion-curve-sm` | 150ms | 0 | 150 |
| 5 | Trigger | Focus visible | `.link` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Focused | `.link` | `outline` | hidden | visible | `motion-curve-sm` | 150ms | 0 | 150 |
| 7 | Trigger | Toggle visited | `.link` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Visited | `.link` | `color` | enabled | visited | `motion-curve-sm` | 150ms | 0 | 150 |
| 9 | Trigger | Toggle disabled | `.link` | — | — | — | — | — | 0 | 0 |
| 10 | Response | Disabled | `.link` | `opacity` | estado actual | 0.45 | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |

---

## Implementación CSS

```css
.link {
  transition-property: color, opacity, transform;
  transition-duration: var(--motion-duration-from-motion-curve-sm);
  transition-timing-function: var(--motion-curve-sm);
}

.link:disabled {
  transition: none;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo motion de estado interno |
| Visited | estado visual estable (sin motion protagonista) |
| Disabled | instantáneo |
| A11y | `prefers-reduced-motion` aplica estado final |
