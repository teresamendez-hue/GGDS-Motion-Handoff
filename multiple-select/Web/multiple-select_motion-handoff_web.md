# Motion Handoff — Multiple Select (Web)

| | |
|---|---|
| **Componente** | Multiple Select |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-md` / `motion-exit` |
| **Categoría** | Default |
| **Fecha** | 2026-06-23 |

---

## Motion Specification

Multiple Select en Web usa motion de estado interno del field y motion del dropdown integrado.

Estados del field: `Enabled`, `Hovered`, `Focused`, `Filled` (con chips), `Error`, `Disabled`.

Al hacer focus o click en el field se despliega el dropdown (opacity + `translateY`). El panel permanece abierto mientras el usuario selecciona opciones; cierra con Escape o click fuera.

El contenido del panel son **Option Items** (con checkbox en Multiple Select) — motion en [Option Item](../../option-item/Web/option-item_motion-handoff_web.html).

Las selecciones persisten en el field como [Chips](../../chip/Web/chip_motion-handoff_web.html) — motion en handoff de Chip.

Sin `Skeleton` ni `Loading` en este documento.

En error: stroke rojo siempre; ring rojo solo en focus.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.multiple-select` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.multiple-select__wrapper` | `border-color` | default | hover | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Trigger | Focus visible | trigger del field | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focus ring in | `.multiple-select__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 300 |
| 5 | Trigger | Field trigger press | `.multiple-select__trigger` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Trigger feedback | chevron / trigger | `opacity` | default | pressed | `motion-curve-md` | 300ms | 0 | 300 |
| 7 | Trigger | Panel open | focus o click en field | — | — | — | — | — | 0 | 0 |
| 8 | Response | Panel enter | `.multiple-select__panel` | `opacity`, `translateY` | `0`, `-6px` | `1`, `0` | `motion-curve-md` | 300ms | 0 | 300 |
| 9 | Response | Chevron rotate | `.multiple-select__icon` | `transform` | `0deg` | `180deg` | `motion-curve-md` | 300ms | 0 | 300 |
| 10 | Trigger | Panel close | Escape / click fuera | — | — | — | — | — | 0 | 0 |
| 11 | Response | Panel exit | `.multiple-select__panel` | `opacity`, `translateY` | `1`, `0` | `0`, `-6px` | `motion-exit` | 200ms | 0 | 200 |
| 12 | Response | Chevron reset | `.multiple-select__icon` | `transform` | `180deg` | `0deg` | `motion-exit` | 200ms | 0 | 200 |
| 13 | Trigger | Validation error | `.multiple-select` | — | — | — | — | — | 0 | 0 |
| 14 | Response | Error feedback | ring, helper | `border-color`, `color` | focus/default | error | `motion-curve-md` | 300ms | 0 | 300 |
| 15 | Trigger | Toggle disabled | `.multiple-select` | — | — | — | — | — | 0 | 0 |
| 16 | Response | Disabled state | `.multiple-select` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Uso | Token semántico | Curva primitiva | Valor CSS | Duración |
|---|---|---|---|---|
| Estados del field (hover, focus, error, pressed) | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Panel enter + chevron open | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Panel exit + chevron close | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | 200ms |

Enter y estados del field comparten `motion-curve-md`. Solo la salida del panel usa `motion-exit`.

---

## Composición

| Pieza | Handoff |
|---|---|
| Dropdown del field (panel vacío en preview) | este documento |
| Filas de opciones | [Option Item](../../option-item/Web/option-item_motion-handoff_web.html) |
| Chips en el field | [Chip](../../chip/Web/chip_motion-handoff_web.html) |

El dropdown del field consume [Option Items](../../option-item/Web/option-item_motion-handoff_web.html) con motion propio en su handoff. Las opciones seleccionadas se representan en el field como [Chips](../../chip/Web/chip_motion-handoff_web.html).

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet CSS de este documento.
- **Composición:** focus/click abre el dropdown integrado del field (este handoff). Contenido del panel → [Option Items](../../option-item/Web/option-item_motion-handoff_web.html).
- **Persistencia:** selecciones visibles como [Chips](../../chip/Web/chip_motion-handoff_web.html) en el field — motion en handoff de Chip.
- **Scroll:** nativo en la lista; sin token de motion.
- **Haptics:** no aplica en Web.
- **QA final:** validar en navegador con tokens semánticos del design system.

---

## Implementación CSS

```css
.multiple-select__wrapper,
.multiple-select__ring {
  transition-property: border-color, opacity, color;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.multiple-select__panel {
  transition-property: opacity, transform;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.multiple-select.is-panel-closing .multiple-select__panel,
.multiple-select.is-panel-closing .multiple-select__icon {
  transition-duration: var(--motion-duration-from-motion-exit);
  transition-timing-function: var(--motion-exit);
}

.multiple-select.is-disabled .multiple-select__wrapper {
  transition: none;
  opacity: 0.45;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope field | estados del contenedor + apertura/cierre del dropdown |
| Contenido del panel | Option Items — handoff aparte |
| Chips | [Chip](../../chip/Web/chip_motion-handoff_web.html) — handoff aparte |
| Panel abierto | permanece durante selección múltiple |
| Focus ring | anillo externo sobre el field completo |
| Error | stroke rojo siempre; ring rojo solo en focus |
| Hover | solo stroke del contenedor |
| Disabled | sin transición; cierra panel |
| Scroll | nativo, sin token |
