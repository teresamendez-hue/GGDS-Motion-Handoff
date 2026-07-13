# Motion Handoff — Pagination (Web)

| | |
|---|---|
| **Componente** | Pagination |
| **Plataforma** | Web |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token semántico** | `motion-curve-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-07-02 |
| **Figma** | [Core — Web Components](https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=7959-66) |

---

## Motion Specification

Pagination en Web es un control de navegación por páginas **persistente en layout** (sin enter/exit de viewport). Compuesto por **`_Pagination/Navigation`** (prev, next, ellipsis) y **`_Pagination/Number`** (celdas numéricas).

**Navigation:** `background-color` en hover y pressed; focus ring con `opacity` 0→1. Token: `motion-curve-sm` (150ms). `Disabled` en prev/next: **sin transición** — estado instantáneo.

**Number:** al cambiar `Selected`, transición de `background-color` y `color` entre `Enabled` y `Selected` con el mismo token (150ms). Sin hover dedicado en Figma para Number — solo feedback al interactuar vía pressed en la celda clickeable.

**Rearmado de ventana** (`items: +4`, ellipsis, salto de rango): reemplazo **instantáneo** del set de números visibles. Sin fade, sin slide, sin stagger. Referencia sistémica: Breadcrumb anima el *expand* del trail colapsado; en Pagination el swap de páginas visibles es utilitario (Material / Fluent: sin animación en el listado).

Sin skeleton. Sin scrim. Sin haptics (solo Web).

> **Nota Figma:** este handoff documenta solo motion para implementación en código.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer enter Navigation | `.pagination-nav` | — | — | — | — | — | * | — |
| 2 | Response | Hovered | `.pagination-nav` | `background-color` | enabled | hover | `motion-curve-sm` | 150ms | * | *+150 |
| 3 | Trigger | Pointer down Navigation | — | — | — | — | — | — | * | — |
| 4 | Response | Pressed | `.pagination-nav` | `background-color` | hover/default | pressed | `motion-curve-sm` | 150ms | * | *+150 |
| 5 | Trigger | Focus visible Navigation | — | — | — | — | — | — | * | — |
| 6 | Response | Focus ring | `.pagination-nav` | `opacity` (ring) | 0 | 1 | `motion-curve-sm` | 150ms | * | *+150 |
| 7 | Trigger | Click Number (cambio página) | — | — | — | — | — | — | * | — |
| 8 | Response | Selected anterior | `.pagination-number` | `background-color`, `color` | selected | enabled | `motion-curve-sm` | 150ms | * | *+150 |
| 9 | Response | Selected nuevo | `.pagination-number` | `background-color`, `color` | enabled | selected | `motion-curve-sm` | 150ms | * | *+150 |
| 10 | Trigger | Prev / Next / Ellipsis (rearmado +4) | — | — | — | — | — | — | * | — |
| 11 | Response | Ventana de números | `.pagination-window` | DOM / contenido | set anterior | set nuevo | **sin animación** | — | — | — |
| 12 | Trigger | Prev/Next disabled | — | — | — | — | — | — | * | — |
| 13 | Response | Disabled | `.pagination-nav` | `color`, pointer | enabled | disabled | **sin animación** | — | — | — |

> `*` = momento variable según interacción.

---

## Token Mapping

| Momento | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor ms |
|---|---|---|---|---|---|
| Navigation hover / pressed / focus | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |
| Number Selected ↔ Enabled | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |
| Rearmado ventana (+4) | — | — | — | — | instantáneo |
| Disabled prev/next | — | — | — | — | instantáneo |

---

## Implementación CSS

```css
:root {
  --motion-curve-sm: cubic-bezier(0.4, 0, 0.2, 1);
  --duration-150: 150ms;
}

.pagination-nav,
.pagination-number {
  transition:
    background-color var(--duration-150) var(--motion-curve-sm),
    color var(--duration-150) var(--motion-curve-sm);
}

.pagination-nav::after,
.pagination-number::after {
  /* focus ring */
  transition: opacity var(--duration-150) var(--motion-curve-sm);
}

.pagination-nav:disabled {
  transition: none;
}

/* Ventana de números: sin transition al reemplazar nodos */

@media (prefers-reduced-motion: reduce) {
  .pagination-nav,
  .pagination-number {
    transition: none !important;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-curve-sm` para estados internos y selección |
| Variantes Figma | `items` (1, 2, 3, 4, +4) × `position` (Start, Middle, End) |
| Ventana +4 | Swap instantáneo del listado; no replicar expand de Breadcrumb |
| Disabled | Prev en Start / Next en End — sin transición |
| Exit viewport | No aplica |
| Accesibilidad | `aria-current="page"` en número activo; `prefers-reduced-motion`: estado final sin transición |
| Skeleton | Excluido del handoff |
