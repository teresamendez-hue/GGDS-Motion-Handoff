# Motion Handoff — Radio Button (Web)

| | |
|---|---|
| **Componente** | Radio Button |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-08 |

---

## Motion Specification

Radio Button en Web usa motion de estados internos (sin enter/exit de viewport): default, hover, pressed, focus, selected, disabled.

La transición principal usa `motion-curve-sm` sobre `border-color`, `background-color`, `transform` (pressed) y `opacity` (focus ring / indicador).

`disabled` es instantáneo. En `prefers-reduced-motion` se aplica estado final sin transición.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | componente | — | — | — | — | — |
| 2 | Response | Hover | contenedor | `border-color` | default | hover | `motion-curve-sm` | 150ms |
| 3 | Trigger | Pointer down | componente | — | — | — | — | — |
| 4 | Response | Pressed | contenedor | `transform` | `scale(1)` | scale sutil | `motion-curve-sm` | 150ms |
| 5 | Trigger | Focus visible | componente | — | — | — | — | — |
| 6 | Response | Focus ring | ring | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms |
| 7 | Trigger | Toggle selected | control | — | — | — | — | — |
| 8 | Response | Selected | indicador | `opacity/background` | off | on | `motion-curve-sm` | 150ms |
| 9 | Trigger | Disabled on | componente | — | — | — | — | — |
| 10 | Response | Disabled | todas | — | — | final | — | 0ms |

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
| Token | `motion-curve-sm` para estados internos |
| Disabled | instantáneo |
| Focus | visible solo en navegación de teclado |
| A11y | respetar `prefers-reduced-motion` |
