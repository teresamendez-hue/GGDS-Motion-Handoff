# Motion Handoff — Progress Indicator (Web)

| | |
|---|---|
| **Componente** | Progress Indicator |
| **Plataforma** | Web |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token cambio de %** | `motion-curve-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-07-01 |
| **Figma** | [Core — Web Components](https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=8353-3332) |

---

## Motion Specification

Indicador de progreso **determinado** (con porcentaje numérico). Variantes **Bar** y **Circular** con paridad App/Web. Props opcionales: label, description, percentage.

**Única animación documentada:** cuando cambia el valor de `percentage` (p. ej. 20% → 45%).

- **Bar:** `width` del fill de 0 a N% del track.
- **Circular:** `stroke-dashoffset` del arco de progreso (circunferencia completa → N%).
- **Texto de porcentaje:** actualiza en **paralelo** con el fill (mismo token y duración).

Token: `motion-curve-sm` (150ms). Mismo token al subir o bajar el valor.

**Sin animación** en: montaje inicial del componente, cambio Bar ↔ Circular, toggle de label/description, modo skeleton (excluido del handoff).

Sin viewport enter/exit. Sin stagger. Sin haptics.

> **Nota Figma:** este handoff documenta solo motion para implementación en código.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Cambio de `percentage` (programático o data) | — | — | — | — | — | — | * | — |
| 2 | Response | Bar fill | `.progress-fill` | `width` | valor anterior % | nuevo % | `motion-curve-sm` | 150ms | * | *+150 |
| 3 | Response | Circular arc | `.progress-circle-fill` | `stroke-dashoffset` | offset anterior | offset nuevo | `motion-curve-sm` | 150ms | * | *+150 |
| 4 | Response | Texto % | `.progress-pct` | contenido / opacity | % anterior | % nuevo | `motion-curve-sm` | 150ms | * | *+150 |
| 5 | Trigger | Cambio Type Bar ↔ Circular | — | — | — | — | — | — | * | — |
| 6 | Response | Swap de variante | root | — | — | — | **sin animación** | — | — | — |
| 7 | Trigger | Toggle label / description | — | — | — | — | — | — | * | — |
| 8 | Response | Visibilidad texto | label, desc | display | — | — | **sin animación** | — | — | — |

> `*` = momento variable según actualización de datos.

---

## Token Mapping

| Momento | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor ms |
|---|---|---|---|---|---|
| Cambio de % (Bar, Circular, texto) | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |

---

## Implementación CSS

```css
:root {
  --motion-curve-sm: cubic-bezier(0.4, 0, 0.2, 1);
  --duration-150: 150ms;
}

.progress-fill {
  transition: width var(--duration-150) var(--motion-curve-sm);
}

.progress-circle-fill {
  transition: stroke-dashoffset var(--duration-150) var(--motion-curve-sm);
}

@media (prefers-reduced-motion: reduce) {
  .progress-fill,
  .progress-circle-fill {
    transition: none !important;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Determinate only | Solo documentar progreso con %; no hay modo indeterminado en Figma |
| Paridad App | Mismo comportamiento; App usa `motion-spring-sm` |
| Type swap | Bar ↔ Circular: reemplazo visual instantáneo |
| Skeleton | Excluido del handoff y del preview |
| Accesibilidad | `role="progressbar"`, `aria-valuenow` / `aria-valuemin` / `aria-valuemax`; `prefers-reduced-motion`: valor final sin transición |
| Figma | Referencia visual; implementación en código según este handoff |
