# Motion Handoff — Tab (Web)

| | |
|---|---|
| **Componente** | Tab |
| **Plataforma** | Web |
| **Owner** | Ezequiel Arguello |
| **Design system** | GGDS |
| **Variantes indicador** | `line` · `segmented` |
| **Token semántico (indicador + panel)** | `motion-curve-md` |
| **Token semántico (tab item)** | `motion-curve-sm` |
| **Categoría** | Guía (indicador/panel) · Default (item) |
| **Fecha** | 2026-07-08 |

---

## Motion Specification

Tab en Web cambia la selección con dos variantes de indicador: **Line** y **Segmented**. El indicador se desplaza con `motion-curve-md` al activar otro tab. El **panel de contenido** hace slide horizontal (`translateX`) con el mismo token y dirección coherente con el cambio de índice.

Los **tab items** animan estados interactivos (hover, focus, pressed) con `motion-curve-sm`. **Selected** es estado visual estático en el label; el movimiento principal lo hace el indicador. **Disabled** es instantáneo.

La lista de tabs admite **scroll horizontal** cuando hay overflow. El usuario puede desplazar la lista con gesto nativo (sin token de motion). Al **seleccionar un tab fuera del viewport**, la lista hace scroll programático para revelarlo (`scrollTo` con `behavior: smooth`); en `prefers-reduced-motion` el scroll es instantáneo (`behavior: auto`). El indicador recalcula su posición en cada evento `scroll`.

Sin enter/exit de viewport del componente. En `prefers-reduced-motion`, indicador, panel e items saltan al estado final sin transición.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Tab selected | Tab bar | — | — | — | — | — |
| 2 | Response | Indicador | `.tabs__indicator` | `transform`, `width` | tab N | tab N±1 | `motion-curve-md` | 300ms |
| 3 | Response | Panel content | `.tabs__panel-track` | `transform` | panel N | panel N±1 | `motion-curve-md` | 300ms |
| 4 | Trigger | Scroll horizontal (usuario) | `.tabs__scroll` | — | — | — | — | — |
| 5 | Response | Indicador sync | `.tabs__indicator` | `transform` | — | recalculado | — | 0ms |
| 6 | Trigger | Tab selected off-screen | `.tabs__scroll` | — | — | — | — | — |
| 7 | Response | Reveal tab activo | `.tabs__scroll` | `scrollLeft` | actual | tab visible | — | nativo / instantáneo |
| 8 | Trigger | Pointer enter | `.tabs__tab` | — | — | — | — | — |
| 9 | Response | Hovered | `.tabs__tab` | `color` | default | hover | `motion-curve-sm` | 150ms |
| 10 | Trigger | Pointer down | `.tabs__tab` | — | — | — | — | — |
| 11 | Response | Pressed | `.tabs__tab` | `opacity` | 1 | 0.85 | `motion-curve-sm` | 150ms |
| 12 | Trigger | Focus visible | `.tabs__tab` | — | — | — | — | — |
| 13 | Response | Focus ring | `.tabs__tab__ring` | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms |
| 14 | Trigger | Tab disabled | `.tabs__tab` | — | — | — | — | — |
| 15 | Response | Disabled | `.tabs__tab` | todas | — | final | — | 0ms |
| 16 | Trigger | `prefers-reduced-motion` | indicador, panel, items | — | — | — | — | — |
| 17 | Response | Reduced motion | todas | — | — | final | — | 0ms |

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor | Uso |
|---|---|---|---|---|---|
| `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | `duration-300` | 300ms | Indicador + slide de panel |
| `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms | Estados del tab item |

---

## Implementación CSS

```css
/* Indicador — Line y Segmented */
.tabs__indicator {
  transition-property: transform, width;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

/* Panel — slide horizontal */
.tabs__panel-track {
  transition-property: transform;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

/* Lista — scroll horizontal nativo */
.tabs__scroll {
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}

/* Tab item — estados interactivos */
.tabs__tab {
  transition-property: color, opacity;
  transition-duration: var(--motion-duration-from-motion-curve-sm);
  transition-timing-function: var(--motion-curve-sm);
}

.tabs__tab__ring {
  transition: opacity var(--motion-duration-from-motion-curve-sm) var(--motion-curve-sm);
}

.tabs__tab:disabled {
  transition: none;
}

@media (prefers-reduced-motion: reduce) {
  .tabs__indicator,
  .tabs__panel-track,
  .tabs__tab,
  .tabs__tab__ring {
    transition: none;
  }
}
```

```javascript
// Reveal tab activo — sin token de motion en el scroll
function revealActiveTab(scrollEl, tabEl, reducedMotion) {
  scrollEl.scrollTo({
    left: tabEl.offsetLeft - 40,
    behavior: reducedMotion ? 'auto' : 'smooth',
  });
}

// Sync indicador al scroll manual
scrollEl.addEventListener('scroll', updateIndicatorPosition);
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Indicador + panel | `motion-curve-md` — misma duración y curva para no desincronizar |
| Tab item | `motion-curve-sm` — microinteracción separada |
| Variantes | **Line** y **Segmented** comparten token de indicador |
| Scroll manual | nativo; indicador sincronizado en `scroll` |
| Reveal tab activo | `scrollTo` smooth; instantáneo en `prefers-reduced-motion` |
| Disabled | sin transición |
| Haptics | no aplica en Web |
| A11y | `prefers-reduced-motion` → salto instantáneo |
