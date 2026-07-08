# Motion Handoff — Search (Web)

| | |
|---|---|
| **Componente** | Search |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-md` / `motion-curve-sm` / `motion-exit` |
| **Categoría** | Default |
| **Fecha** | 2026-06-23 |

---

## Motion Specification

Search en Web combina motion de estado interno del field, micro-motion del botón clear y motion del panel overlay de resultados.

Estados del field: `Enabled`, `Hovered`, `Focused`, `Typing`, `Filled`, `Error`, `Disabled`.

El ícono leading (lupa) es fijo — sin motion. El botón clear (X) aparece solo cuando el input tiene contenido (`Filled`); al presionarlo limpia el texto y restaura el estado inicial de la lista.

El panel overlay **no** abre en la primera búsqueda sin historial. Se despliega cuando la persona ingresa **≥ 3 caracteres** o cuando la prop `showMenuOnFocus` expone historial/sugerencias en focus vacío. Al primer carácter con menú abierto, el contenido transiciona de historial a resultados dinámicos **sin re-animar** el panel.

Al seleccionar un **Search Result Item** (átomo interno), el valor pasa al input y el panel cierra con `motion-exit`. Cierre también por Escape, click fuera, blur o al vaciar el input con clear.

Las filas del panel son un **átomo propio de Search** (`_Search/Results Items` en Figma) — no consumen [Option Item](../../option-item/Web/option-item_motion-handoff_web.html). Cada fila incluye ícono leading fijo por tipo: **clock** (historial) o **lupa** (sugerencias / resultados nuevos). Motion de fila documentado en este handoff con `motion-curve-sm`.

Sin `Skeleton`, `Loading` ni `Readonly`. Filtrado y scroll de lista: sin motion adicional.

En error: stroke rojo siempre; ring rojo solo en focus.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.search` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.search__wrapper` | `border-color` | default | hover | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Trigger | Focus visible | `.search__input` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focus ring in | `.search__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 300 |
| 5 | Trigger | Input typing | `.search__input` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Typing settle | `.search__wrapper` | `border-color` | focus | active | `motion-curve-md` | 300ms | 0 | 300 |
| 7 | Trigger | Input filled | `.search__input` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Clear button in | `.search__clear` | `opacity` | `0` | `1` | `motion-curve-sm` | 150ms | 0 | 150 |
| 9 | Trigger | Panel open | ≥3 chars o `showMenuOnFocus` en focus | — | — | — | — | — | 0 | 0 |
| 10 | Response | Panel enter | `.search__panel` | `opacity`, `translateY` | `0`, `-6px` | `1`, `0` | `motion-curve-md` | 300ms | 0 | 300 |
| 11 | Trigger | Primer carácter con menú abierto | contenido del panel | — | — | — | — | — | 0 | 0 |
| 12 | Response | Swap historial → dinámico | Search Result Items | contenido | historial | resultados | none | instantáneo | 0 | 0 |
| 13 | Trigger | Pointer enter | `.search__result-item` | — | — | — | — | — | 0 | 0 |
| 14 | Response | Row hovered | `.search__result-item` | `background-color` | default | hover | `motion-curve-sm` | 150ms | 0 | 150 |
| 15 | Trigger | Pointer down | `.search__result-item` | — | — | — | — | — | 0 | 0 |
| 16 | Response | Row pressed | `.search__result-item` | `background-color` | hover/default | pressed | `motion-curve-sm` | 150ms | 0 | 150 |
| 17 | Trigger | Focus visible | `.search__result-item` | — | — | — | — | — | 0 | 0 |
| 18 | Response | Row focus ring | `.search__result-item` | `outline` / ring | none | focus | `motion-curve-sm` | 150ms | 0 | 150 |
| 19 | Trigger | Result selected | Search Result Item | — | — | — | — | — | 0 | 0 |
| 20 | Trigger | Panel close | selección / Escape / click fuera / blur / clear | — | — | — | — | — | 0 | 0 |
| 21 | Response | Panel exit | `.search__panel` | `opacity`, `translateY` | `1`, `0` | `0`, `-6px` | `motion-exit` | 200ms | 0 | 200 |
| 22 | Trigger | Clear pressed | `.search__clear` | — | — | — | — | — | 0 | 0 |
| 23 | Response | Clear button out | `.search__clear` | `opacity` | `1` | `0` | `motion-curve-sm` | 150ms | 0 | 150 |
| 24 | Trigger | Validation error | `.search` | — | — | — | — | — | 0 | 0 |
| 25 | Response | Error feedback | ring, helper | `border-color`, `color` | focus/default | error | `motion-curve-md` | 300ms | 0 | 300 |
| 26 | Trigger | Toggle disabled | `.search` | — | — | — | — | — | 0 | 0 |
| 27 | Response | Disabled state | `.search` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Uso | Token semántico | Curva primitiva | Valor CSS | Duración |
|---|---|---|---|---|
| Estados del field (hover, focus, error, typing) | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Clear button enter/exit | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | 150ms |
| Panel overlay enter | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Panel overlay exit | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | 200ms |
| Search Result Item (hover, pressed, focus) | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | 150ms |

Enter del panel y estados del field comparten `motion-curve-md`. Solo la salida del panel usa `motion-exit`. Las filas del átomo usan `motion-curve-sm` — misma familia que Option Item, scope distinto.

---

## Search Result Item (átomo)

Átomo interno `_Search/Results Items`. **No** es un componente DS independiente ni reutiliza Option Item.

| Variante ícono | Uso |
|---|---|
| **clock** | Búsquedas anteriores / historial |
| **lupa** | Sugerencias destacadas o resultados dinámicos |

Estados de fila: `Enabled`, `Hovered`, `Pressed`, `Focused`. Sin enter/exit de viewport. Sin stagger entre filas. El cambio de ícono clock → lupa al tipear es swap de contenido, sin motion.

---

## Composición

| Pieza | Handoff |
|---|---|
| Field + clear + panel overlay | este documento |
| Search Result Item (átomo) | este documento — sección anterior |
| Consumidor | [Smart Select](../../smart-select/Web/smart-select_motion-handoff_web.html) integra Search completo |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet CSS de este documento.
- **Umbral:** panel abre con ≥ 3 caracteres o con `showMenuOnFocus` en focus vacío.
- **Swap de contenido:** al primer carácter no re-animar el panel.
- **Átomo de fila:** Search Result Item documentado en este handoff; no usar Option Item.
- **Clear:** limpia input y restaura lista inicial; oculta botón con `motion-curve-sm`.
- **Relacionados:** [Autocomplete](../../autocomplete/Web/autocomplete_motion-handoff_web.html) (panel al 1.er char) · [Smart Select](../../smart-select/Web/smart-select_motion-handoff_web.html) (Search como slot interno).
- **Haptics:** no aplica en Web.
- **QA final:** validar en navegador con tokens semánticos del design system.

---

## Implementación CSS

```css
.search__wrapper,
.search__ring,
.search__helper {
  transition-property: border-color, opacity, color;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.search__clear {
  transition-property: opacity;
  transition-duration: var(--motion-duration-from-motion-curve-sm);
  transition-timing-function: var(--motion-curve-sm);
}

.search__panel {
  transition-property: opacity, transform;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.search.is-panel-closing .search__panel {
  transition-duration: var(--motion-duration-from-motion-exit);
  transition-timing-function: var(--motion-exit);
}

.search__result-item {
  transition-property: background-color;
  transition-duration: var(--motion-duration-from-motion-curve-sm);
  transition-timing-function: var(--motion-curve-sm);
}

.search.is-disabled .search__wrapper {
  transition: none;
  opacity: 0.45;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope field | estados del contenedor + clear + overlay |
| Leading icon | sin motion |
| Clear | solo visible en Filled |
| Panel | overlay; no desplaza layout |
| Umbral 3 chars | sin parpadeo si ya abierto por historial |
| vs Autocomplete | Search exige ≥3 chars o historial |
| Search Result Item | átomo interno; clock / lupa; no Option Item |
| vs Option Item | Option Item es genérico y componible; Search Result Item es exclusivo del panel de búsqueda |
| Disabled | sin transición; cierra panel |
