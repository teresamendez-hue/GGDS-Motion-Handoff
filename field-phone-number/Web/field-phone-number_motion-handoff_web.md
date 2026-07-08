# Motion Handoff — Field Phone Number (Web)

| | |
|---|---|
| **Componente** | Field Phone Number |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-md` / `motion-exit` |
| **Categoría** | Default |
| **Fecha** | 2026-06-23 |

---

## Motion Specification

Field Phone Number en Web usa motion de estado interno y motion del dropdown integrado del prefix.

Estados del field: `Enabled`, `Hovered`, `Focused`, `Typing`, `Filled`, `Error`, `Disabled`.

El campo es compuesto: selector de país (`+54` + chevron) + input numérico. El anillo de foco envuelve el contenedor completo.

Al tocar el prefix se despliega el dropdown del field (opacity + `translateY`). El contenido del panel son **Option Items** — motion documentado en [Option Item](../../option-item/Web/option-item_motion-handoff_web.html).

Sin `Skeleton`, `Loading` ni `Readonly`.

En error: stroke rojo siempre; ring rojo solo en focus.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter | `.field-phone-number` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Hovered | `.field-phone-number__wrapper` | `border-color` | default | hover | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Trigger | Focus visible | input o country trigger | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focus ring in | `.field-phone-number__ring` | `opacity`, `border-color` | `0`, neutral | `1`, focus | `motion-curve-md` | 300ms | 0 | 300 |
| 5 | Trigger | Country trigger press | `.field-phone-number__country` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Trigger feedback | `.field-phone-number__country` | `opacity` | default | pressed | `motion-curve-md` | 300ms | 0 | 300 |
| 7 | Trigger | Prefix panel open | `.field-phone-number__country` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Panel enter | `.field-phone-number__panel` | `opacity`, `translateY` | `0`, `-6px` | `1`, `0` | `motion-curve-md` | 300ms | 0 | 300 |
| 9 | Trigger | Prefix panel close | `.field-phone-number__country` / Escape / focus número | — | — | — | — | — | 0 | 0 |
| 10 | Response | Panel exit | `.field-phone-number__panel` | `opacity`, `translateY` | `1`, `0` | `0`, `-6px` | `motion-exit` | 200ms | 0 | 200 |
| 11 | Trigger | Input typing | `.field-phone-number__input` | — | — | — | — | — | 0 | 0 |
| 12 | Response | Typing settle | `.field-phone-number__wrapper` | `border-color` | focus | active | `motion-curve-md` | 300ms | 0 | 300 |
| 13 | Trigger | Validation error | `.field-phone-number` | — | — | — | — | — | 0 | 0 |
| 14 | Response | Error feedback | ring, helper | `border-color`, `color` | focus/default | error | `motion-curve-md` | 300ms | 0 | 300 |
| 15 | Trigger | Toggle disabled | `.field-phone-number` | — | — | — | — | — | 0 | 0 |
| 16 | Response | Disabled state | `.field-phone-number` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Uso | Token semántico | Curva primitiva | Valor CSS | Duración |
|---|---|---|---|---|
| Estados del field (hover, focus, error, prefix pressed) | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Panel enter + chevron open | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms |
| Panel exit + chevron close | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | 200ms |

Enter y estados del field comparten `motion-curve-md`. Solo la salida del panel usa `motion-exit`.

---

## Composición

| Pieza | Handoff |
|---|---|
| Dropdown del prefix (panel vacío en preview) | este documento |
| Filas de países / opciones | [Option Item](../../option-item/Web/option-item_motion-handoff_web.html) |

---

## Implementación CSS

```css
.field-phone-number__wrapper,
.field-phone-number__ring,
.field-phone-number__country {
  transition-property: border-color, opacity, color;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.field-phone-number__panel {
  transition-property: opacity, transform;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.field-phone-number.is-panel-closing .field-phone-number__panel {
  transition-duration: var(--motion-duration-from-motion-exit);
  transition-timing-function: var(--motion-exit);
}

.field-phone-number.is-disabled .field-phone-number__wrapper {
  transition: none;
  opacity: 0.45;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope field | estados del contenedor + trigger prefix |
| Scope panel | apertura/cierre del dropdown del prefix |
| Contenido del panel | Option Items — handoff aparte |
| Focus ring | anillo externo único sobre país + número |
| Prefix activo | solo cuando el focus está en el trigger o el panel está abierto |
| Error | stroke rojo siempre; ring rojo solo en focus |
| Hover | solo stroke del contenedor, sin fill |
| Disabled | sin transición; cierra panel |
| Haptics | no aplica en Web |
| Coherencia | mismo patrón que `Field Prefix` |
