# Motion Handoff — Tooltip (Web)

| | |
|---|---|
| **Componente** | Tooltip |
| **Plataforma** | Web |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token entrada** | `motion-curve-sm` |
| **Token salida** | `motion-exit` |
| **Categoría** | Default |
| **Fecha** | 2026-06-25 |

---

## Motion Specification

El Tooltip provee información contextual breve al hacer hover o focus sobre un elemento disparador. No es interactivo: `pointer-events: none` en el tooltip; el hit area vive solo en el trigger (evita hover tunneling).

Variantes de posicionamiento — **Vertical:** Top, Bottom · **Alignment:** Left, Center, Right — afectan layout y flecha indicadora, **no** el motion. Todas comparten la misma transición.

Se anima únicamente `opacity`. Entrada con `motion-curve-sm` (150ms). Salida suave (`mouseleave`, `blur` sin cambio de foco forzado) con `motion-exit` (100ms). Cierre forzado — `Escape`, scroll significativo, click en trigger, Tab al siguiente elemento — es **instantáneo** (`transition: none`).

El delay funcional de apertura post-hover (si lo define producto) queda fuera del contrato de motion; la transición documentada arranca cuando el tooltip pasa a visible.

> **Haptics:** no aplica en Web.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | `mouseenter` o `focus-visible` en trigger | — | — | — | — | — | — | 0 | — |
| 2 | Response | Tooltip aparece | `.tooltip` | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms | 0 | 150 |
| 3 | Trigger | `mouseleave` o `blur` (salida suave) | — | — | — | — | — | — | * | — |
| 4 | Response | Tooltip se oculta (fade) | `.tooltip` | `opacity` | 1 | 0 | `motion-exit` | 100ms | * | *+100 |
| 5 | Trigger | `Escape` | — | — | — | — | — | — | * | — |
| 6 | Response | Cierre forzado | `.tooltip` | `opacity` | 1 | 0 | none | 0ms | * | * |
| 7 | Trigger | Scroll significativo en viewport | — | — | — | — | — | — | * | — |
| 8 | Response | Cierre forzado | `.tooltip` | `opacity` | 1 | 0 | none | 0ms | * | * |
| 9 | Trigger | Click en trigger | — | — | — | — | — | — | * | — |
| 10 | Response | Cierre forzado | `.tooltip` | `opacity` | 1 | 0 | none | 0ms | * | * |
| 11 | Trigger | Tab → foco en otro elemento | — | — | — | — | — | — | * | — |
| 12 | Response | Cierre forzado | `.tooltip` | `opacity` | 1 | 0 | none | 0ms | * | * |

> `*` = momento variable según interacción.

---

## Token Mapping

| Dirección | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor ms |
|---|---|---|---|---|---|
| Entrada | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |
| Salida suave | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | `duration-100` | 100ms |
| Salida forzada | — | — | — | — | 0ms (instantáneo) |

---

## Implementación CSS

```css
.tooltip {
  pointer-events: none;
  opacity: 0;
  transition: none;
}

.tooltip.is-entering {
  opacity: 1;
  transition: opacity var(--motion-duration-from-motion-curve-sm) var(--motion-curve-sm);
}

.tooltip.is-exiting {
  opacity: 0;
  transition: opacity var(--motion-duration-from-motion-exit) var(--motion-exit);
}

.tooltip.is-forced-hidden,
.tooltip.is-hidden {
  opacity: 0;
  transition: none;
}

@media (prefers-reduced-motion: reduce) {
  .tooltip,
  .tooltip.is-entering,
  .tooltip.is-exiting {
    transition: none;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Motion | Solo `opacity` — sin `translate` ni `scale` |
| Variantes | Top/Bottom + Left/Center/Right cambian posición, no tokens |
| Hover tunneling | `pointer-events: none` en tooltip; hit area solo en trigger |
| Salida suave | `mouseleave` / `blur` → `motion-exit` · 100ms |
| Salida forzada | Esc, scroll, click trigger, Tab → instantáneo |
| Token | `motion-curve-sm` / `motion-exit` — categoría Default |
| Haptics | No aplica en Web |
| Accesibilidad | `prefers-reduced-motion`: estado final sin transición |
