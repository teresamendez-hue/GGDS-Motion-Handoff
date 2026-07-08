# Motion Handoff — Smart Select (Web)

| | |
|---|---|
| **Componente** | Smart Select |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-md` / `motion-exit` |
| **Categoría** | Default |
| **Fecha** | 2026-06-23 |

---

## Motion Specification

Smart Select en Web combina motion de estado interno del field y motion del dropdown integrado con búsqueda interna.

Estados del field: `Enabled`, `Hovered`, `Focused`, `Filled`, `Error`, `Disabled`.

Al hacer **click** en el field, el componente transiciona a Focused: el dropdown se renderiza debajo del input (`opacity` + `translateY`) y el chevron rota 180°. El panel incluye un componente **Search** en la parte superior y la lista de resultados debajo.

Al hacer click en el Search del panel, el foco pasa al search. Al tipear, la lista se filtra de forma funcional; si no hay coincidencias, se muestra un mensaje en el panel — sin motion adicional.

Al seleccionar una opción, el valor se transfiere al input, el chevron vuelve a su posición original y el panel cierra con `motion-exit`.

El panel cierra al: (1) seleccionar una opción, (2) click fuera del componente, (3) Tab al siguiente campo.

Sin `Skeleton`, `Loading` ni `Readonly`. Sin entrada libre — solo opciones de la lista.

En error: stroke rojo siempre; ring rojo solo en focus.

> Componente exclusivo Web. En App, listas largas con búsqueda usan **Combobox** — handoff aparte.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.smart-select` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.smart-select__wrapper` | `border-color` | default | hover | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Trigger | Click en field | trigger del field | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focus ring in | `.smart-select__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 300 |
| 5 | Trigger | Field trigger press | `.smart-select__trigger` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Trigger feedback | chevron | `opacity` | default | pressed | `motion-curve-md` | 300ms | 0 | 300 |
| 7 | Trigger | Panel open | click en field | — | — | — | — | — | 0 | 0 |
| 8 | Response | Panel enter | `.smart-select__panel` | `opacity`, `translateY` | `0`, `-6px` | `1`, `0` | `motion-curve-md` | 300ms | 0 | 300 |
| 9 | Response | Chevron rotate | `.smart-select__icon` | `transform` | `0deg` | `180deg` | `motion-curve-md` | 300ms | 0 | 300 |
| 10 | Trigger | Focus en Search del panel | search interno | — | — | — | — | — | 0 | 0 |
| 11 | Trigger | Input en Search | search interno | — | — | — | — | — | 0 | 0 |
| 12 | Response | Lista filtrada | Option Items | contenido | lista completa | coincidencias | none | instantáneo | 0 | 0 |
| 13 | Trigger | Option selected | Option Item | — | — | — | — | — | 0 | 0 |
| 14 | Trigger | Panel close | selección / click fuera / Tab | — | — | — | — | — | 0 | 0 |
| 15 | Response | Panel exit | `.smart-select__panel` | `opacity`, `translateY` | `1`, `0` | `0`, `-6px` | `motion-exit` | 200ms | 0 | 200 |
| 16 | Response | Chevron reset | `.smart-select__icon` | `transform` | `180deg` | `0deg` | `motion-exit` | 200ms | 0 | 200 |
| 17 | Trigger | Validation error | `.smart-select` | — | — | — | — | — | 0 | 0 |
| 18 | Response | Error feedback | ring, helper | `border-color`, `color` | focus/default | error | `motion-curve-md` | 300ms | 0 | 300 |
| 19 | Trigger | Toggle disabled | `.smart-select` | — | — | — | — | — | 0 | 0 |
| 20 | Response | Disabled state | `.smart-select` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

Filas 10–12: motion del Search interno en [Search](../../search/Web/search_motion-handoff_web.html) (field + clear; sin overlay propio). Motion de filas en [Option Item](../../option-item/Web/option-item_motion-handoff_web.html).

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
| Search en el header del panel | [Search](../../search/Web/search_motion-handoff_web.html) — field + clear; sin overlay propio |
| Filas de resultados | [Option Item](../../option-item/Web/option-item_motion-handoff_web.html) |

El dropdown integra [Search](../../search/Web/search_motion-handoff_web.html) en la parte superior (estados del input y clear; **no** abre overlay propio — el panel es el de Smart Select) y [Option Items](../../option-item/Web/option-item_motion-handoff_web.html) en la lista. El filtrado y el estado “0 resultados” son lógica de contenido, no motion del panel.

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet CSS de este documento.
- **Apertura:** click en el field abre el panel y rota el chevron.
- **Search:** motion del field interno en handoff [Search](../../search/Web/search_motion-handoff_web.html); el foco al search no re-anima el panel de Smart Select.
- **Filtrado:** instantáneo en la lista; sin stagger ni re-enter del panel.
- **Cierre:** selección, click fuera o Tab — todos con `motion-exit` en el panel.
- **Relacionados:** [Select](../../select/Web/select_motion-handoff_web.html) (lista cerrada), [Autocomplete](../../autocomplete/Web/autocomplete_motion-handoff_web.html) (búsqueda en el field de página).
- **Haptics:** no aplica en Web.
- **QA final:** validar en navegador con tokens semánticos del design system.

---

## Implementación CSS

```css
.smart-select__wrapper,
.smart-select__ring {
  transition-property: border-color, opacity, color;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.smart-select__icon {
  transition-property: transform, opacity;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.smart-select__panel {
  transition-property: opacity, transform;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.smart-select.is-panel-closing .smart-select__panel,
.smart-select.is-panel-closing .smart-select__icon {
  transition-duration: var(--motion-duration-from-motion-exit);
  transition-timing-function: var(--motion-exit);
}

.smart-select.is-disabled .smart-select__wrapper {
  transition: none;
  opacity: 0.45;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope field | estados del contenedor + apertura/cierre del dropdown |
| Search en panel | [Search](../../search/Web/search_motion-handoff_web.html) — field embebido, sin overlay |
| Contenido del panel | Option Items — handoff aparte |
| Filtrado / 0 resultados | sin motion adicional |
| Selección | única; cierra panel al elegir |
| Cierre | selección · click fuera · Tab |
| vs Select | incluye Search interno para listas largas |
| vs Autocomplete | búsqueda en panel, no en field de página |
| Focus ring | anillo externo sobre el field completo |
| Error | stroke rojo siempre; ring rojo solo en focus |
| Disabled | sin transición; cierra panel |
