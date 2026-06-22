# Motion Handoff — Quick Action (Web)

| | |
|---|---|
| **Componente** | Quick Action |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-17 |

---

## Motion Specification

Quick Action en Web se comporta como componente de estado interno. No usa entrada/salida de viewport.

Los estados cubiertos son `Enabled`, `Hovered`, `Focused`, `Pressed`, `Disabled` y `Skeleton`.

Todos los cambios con transición usan `motion-curve-sm`. `Disabled` aplica estado final sin transición para evitar ambiguedad de interactividad.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.quick-action` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.quick-action` | `background-color`, `border-color` | default | hover | `motion-curve-sm` | 150ms | 0 | 150 |
| 3 | Trigger | Pointer down | `.quick-action` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Pressed | `.quick-action` | `transform`, `opacity` | `scale(1)`, `1` | `scale(0.985)`, `0.92` | `motion-curve-sm` | 150ms | 0 | 150 |
| 5 | Trigger | Focus visible | `.quick-action` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Focused | `.quick-action` | `outline` | 0 | visible | `motion-curve-sm` | 150ms | 0 | 150 |
| 7 | Trigger | Toggle skeleton | `.quick-action` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Skeleton | `.quick-action` | `surface` | normal | shimmer | `motion-curve-sm` | 150ms + loop visual | 0 | 150 |
| 9 | Trigger | Toggle disabled | `.quick-action` | — | — | — | — | — | 0 | 0 |
| 10 | Response | Disabled | `.quick-action` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |

---

## Implementación CSS

```css
.quick-action {
  transition:
    background-color var(--motion-duration-from-motion-curve-sm) var(--motion-curve-sm),
    border-color var(--motion-duration-from-motion-curve-sm) var(--motion-curve-sm),
    transform var(--motion-duration-from-motion-curve-sm) var(--motion-curve-sm),
    opacity var(--motion-duration-from-motion-curve-sm) var(--motion-curve-sm);
}

.quick-action:disabled {
  transition: none;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo motion de estado interno |
| Hover | solo Web |
| Disabled | sin transición |
| A11y | `prefers-reduced-motion` aplica estado final |
| Coherencia | mismo patrón de `button` e `icon-button` |
