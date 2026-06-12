# Motion Handoff — Icon Button (Web)

| | |
|---|---|
| **Componente** | Icon Button |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-10 |

---

## Motion Specification

Icon Button en Web usa motion de estados internos (sin enter/exit de viewport): `default`, `hover`, `pressed`, `focus`, `disabled`, `loading`, `skeleton`.

Se anima `background-color`, `transform` (pressed), `opacity` (focus ring / loading) con `motion-curve-sm`.

`disabled` es instantáneo. En `prefers-reduced-motion` se aplica estado final sin transición.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.icon-button` | — | — | — | — | — |
| 2 | Response | Hover | `.icon-button` | `background-color` | default | hover | `motion-curve-sm` | 150ms |
| 3 | Trigger | Pointer down | `.icon-button` | — | — | — | — | — |
| 4 | Response | Pressed | `.icon-button` | `transform` | `scale(1)` | `scale(0.96)` | `motion-curve-sm` | 150ms |
| 5 | Trigger | Focus visible | `.icon-button` | — | — | — | — | — |
| 6 | Response | Focus ring | `.ring` | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms |
| 7 | Trigger | Loading on/off | `.icon-button` | — | — | — | — | — |
| 8 | Response | Loading crossfade | `.icon/.spinner` | `opacity` | 1/0 | 0.12/1 | `motion-curve-sm` | 150ms |
| 9 | Trigger | Skeleton on/off | `.icon-button` | — | — | — | — | — |
| 10 | Response | Skeleton | `.icon-button` | `background/opacity` | default | skeleton | `motion-curve-sm` | 150ms |
| 11 | Trigger | Disabled on | `.icon-button` | — | — | — | — | — |
| 12 | Response | Disabled | `.icon-button` | todas | — | final | — | 0ms |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |

---

## Implementación CSS

```css
.icon-button {
  transition-timing-function: var(--motion-curve-sm);
  transition-duration: var(--motion-duration-from-motion-curve-sm);
}

.icon-button:disabled {
  transition: none;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-curve-sm` para estados internos |
| Pressed | escala sutil para feedback táctil |
| Disabled | instantáneo |
| A11y | `prefers-reduced-motion` obligatorio |
| Radius | `4px` (match con Button en Figma) |
