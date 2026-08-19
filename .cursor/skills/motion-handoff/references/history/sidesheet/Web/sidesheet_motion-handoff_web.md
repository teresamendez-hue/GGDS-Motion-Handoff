# Motion Handoff — Sidesheet (Web)

| | |
|---|---|
| **Componente** | Sidesheet |
| **Plataforma** | Web |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token entrada** | `motion-curve-lg` |
| **Token salida** | `motion-exit` |
| **Variantes** | Overlay (desktop), Full-screen (web mobile) |
| **Fecha** | 2026-06-05 |

---

## Motion Specification

Sidesheet es un panel lateral que entra desde el borde derecho. El motion combina desplazamiento horizontal del panel y fade de opacidad para mantener lectura espacial.

El scrim acompaña en paralelo con fade para transferir foco al panel. El trigger de cierre no forma parte de este documento; solo se especifica el comportamiento de motion.

Para desktop overlay y mobile full-screen se mantiene la misma intención: entrada más pausada y salida más rápida.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Apertura | `sidesheet` | — | — | — | — | — |
| 2 | Response | Entrada panel | `.sidesheet` | `transform` | `translateX(100%)` | `translateX(0)` | `motion-curve-lg` | 500ms |
| 3 | Response | Entrada panel | `.sidesheet` | `opacity` | 0 | 1 | `motion-curve-lg` | 500ms |
| 4 | Response | Entrada scrim | `.sidesheet-scrim` | `opacity` | 0 | 1 | `motion-curve-lg` | 500ms |
| 5 | Trigger | Cierre | `sidesheet` | — | — | — | — | — |
| 6 | Response | Salida panel | `.sidesheet` | `transform` | `translateX(0)` | `translateX(100%)` | `motion-exit` | 300ms |
| 7 | Response | Salida panel | `.sidesheet` | `opacity` | 1 | 0 | `motion-exit` | 300ms |
| 8 | Response | Salida scrim | `.sidesheet-scrim` | `opacity` | 1 | 0 | `motion-exit` | 300ms |

---

## Token Mapping

| Dirección | Token semántico | Curva primitiva | Valor CSS | Duración |
|---|---|---|---|---|
| Entrada | `motion-curve-lg` | `easing-emphasis` | `cubic-bezier(0.2, 0, 0, 1)` | `duration-500` (500ms) |
| Salida | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | `duration-300` (300ms) |

---

## Implementación CSS

```css
/* Consumir tokens semánticos como fuente de verdad */
.sidesheet {
  transform: translateX(100%);
  opacity: 0;
}

.sidesheet.is-entering {
  transform: translateX(0);
  opacity: 1;
  transition:
    transform var(--motion-duration-from-motion-curve-lg) var(--motion-curve-lg),
    opacity var(--motion-duration-from-motion-curve-lg) var(--motion-curve-lg);
}

.sidesheet.is-exiting {
  transform: translateX(100%);
  opacity: 0;
  transition:
    transform var(--motion-duration-from-motion-exit) var(--motion-exit),
    opacity var(--motion-duration-from-motion-exit) var(--motion-exit);
}

.sidesheet-scrim {
  opacity: 0;
}

.sidesheet-scrim.is-entering {
  opacity: 1;
  transition: opacity var(--motion-duration-from-motion-curve-lg) var(--motion-curve-lg);
}

.sidesheet-scrim.is-exiting {
  opacity: 0;
  transition: opacity var(--motion-duration-from-motion-exit) var(--motion-exit);
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Dirección | Siempre desde el borde derecho para preservar jerarquía espacial |
| Scrim | Animar en paralelo con panel |
| Salida | Más corta que entrada (`motion-exit`) |
| Evitar | `scale` y `easing-back` en sidesheet |
| Accesibilidad | En reduced motion: ir directo al estado final |
