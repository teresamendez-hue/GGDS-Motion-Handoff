# Motion Handoff — Date Picker (Web)

| | |
|---|---|
| **Componente** | Date Picker |
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
| [Preview Web (HTML)](./date-picker_motion-handoff_web.html) | Field + popover con calendario integrado. Curvas CSS en preview. |
| [Date Picker App companion](../App/date-picker_motion-handoff_app.html) | Paridad de flujo — App usa sheet + confirmación explícita. |

> En Web **Single** cierra el popover al elegir día. **Range** usa footer **Cancelar / Aceptar** — Aceptar bloqueado hasta inicio + fin.

---

## Paridad Web ↔ App

| Aspecto | Web (Date Picker) | App (Date Picker) |
|---------|-------------------|-------------------|
| Token field | `motion-curve-md` | `motion-spring-md` |
| Contenedor | popover anclado al field | [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) |
| Confirmación | Single auto-close · Range footer Cancelar/Aceptar | **Aceptar** obligatorio |
| Footer Cancelar/Aceptar | **solo Range** | sí — [Button](../../button/App/button_motion-handoff_app.html) |
| Hover en field | sí | no |
| Calendar / Day Item | átomo interno en este handoff | [Calendar](../calendar/App/calendar_motion-handoff_app.html) aparte |
| Haptics | no | sí |
| Variantes | Single / Range | Single / Range |

---

## Motion Specification

Date Picker en Web combina motion de estado interno del field y motion del **popover integrado** con calendario embebido (mismo patrón que [Select](../../select/Web/select_motion-handoff_web.html) y [Search](../../search/Web/search_motion-handoff_web.html)).

Estados del field: `Enabled`, `Hovered`, `Focused`, `Filled`, `Error`, `Disabled`. Variantes `Type=Single` y `Type=Range`.

Al hacer **focus o click** en el field se despliega el popover (`opacity` + `translateY` ~6px). El ícono de calendario es **estático** — sin rotación tipo chevron.

**Single:** al elegir un día el valor persiste en el field y el popover cierra con `motion-exit`.

**Range:** primer tap fija inicio; segundo tap fija fin y actualiza relleno (instantáneo). El popover **permanece abierto** hasta **Aceptar**. **Cancelar** limpia el borrador en el calendario y restaura el último rango confirmado (o vacío) — **no modifica el input**. **Aceptar** consolida el rango y cierra con `motion-exit`. Dismiss (Escape, click fuera) descarta borrador sin cambiar el input.

El popover incluye header (mes/año + navegación), vista **Month** y vista **Year**, y grid de **Calendar Day Items** — átomo `_Datepicker/Day Item` documentado en este handoff (como Search Result Item en Search Web).

Cambio de mes: slide horizontal + fade con `motion-curve-md`. Vista Month ↔ Year: crossfade con `motion-curve-md`. Estados de celda (hover, pressed, active): `motion-curve-sm`. Relleno de rango entre celdas: instantáneo.

Sin `Skeleton`, `Loading` ni `Readonly`.

En error: stroke rojo siempre; ring rojo solo en focus.

---

## Calendar Day Item (átomo)

Átomo `_Datepicker/Day Item`. **No** es un componente DS independiente ni handoff aparte en Web.

| Variante | Uso |
|---|---|
| **Present** | Día actual — punto debajo del número |
| **Range: Start / Middle / End** | Selección de rango |
| **Active** | Día seleccionado (Single o extremos de Range) |

Estados: `Enabled`, `Hovered`, `Pressed`, `Focused`, `Active`, `Disabled`. Esquinas de selección: **4px**. Motion de celda: `background-color` con `motion-curve-sm`. Relleno de rango: sin motion.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.dp` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.dp__wrapper` | `border-color` | default | hover | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Trigger | Focus visible | field trigger | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focus ring in | `.dp__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 300 |
| 5 | Trigger | Field trigger press | `.dp__trigger` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Trigger feedback | trigger | `opacity` | default | pressed | `motion-curve-md` | 300ms | 0 | 300 |
| 7 | Trigger | Panel open | focus o click en field | — | — | — | — | — | 0 | 0 |
| 8 | Response | Panel enter | `.dp__panel` | `opacity`, `translateY` | `0`, `-6px` | `1`, `0` | `motion-curve-md` | 300ms | 0 | 300 |
| 9 | Trigger | Mes anterior / siguiente | Icon Button header | — | — | — | — | — | 0 | 0 |
| 10 | Response | Month slide | `.cal__grid-wrap` | `translateX`, `opacity` | offset | `0`, `1` | `motion-curve-md` | 300ms | 0 | 300 |
| 11 | Trigger | Cambio Month ↔ Year | header control | — | — | — | — | — | 0 | 0 |
| 12 | Response | Vista crossfade | Month / Year panel | `opacity` | visible | swap | `motion-curve-md` | 300ms | 0 | 300 |
| 13 | Trigger | Pointer enter | `.cal__day` | — | — | — | — | — | 0 | 0 |
| 14 | Response | Day hovered | `.cal__day` | `background-color` | default | hover | `motion-curve-sm` | 150ms | 0 | 150 |
| 15 | Trigger | Pointer down | `.cal__day` | — | — | — | — | — | 0 | 0 |
| 16 | Response | Day pressed | `.cal__day` | `background-color` | hover/default | pressed | `motion-curve-sm` | 150ms | 0 | 150 |
| 17 | Trigger | Day selected (Single) | Day Item | — | — | — | — | — | 0 | 0 |
| 18 | Response | Active + field filled | Day Item, field value | contenido | previo | fecha | none | instantáneo | 0 | 0 |
| 19 | Trigger | Range start / end | Day Item | — | — | — | — | — | 0 | 0 |
| 20 | Response | Range fill | celdas intermedias | `background-color` | none | middle | none | instantáneo | 0 | 0 |
| 21 | Trigger | Range complete (borrador) | Day Item (fin) | — | — | — | — | — | 0 | 0 |
| 22 | Response | Aceptar enabled | footer primario | `opacity` / estado | disabled | enabled | none | instantáneo | 0 | 0 |
| 23 | Trigger | Cancelar pressed | footer secundario | — | — | — | — | — | 0 | 0 |
| 24 | Response | Borrador limpiado | grid | contenido | parcial | ultimo confirmado | none | instantáneo | 0 | 0 |
| 25 | Trigger | Aceptar pressed | footer primario | — | — | — | — | — | 0 | 0 |
| 26 | Response | Field filled + panel exit | field value, `.dp__panel` | contenido + opacity/translateY | borrador | rango confirmado | motion-exit | 200ms | 0 | 200 |
| 27 | Trigger | Panel close (Single / dismiss) | selección / Escape / click fuera | — | — | — | — | — | 0 | 0 |
| 28 | Response | Panel exit | `.dp__panel` | `opacity`, `translateY` | `1`, `0` | `0`, `-6px` | `motion-exit` | 200ms | 0 | 200 |
| 29 | Trigger | Dismiss sin aplicar (Range) | Escape / click fuera | — | — | — | — | — | 0 | 0 |
| 30 | Response | Descartar borrador | selección pendiente | contenido | parcial | descartado | none | instantáneo | 0 | 0 |
| 31 | Trigger | Validation error | `.dp` | — | — | — | — | — | 0 | 0 |
| 32 | Response | Error feedback | ring, helper | `border-color`, `color` | focus/default | error | `motion-curve-md` | 300ms | 0 | 300 |
| 33 | Trigger | Toggle disabled | `.dp` | — | — | — | — | — | 0 | 0 |
| 34 | Response | Disabled state | `.dp` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Uso | Token semántico | Curva primitiva | Valor CSS | Duración |
|---|---|---|---|---|
| Estados del field (hover, focus, error, pressed) | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Popover enter | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Popover exit | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | 200ms |
| Cambio de mes + vista Year | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Calendar Day Item (hover, pressed, active) | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | 150ms |
| Relleno de rango | none | — | — | instantáneo |

Enter del popover y estados del field comparten `motion-curve-md`. Solo la salida del popover usa `motion-exit`.

---

## Composición

| Pieza | Handoff |
|---|---|
| Field + popover integrado | este documento |
| Calendar Day Item (átomo) | este documento — sección anterior |
| Footer Cancelar / Aceptar (solo Range) | [Button](../../button/Web/button_motion-handoff_web.html) — motion de botones |
| Icon Buttons de navegación header | [Icon Button](../../icon-button/Web/icon-button_motion-handoff_web.html) |
| Consumidor App (referencia) | [Date Picker App](../App/date-picker_motion-handoff_app.html) + [Calendar App](../calendar/App/calendar_motion-handoff_app.html) |

El popover del field **no** usa un handoff Dropdown aparte — el calendario vive dentro del Date Picker Web (patrón Select/Search).

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet CSS de este documento.
- **Apertura:** focus o click abre el popover anclado al field.
- **Single:** selección de día cierra panel y persiste valor.
- **Range:** footer Cancelar/Aceptar; Aceptar requiere inicio + fin.
- **Cancelar:** limpia borrador en calendario; no toca el input.
- **Ícono calendario:** estático; sin animación de rotación.
- **Panel:** `opacity` + `translateY` — sin `scale`.
- **Átomo de celda:** Calendar Day Item en este handoff; no crear `calendar/Web/`.
- **Icon Buttons:** reutilizar motion de Icon Button Web en flechas del header.
- **Haptics:** no aplica en Web.
- **QA final:** validar en navegador con tokens semánticos del design system y `prefers-reduced-motion`.

---

## Implementación CSS

```css
.dp__wrapper,
.dp__ring,
.dp__helper {
  transition-property: border-color, opacity, color;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.dp__trigger {
  transition-property: opacity;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.dp__panel {
  transition-property: opacity, transform;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.dp.is-panel-closing .dp__panel {
  transition-duration: var(--motion-duration-from-motion-exit);
  transition-timing-function: var(--motion-exit);
}

.cal__grid-wrap,
.cal__view-panel {
  transition-property: opacity, transform;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.cal__day {
  border-radius: 4px;
  transition-property: background-color;
  transition-duration: var(--motion-duration-from-motion-curve-sm);
  transition-timing-function: var(--motion-curve-sm);
}

.dp.is-disabled .dp__wrapper {
  transition: none;
  opacity: 0.45;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | field + popover + calendario interno |
| vs App | Range con footer en popover; Single auto-close |
| Day Item | átomo interno — no handoff `calendar/Web/` |
| Panel enter/exit | paridad Select Web (`curve-md` / `motion-exit`) |
| Mes / Year | `motion-curve-md`; sin spring en Web |
| Rango | Aceptar requiere inicio + fin; Cancelar limpia borrador |
| Hover field | solo stroke del contenedor |
| Skeleton | fuera de scope motion |
| Disabled | sin transición; cierra popover |
