# Motion Handoff — Select (Web)

| | |
|---|---|
| **Componente** | Select |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-md` / `motion-exit` |
| **Categoría** | Default |
| **Fecha** | 2026-06-23 |

---

## Motion Specification

Select en Web usa motion de estado interno del field y motion del dropdown integrado.

Estados del field: `Enabled`, `Hovered`, `Focused`, `Filled`, `Error`, `Disabled`.

Al hacer **focus o click** en el field se despliega el dropdown (`opacity` + `translateY`) y el chevron rota 180°. Cierra al seleccionar una opción, Escape o click fuera. El valor elegido persiste en el trigger (selección única, sin Chip).

Al reabrir, la lista muestra la opción actual resaltada y hace scroll para mantenerla visible — motion de la fila en [Option Item](../../option-item/Web/option-item_motion-handoff_web.html).

El contenido del panel son **Option Items** sin Checkbox — motion documentado en [Option Item](../../option-item/Web/option-item_motion-handoff_web.html).

Sin `Skeleton`, `Loading` ni `Readonly`.

En error: stroke rojo siempre; ring rojo solo en focus.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.select` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.select__wrapper` | `border-color` | default | hover | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Trigger | Focus visible | trigger del field | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focus ring in | `.select__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 300 |
| 5 | Trigger | Field trigger press | `.select__trigger` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Trigger feedback | chevron | `opacity` | default | pressed | `motion-curve-md` | 300ms | 0 | 300 |
| 7 | Trigger | Panel open | focus o click en field | — | — | — | — | — | 0 | 0 |
| 8 | Response | Panel enter | `.select__panel` | `opacity`, `translateY` | `0`, `-6px` | `1`, `0` | `motion-curve-md` | 300ms | 0 | 300 |
| 9 | Response | Chevron rotate | `.select__icon` | `transform` | `0deg` | `180deg` | `motion-curve-md` | 300ms | 0 | 300 |
| 10 | Trigger | Option selected | Option Item | — | — | — | — | — | 0 | 0 |
| 11 | Trigger | Panel close | selección / Escape / click fuera | — | — | — | — | — | 0 | 0 |
| 12 | Response | Panel exit | `.select__panel` | `opacity`, `translateY` | `1`, `0` | `0`, `-6px` | `motion-exit` | 200ms | 0 | 200 |
| 13 | Response | Chevron reset | `.select__icon` | `transform` | `180deg` | `0deg` | `motion-exit` | 200ms | 0 | 200 |
| 14 | Trigger | Validation error | `.select` | — | — | — | — | — | 0 | 0 |
| 15 | Response | Error feedback | ring, helper | `border-color`, `color` | focus/default | error | `motion-curve-md` | 300ms | 0 | 300 |
| 16 | Trigger | Toggle disabled | `.select` | — | — | — | — | — | 0 | 0 |
| 17 | Response | Disabled state | `.select` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

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

El dropdown del field consume [Option Items](../../option-item/Web/option-item_motion-handoff_web.html) con motion propio en su handoff. Sin Checkbox en las filas (a diferencia de Multiple Select).

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet CSS de este documento.
- **Apertura:** focus o click abre el dropdown integrado del field.
- **Cierre:** selección de opción cierra el panel con `motion-exit`.
- **Re-apertura:** scroll y resaltado de la opción actual — lógica en Option Item + comportamiento de lista.
- **Haptics:** no aplica en Web.
- **QA final:** validar en navegador con tokens semánticos del design system.

---

## Implementación CSS

```css
.select__wrapper,
.select__ring {
  transition-property: border-color, opacity, color;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.select__icon {
  transition-property: transform, opacity;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.select__panel {
  transition-property: opacity, transform;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.select.is-panel-closing .select__panel,
.select.is-panel-closing .select__icon {
  transition-duration: var(--motion-duration-from-motion-exit);
  transition-timing-function: var(--motion-exit);
}

.select.is-disabled .select__wrapper {
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
| Selección | única; cierra panel al elegir |
| vs Multiple Select | sin Chips ni Checkbox en filas |
| Focus ring | anillo externo sobre el field completo |
| Error | stroke rojo siempre; ring rojo solo en focus |
| Hover | solo stroke del contenedor |
| Disabled | sin transición; cierra panel |
| Scroll | nativo en lista; re-entrada con opción visible |
