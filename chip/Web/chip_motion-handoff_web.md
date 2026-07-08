# Motion Handoff — Chip (Web)

| | |
|---|---|
| **Componente** | Chip |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-sm` / `motion-exit` |
| **Categoría** | Default |
| **Variantes** | Informativo, Removible, Disabled |
| **Fecha** | 2026-06-23 |

---

## Motion Specification

Chip en Web anima **enter**, **exit** y estados internos del pill compacto.

**Variantes:** informativo (solo label), removible (label + ×), disabled.

**Enter** (cuando el consumidor monta el chip — ej. Multiple Select): `opacity` 0→1 + `scale` 0.96→1 con `motion-curve-sm`.

**Exit** (dismiss en variante removible): `opacity` 1→0 + `scale` 1→0.96 con `motion-exit`.

**Estados internos:** hover en el pill, pressed en el botón dismiss, focus-visible en dismiss. Disabled sin transición.

Sin skeleton. El consumidor orquesta mount/unmount; el contrato de motion vive en este handoff.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Chip mounted | `.chip` | — | — | — | — | — |
| 2 | Response | Enter | `.chip` | `opacity`, `transform` | `0`, `scale(0.96)` | `1`, `scale(1)` | `motion-curve-sm` | 150ms |
| 3 | Trigger | Pointer enter | `.chip` | — | — | — | — | — |
| 4 | Response | Hover | `.chip` | `background-color` | default | hover | `motion-curve-sm` | 150ms |
| 5 | Trigger | Pointer down | `.chip__dismiss` | — | — | — | — | — |
| 6 | Response | Dismiss pressed | `.chip__dismiss` | `transform` | `scale(1)` | `scale(0.92)` | `motion-curve-sm` | 150ms |
| 7 | Trigger | Focus visible | `.chip__dismiss` | — | — | — | — | — |
| 8 | Response | Focus ring | `.chip__ring` | `opacity` | `0` | `1` | `motion-curve-sm` | 150ms |
| 9 | Trigger | Dismiss click | `.chip__dismiss` | — | — | — | — | — |
| 10 | Response | Exit | `.chip` | `opacity`, `transform` | `1`, `scale(1)` | `0`, `scale(0.96)` | `motion-exit` | 100ms |
| 11 | Trigger | Disabled on | `.chip` | — | — | — | — | — |
| 12 | Response | Disabled | `.chip` | `opacity` | `1` | `0.45` | none | 0ms |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|
| `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |
| `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | `duration-100` | 100ms |

---

## Composición / Consumidores

| Consumidor | Uso |
|---|---|
| [Multiple Select](../../multiple-select/Web/multiple-select_motion-handoff_web.html) | Persistencia de selecciones como chips en el field |

El padre monta/desmonta el chip; enter y exit se aplican al montar y al dismiss.

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet CSS de este documento.
- **Enter:** al insertar el chip en el DOM del consumidor.
- **Exit:** al confirmar dismiss; el padre desmonta tras completar la transición.
- **Variante informativa:** sin dismiss ni exit por ×.
- **QA final:** validar enter, exit y `prefers-reduced-motion`.

---

## Implementación CSS

```css
.chip {
  transition-property: opacity, transform, background-color;
  transition-duration: var(--motion-duration-from-motion-curve-sm);
  transition-timing-function: var(--motion-curve-sm);
}

.chip__dismiss {
  transition-property: transform, opacity;
  transition-duration: var(--motion-duration-from-motion-curve-sm);
  transition-timing-function: var(--motion-curve-sm);
}

.chip__ring {
  transition: opacity var(--motion-duration-from-motion-curve-sm) var(--motion-curve-sm);
}

.chip.is-exiting {
  transition-duration: var(--motion-duration-from-motion-exit);
  transition-timing-function: var(--motion-exit);
}

.chip.is-disabled,
.chip:disabled {
  transition: none;
  opacity: 0.45;
}

@media (prefers-reduced-motion: reduce) {
  .chip,
  .chip__dismiss,
  .chip__ring {
    transition: none;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token estados | `motion-curve-sm` |
| Token exit | `motion-exit` · 100ms |
| Enter | opacity + scale sutil |
| Dismiss | solo variante removible |
| Disabled | instantáneo |
| Stagger | no — el consumidor monta chips sin delay |
| A11y | focus en dismiss; `prefers-reduced-motion` |
