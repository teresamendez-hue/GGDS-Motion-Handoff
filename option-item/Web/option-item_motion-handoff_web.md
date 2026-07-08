# Motion Handoff — Option Item (Web)

| | |
|---|---|
| **Componente** | Option Item |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-23 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview Web (HTML)](./option-item_motion-handoff_web.html) | Estados internos con curva y duración del Token Mapping. |

---

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-sm` | `motion-spring-sm` |
| Curva / física | easing-standard · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Estados de fila | enabled, hover, pressed, focus, selected, selected+focus | enabled, pressed, focus, selected, selected+focus |
| Disabled | no aplica | no aplica |
| Viewport enter/exit | no aplica | no aplica |
| Hijos opcionales | Badge, Thumbnail, Checkbox | Badge, Thumbnail, Checkbox |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet CSS de este documento.
- **Scope:** solo la fila (`OptionItem`). El motion de **Badge**, **Thumbnail** y **Checkbox** anidados se documenta en sus handoffs respectivos.
- **Sin disabled:** este componente no define estado disabled.
- **Skeleton:** no aplica en este handoff; loading/skeleton lo resuelve implementación del consumidor.
- **QA final:** validar en navegador con tokens semánticos del design system.

---

## Composición / Consumidores

Option Item es atómico. Los consumidores orquestan el contexto; el motion de apertura/cierre del contenedor no va acá.

| Consumidor | Uso |
|---|---|
| **Select / Multiple Select** | Al focus del field, la lista de opciones se despliega **inline** en el mismo componente (no consume Dropdown). Cada fila es un Option Item. |
| **Dropdown** (slot) | Otros casos con panel y slot; Option Items dentro del slot. |
| **Search** | Filas de resultados. |
| **Field Phone Number** (Web) | Países u opciones en lista desplegable del field. |

En **Multiple Select**, la fila puede llevar **Checkbox** anidado: la fila anima `background-color` con `motion-curve-sm`; el check usa el handoff de [Checkbox](../../checkbox/Web/checkbox_motion-handoff_web.html).

---

## Motion Specification

Option Item en Web anima estados internos de la fila con `motion-curve-sm` sobre `background-color` y `opacity` del focus ring.

Sin enter/exit de viewport. Sin scale en la fila. Sin estado disabled.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | fila | — | — | — | — | — |
| 2 | Response | Hover | fila | `background-color` | enabled | hover | `motion-curve-sm` | 150ms |
| 3 | Trigger | Pointer down | fila | — | — | — | — | — |
| 4 | Response | Pressed | fila | `background-color` | hover/default | pressed | `motion-curve-sm` | 150ms |
| 5 | Trigger | Focus visible | fila | — | — | — | — | — |
| 6 | Response | Focus ring | ring | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms |
| 7 | Trigger | Select option | fila | — | — | — | — | — |
| 8 | Response | Selected | fila | `background-color` | default | selected | `motion-curve-sm` | 150ms |
| 9 | Trigger | Focus + selected | fila | — | — | — | — | — |
| 10 | Response | Selected focused | fila + ring | `background-color`, `opacity` | selected | selected+focus | `motion-curve-sm` | 150ms |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |

---

## Implementación CSS

```css
.option-item {
  transition-property: background-color, opacity;
  transition-duration: var(--motion-duration-from-motion-curve-sm);
  transition-timing-function: var(--motion-curve-sm);
}

.option-item__ring {
  transition: opacity var(--motion-duration-from-motion-curve-sm) var(--motion-curve-sm);
}

@media (prefers-reduced-motion: reduce) {
  .option-item,
  .option-item__ring {
    transition: none;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-curve-sm` para estados de fila |
| Hijos | Badge / Thumbnail / Checkbox — motion en handoffs propios |
| Disabled | no aplica en este componente |
| Focus | visible solo en navegación de teclado |
| A11y | respetar `prefers-reduced-motion` |
| Contenedor | motion de despliegue en Select, Dropdown o field — handoff aparte |
