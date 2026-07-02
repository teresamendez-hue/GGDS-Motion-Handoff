# Motion Handoff — Tabs (Web)

| | |
|---|---|
| **Componente** | Tabs |
| **Plataforma** | Web |
| **Owner** | Ezequiel Arguello |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-sm` (estado) · `motion-curve-md` (indicador) |
| **Categoría** | Default (estado) / Guía (indicador) |
| **Variantes** | Segmented Item · Line Item |
| **Fecha** | 2026-07-02 |

---

## Motion Specification

Tabs en Web usa motion de **estados internos**, sin enter/exit de viewport: `Enabled`, `Hovered`, `Pressed`, `Focused`, `Selected`, `Disabled`. Aplica igual a las dos variantes — Segmented Item (fondo relleno) y Line Item (borde inferior) — que comparten el mismo contrato de motion y difieren solo en qué propiedad visual representa la selección.

Los cambios de `Hovered`, `Pressed` y `Focused` son microinteracciones puntuales sobre `border-color` / `background-color` / `color`, y usan `motion-curve-sm` — el mismo token que Button, para mantener coherencia entre controles interactivos frecuentes del sistema.

El cambio de `Selected` introduce además un **indicador de posición** (fondo relleno en Segmented, línea inferior en Line) que se desplaza espacialmente entre tabs. Ese desplazamiento (`transform: translateX` + ajuste de `width`) usa `motion-curve-md`, distinto del token de estado, porque es un movimiento guiado entre posiciones y no un simple cambio de color — igual criterio que el indicador de Tabs en Material 3.

El fade de color en label/ícono (no seleccionado ↔ seleccionado) corre en paralelo al movimiento del indicador, no en secuencia, para que se perciba como un solo gesto.

`Disabled` es instantáneo, sin transición. En `prefers-reduced-motion` se aplica el estado final sin transición, incluyendo el indicador.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.tab-item` | — | — | — | — | — |
| 2 | Response | Hovered | `.tab-item` | `border-color` / `background-color` | Enabled | Hovered | `motion-curve-sm` | 150ms |
| 3 | Trigger | Pointer down | `.tab-item` | — | — | — | — | — |
| 4 | Response | Pressed | `.tab-item` | `background-color` (overlay sutil) | Hovered | Pressed | `motion-curve-sm` | 150ms |
| 5 | Trigger | Focus visible (teclado) | `.tab-item` | — | — | — | — | — |
| 6 | Response | Focus ring | `.tab-item__ring` | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms |
| 7 | Trigger | Cambio de tab seleccionado | `.tabs` | — | — | — | — | — |
| 8 | Response | Indicador — Segmented | `.tab-indicator` | `transform: translateX` + `width` | posición anterior | posición del tab activo | `motion-curve-md` | 300ms |
| 9 | Response | Indicador — Line | `.tab-indicator` | `transform: translateX` + `width` | posición anterior | posición del tab activo | `motion-curve-md` | 300ms |
| 10 | Response | Label/ícono — saliente | `.tab-item.was-selected` | `color` | color seleccionado | color enabled | `motion-curve-sm` | 150ms |
| 11 | Response | Label/ícono — entrante | `.tab-item.is-selected` | `color` / `font-weight` | color enabled | color seleccionado | `motion-curve-sm` | 150ms |
| 12 | Trigger | Disabled on | `.tab-item` | — | — | — | — | — |
| 13 | Response | Disabled | `.tab-item` | todas | — | final | — | 0ms |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor | Uso |
|---|---|---|---|---|---|
| `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms | Hovered, Pressed, Focused, fade de color |
| `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | `duration-300` | 300ms | Movimiento del indicador de selección |

---

## Implementación CSS

```css
/* Consumir tokens semánticos, no primitivos hardcodeados */
.tab-item {
  transition-property: border-color, background-color, color;
  transition-duration: var(--motion-duration-from-motion-curve-sm);
  transition-timing-function: var(--motion-curve-sm);
}

.tab-item:disabled,
.tab-item[aria-disabled="true"] {
  transition: none;
}

.tab-indicator {
  transition-property: transform, width;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

@media (prefers-reduced-motion: reduce) {
  .tab-item,
  .tab-indicator {
    transition: none;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token de estado | `motion-curve-sm` para Hovered/Pressed/Focused, coherente con Button |
| Token de indicador | `motion-curve-md` por ser desplazamiento espacial, no color plano |
| Rebote | Ninguno — Tabs es navegación/selección, no expresivo |
| Fade | Color de label/ícono en paralelo al movimiento del indicador, no en secuencia |
| Disabled | Instantáneo, sin transición |
| A11y | `prefers-reduced-motion` obligatorio, incluyendo el indicador |
