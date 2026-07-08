# Motion Handoff — Time Picker (Web)

| | |
|---|---|
| **Componente** | Time Picker |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-md` / `motion-curve-sm` / `motion-exit` |
| **Categoría** | Default |
| **Fecha** | 2026-07-06 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview Web (HTML)](./time-picker_motion-handoff_web.html) | Field + overlay con lista de horarios. Curvas CSS en preview. |
| [Time Picker App companion](../App/time-picker_motion-handoff_app.html) | Paridad de flujo — App usa Bottom Sheet en lugar de popover. |

> En Web el menú de horarios es un **overlay integrado** al field (patrón Select). La selección cierra en **un clic** — sin footer de confirmación.

---

## Paridad Web ↔ App

| Aspecto | Web (Time Picker) | App (Time Picker) |
|---------|-------------------|-------------------|
| Token field | `motion-curve-md` | `motion-spring-md` |
| Contenedor | popover / overlay integrado | [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) |
| Filas | [Option Item](../../option-item/Web/option-item_motion-handoff_web.html) | [Option Item](../../option-item/App/option-item_motion-handoff_app.html) |
| Confirmación | un clic cierra y persiste | un tap cierra y persiste |
| Smart default | visible en el field desde el inicio | al abrir sheet (scroll al sugerido) |
| Hover en field | sí | no |
| Formato | 12h AM/PM (Figma) | 12h AM/PM (Figma) |
| Haptics | no | vía Option Item App |

---

## Motion Specification

Time Picker en Web combina motion de estado interno del field y motion del **overlay integrado** de opciones (mismo patrón que [Select](../../select/Web/select_motion-handoff_web.html)).

Estados del field: `Enabled`, `Hovered`, `Focused`, `Filled`, `Error`, `Disabled`.

Al hacer **focus o click** en el field se despliega el overlay (`opacity` + `translateY` ~6px) anclado al borde inferior del input. Con detección de colisiones (**flip**): si no hay espacio abajo, el panel abre hacia arriba — misma curva, dirección de `translateY` invertida. El ícono de reloj es **estático**.

**Smart default:** el field muestra desde el inicio la hora local redondeada al **próximo bloque de 15 min** (ej. 09:12 → 09:15 AM). Al abrir, la lista hace **scroll instantáneo** a ese valor (o al confirmado previamente) con la fila en Selected.

**Selección:** un clic en [Option Item](../../option-item/Web/option-item_motion-handoff_web.html) persiste la hora y cierra el overlay con `motion-exit`.

**Cierre:** selección, Escape o click fuera.

Sin `Skeleton`, `Loading` ni `Readonly`.

En error: stroke rojo siempre; ring rojo solo en focus.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.tp` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.tp__wrapper` | `border-color` | default | hover | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Trigger | Focus visible | field trigger | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focus ring in | `.tp__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 300 |
| 5 | Trigger | Field trigger press | `.tp__trigger` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Trigger feedback | trigger | `opacity` | default | pressed | `motion-curve-md` | 300ms | 0 | 300 |
| 7 | Trigger | Panel open | focus o click en field | — | — | — | — | — | 0 | 0 |
| 8 | Response | Panel enter | `.tp__panel` | `opacity`, `translateY` | `0`, `±6px` | `1`, `0` | `motion-curve-md` | 300ms | 0 | 300 |
| 9 | Response | Auto-scroll a valor | lista interna | `scrollTop` | offset | target | none | instantáneo | 0 | 0 |
| 10 | Trigger | Pointer enter | Option Item | — | — | — | — | — | 0 | 0 |
| 11 | Response | Row hovered | Option Item | `background-color` | default | hover | `motion-curve-sm` | 150ms | 0 | 150 |
| 12 | Trigger | Pointer down | Option Item | — | — | — | — | — | 0 | 0 |
| 13 | Response | Row pressed | Option Item | `background-color` | hover/default | pressed | `motion-curve-sm` | 150ms | 0 | 150 |
| 14 | Trigger | Option selected | Option Item | — | — | — | — | — | 0 | 0 |
| 15 | Response | Field value update | `.tp__value` | contenido | sugerido | hora elegida | none | instantáneo | 0 | 0 |
| 16 | Trigger | Panel close | selección / Escape / click fuera | — | — | — | — | — | 0 | 0 |
| 17 | Response | Panel exit | `.tp__panel` | `opacity`, `translateY` | `1`, `0` | `0`, `±6px` | `motion-exit` | 200ms | 0 | 200 |
| 18 | Trigger | Validation error | `.tp` | — | — | — | — | — | 0 | 0 |
| 19 | Response | Error feedback | ring, helper | `border-color`, `color` | focus/default | error | `motion-curve-md` | 300ms | 0 | 300 |
| 20 | Trigger | Toggle disabled | `.tp` | — | — | — | — | — | 0 | 0 |
| 21 | Response | Disabled state | `.tp` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

Filas 10–13: motion de fila en [Option Item](../../option-item/Web/option-item_motion-handoff_web.html).

---

## Token Mapping

| Uso | Token semántico | Curva primitiva | Valor CSS | Duración |
|---|---|---|---|---|
| Estados del field (hover, focus, error, pressed) | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Panel enter | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Panel exit | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | 200ms |
| Option Item (hover, pressed, selected) | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | 150ms |
| Auto-scroll lista | none | — | — | instantáneo |

Enter del panel y estados del field comparten `motion-curve-md`. Solo la salida del panel usa `motion-exit`.

---

## Composición

| Pieza | Handoff |
|---|---|
| Field + overlay integrado | este documento |
| Filas de horarios (slots 15 min) | [Option Item](../../option-item/Web/option-item_motion-handoff_web.html) |
| Consumidor App (referencia) | [Time Picker App](../App/time-picker_motion-handoff_app.html) |

El overlay del field consume [Option Items](../../option-item/Web/option-item_motion-handoff_web.html) con motion propio en su handoff. Sin Bottom Sheet en Web.

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet CSS de este documento.
- **Apertura:** focus o click abre el overlay integrado.
- **Smart default:** redondeo al próximo bloque de 15 min; visible en el field.
- **Flip:** sin token adicional — invertir anclaje y `translateY` del panel.
- **Selección:** un clic cierra panel y persiste valor.
- **Scroll inicial:** instantáneo al valor del field.
- **Haptics:** no aplica en Web.
- **QA final:** validar en navegador con tokens semánticos y `prefers-reduced-motion`.

---

## Implementación CSS

```css
.tp__wrapper,
.tp__ring,
.tp__helper {
  transition-property: border-color, opacity, color;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.tp__trigger {
  transition-property: opacity;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.tp__panel {
  transition-property: opacity, transform;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.tp.is-panel-closing .tp__panel {
  transition-duration: var(--motion-duration-from-motion-exit);
  transition-timing-function: var(--motion-exit);
}

.tp.is-flip-up .tp__panel {
  top: auto;
  bottom: calc(100% + 6px);
  transform: translateY(6px);
}

.tp.is-panel-open.is-flip-up .tp__panel,
.tp.is-panel-closing.is-flip-up .tp__panel {
  transform: translateY(0);
}

.tp.is-panel-closing.is-flip-up .tp__panel {
  transform: translateY(6px);
}

.tp.is-disabled .tp__wrapper {
  transition: none;
  opacity: 0.45;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | field + overlay integrado |
| vs Select Web | misma familia tokens; sin chevron rotate |
| vs App | popover vs Bottom Sheet; smart default en field |
| Option Item | handoff enlazado — no duplicar |
| Flip | layout; mismo motion-curve-md |
| Scroll inicial | instantáneo |
| Skeleton | fuera de scope |
| Disabled | sin transición; cierra panel |
