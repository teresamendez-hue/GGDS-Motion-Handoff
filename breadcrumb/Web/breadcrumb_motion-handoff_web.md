# Motion Handoff — Breadcrumb (Web)

| | |
|---|---|
| **Componente** | Breadcrumb |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-17 |

---

## Motion Specification

Breadcrumb en Web usa motion de estado interno por item (sin enter/exit de viewport): `Enabled`, `Hovered`, `Pressed`, `Focused`, `Active`.

También contempla variante de componente `Type`: `Collapse` y `Expanded`.

En `Collapse`, cuando la persona usuaria presiona `...`, el breadcrumb se expande a `Expanded`.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.breadcrumb-item` | — | — | — | — | — |
| 2 | Response | Hovered | `.breadcrumb-item` | `color` | muted | text | `motion-curve-sm` | 150ms |
| 3 | Trigger | Pointer down | `.breadcrumb-item` | — | — | — | — | — |
| 4 | Response | Pressed | `.breadcrumb-item` | `opacity` | 1 | 0.75 | `motion-curve-sm` | 150ms |
| 5 | Trigger | Focus visible | `.breadcrumb-item` | — | — | — | — | — |
| 6 | Response | Focused | `.ring` | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms |
| 7 | Trigger | Click item | `.breadcrumb-item` | — | — | — | — | — |
| 8 | Response | Active | `.breadcrumb-item` | `color/weight` | muted | text/bold | `motion-curve-sm` | 150ms |
| 9 | Trigger | Click `...` (type collapse) | `.ellipsis` | — | collapse | expanded | `motion-curve-sm` | 150ms |
| 10 | Response | Expand reveal | `.breadcrumb-trail` | `opacity + translateY` | 0 + 4px | 1 + 0px | `motion-curve-sm` | 150ms |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |

---

## Implementación CSS

```css
.breadcrumb-item {
  transition-timing-function: var(--motion-curve-sm);
  transition-duration: var(--motion-duration-from-motion-curve-sm);
}

.breadcrumb-trail[data-expand="true"] {
  animation: breadcrumb-expand var(--motion-duration-from-motion-curve-sm) var(--motion-curve-sm);
}

@keyframes breadcrumb-expand {
  from { opacity: 0; transform: translateY(4px); }
  to { opacity: 1; transform: translateY(0); }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-curve-sm` para estados internos |
| Type variant | soportar `Collapse` y `Expanded` |
| Mobile | `...` expande trail cuando hay demasiados niveles |
| Exit | no aplica |
| A11y | `focus-visible` + `prefers-reduced-motion` |
