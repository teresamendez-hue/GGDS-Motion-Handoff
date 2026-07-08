# Motion Handoff — Autocomplete (Web)

| | |
|---|---|
| **Componente** | Autocomplete |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-md` / `motion-exit` |
| **Categoría** | Default |
| **Fecha** | 2026-06-23 |

---

## Motion Specification

Autocomplete en Web combina motion de estado interno del field (input editable) y motion del dropdown de sugerencias integrado.

Estados del field: `Enabled`, `Hovered`, `Focused`, `Typing`, `Filled`, `Error`, `Disabled`.

El panel de sugerencias **no** abre en focus vacío. Aparece al escribir el **primer carácter** (`opacity` + `translateY`) y permanece abierto mientras haya texto en el input. Cierra al seleccionar una opción, Escape, click fuera, blur o al borrar todo el contenido.

El contenido del panel son **Option Items** — motion documentado en [Option Item](../../option-item/Web/option-item_motion-handoff_web.html).

El filtrado de resultados es actualización funcional del contenido; **sin** re-animar el panel ni stagger en filas.

Si el componente permite entrada libre, el panel cierra con el mismo `motion-exit` al hacer blur — sin motion adicional.

Sin chevron, `Skeleton`, `Loading` ni `Readonly`.

En error: stroke rojo siempre; ring rojo solo en focus.

> En App el equivalente es **Combobox** — handoff aparte.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.autocomplete` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.autocomplete__wrapper` | `border-color` | default | hover | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Trigger | Focus visible | `.autocomplete__input` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focus ring in | `.autocomplete__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 300 |
| 5 | Trigger | Input typing | `.autocomplete__input` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Typing settle | `.autocomplete__wrapper` | `border-color` | focus | active | `motion-curve-md` | 300ms | 0 | 300 |
| 7 | Trigger | Primer carácter ingresado | `.autocomplete__input` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Panel enter | `.autocomplete__panel` | `opacity`, `translateY` | `0`, `-6px` | `1`, `0` | `motion-curve-md` | 300ms | 0 | 300 |
| 9 | Trigger | Panel close | selección / Escape / click fuera / blur / input vacío | — | — | — | — | — | 0 | 0 |
| 10 | Response | Panel exit | `.autocomplete__panel` | `opacity`, `translateY` | `1`, `0` | `0`, `-6px` | `motion-exit` | 200ms | 0 | 200 |
| 11 | Trigger | Validation error | `.autocomplete` | — | — | — | — | — | 0 | 0 |
| 12 | Response | Error feedback | ring, helper | `border-color`, `color` | focus/default | error | `motion-curve-md` | 300ms | 0 | 300 |
| 13 | Trigger | Toggle disabled | `.autocomplete` | — | — | — | — | — | 0 | 0 |
| 14 | Response | Disabled state | `.autocomplete` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Uso | Token semántico | Curva primitiva | Valor CSS | Duración |
|---|---|---|---|---|
| Estados del field (hover, focus, error, typing) | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Panel enter | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Panel exit | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | 200ms |

Enter y estados del field comparten `motion-curve-md`. Solo la salida del panel usa `motion-exit`.

---

## Composición

| Pieza | Handoff |
|---|---|
| Dropdown de sugerencias (panel vacío en preview) | este documento |
| Filas de resultados | [Option Item](../../option-item/Web/option-item_motion-handoff_web.html) |

El dropdown del field consume [Option Items](../../option-item/Web/option-item_motion-handoff_web.html) con motion propio en su handoff.

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet CSS de este documento.
- **Apertura del panel:** solo después del primer carácter; no en focus vacío.
- **Composición:** contenido del panel → [Option Items](../../option-item/Web/option-item_motion-handoff_web.html).
- **Filtrado:** sin motion adicional al actualizar la lista.
- **Entrada libre:** mismo `motion-exit` al cerrar; persistencia es lógica de negocio.
- **Scroll:** nativo en la lista; sin token de motion.
- **Haptics:** no aplica en Web.
- **QA final:** validar en navegador con tokens semánticos del design system.

---

## Implementación CSS

```css
.autocomplete__wrapper,
.autocomplete__ring,
.autocomplete__helper {
  transition-property: border-color, opacity, color;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.autocomplete__panel {
  transition-property: opacity, transform;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.autocomplete.is-panel-closing .autocomplete__panel {
  transition-duration: var(--motion-duration-from-motion-exit);
  transition-timing-function: var(--motion-exit);
}

.autocomplete.is-disabled .autocomplete__wrapper {
  transition: none;
  opacity: 0.45;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope field | estados del contenedor + apertura/cierre del panel |
| Contenido del panel | Option Items — handoff aparte |
| Apertura | primer carácter; no focus vacío |
| Chevron | no aplica |
| Filtrado | sin re-animar panel ni filas |
| Entrada libre | mismo exit que selección de lista |
| Focus ring | anillo externo sobre el field completo |
| Error | stroke rojo siempre; ring rojo solo en focus |
| Hover | solo stroke del contenedor |
| Disabled | sin transición; cierra panel |
| Scroll | nativo, sin token |
