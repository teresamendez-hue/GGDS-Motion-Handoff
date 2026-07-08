# Motion Handoff — Carousel (Web)

| | |
|---|---|
| **Componente** | Carousel |
| **Plataforma** | Web |
| **Owner** | Ezequiel Arguello |
| **Design system** | GGDS |
| **Variantes** | `infinite` · `finite` |
| **Token semántico (slide)** | `motion-curve-md` |
| **Token semántico (dots)** | `motion-curve-sm` |
| **Categoría** | Guía (slide) · Default (dots) |
| **Fecha** | 2026-07-08 |

---

## Motion Specification

Carousel en Web desplaza el track horizontalmente (`translateX`) en modo **peek**: los slides adyacentes quedan parcialmente visibles. El cambio de slide — por swipe, flecha o dot — usa `motion-curve-md` en ambas variantes.

Existen dos variantes: **`infinite`** hace loop continuo del track (sin flechas disabled); **`finite`** pagina con inicio y fin (flechas disabled en extremos). Las flechas son **Icon Button** y su motion vive en el handoff de ese componente — este documento no las cubre.

Los **dots** animan el indicador activo con `motion-curve-sm`. No hay autoplay. El componente no tiene enter/exit de viewport; los slides se reemplazan in-place.

En **`infinite`**, la transición entre slides adyacentes es animada; el **reset de posición** al clonar el primero/último slide es instantáneo (`transition: none`). En **`prefers-reduced-motion`**, el track y los dots saltan al destino sin transición.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Swipe / flecha / dot | Carousel track | — | — | — | — | — |
| 2 | Response | Slide change | `.carousel__track` | `transform` | slide N | slide N±1 | `motion-curve-md` | 300ms |
| 3 | Trigger | Dot selected | `.carousel__dot` | — | — | — | — | — |
| 4 | Response | Active dot | `.carousel__dot` | `transform`, `opacity` | inactive | active | `motion-curve-sm` | 150ms |
| 5 | Trigger | Loop boundary (`infinite`) | `.carousel__track` | — | — | — | — | — |
| 6 | Response | Clone reset | `.carousel__track` | `transform` | clone position | real slide | — | 0ms |
| 7 | Trigger | `prefers-reduced-motion` | `.carousel__track`, `.carousel__dot` | — | — | — | — | — |
| 8 | Response | Reduced motion | track + dots | todas | — | final | — | 0ms |

> **Flechas:** estados hover, pressed, focus y disabled documentados en el handoff de **Icon Button**. No repetir en este componente.

---

## Token Mapping

| Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor | Uso |
|---|---|---|---|---|---|
| `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | `duration-300` | 300ms | Cambio de slide (ambas variantes) |
| `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms | Indicador activo en dots |

---

## Implementación CSS

```css
/* Track — cambio de slide (peek), ambas variantes */
.carousel__track {
  transition-property: transform;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

/* Infinite — reset instantáneo al clonar primero/último */
.carousel__track--no-transition {
  transition: none;
}

/* Dots — indicador activo */
.carousel__dot {
  transition-property: transform, opacity;
  transition-duration: var(--motion-duration-from-motion-curve-sm);
  transition-timing-function: var(--motion-curve-sm);
}

@media (prefers-reduced-motion: reduce) {
  .carousel__track,
  .carousel__dot {
    transition: none;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Slide | `motion-curve-md` — categoría Guía, coherente con Sidesheet |
| Dots | `motion-curve-sm` — microinteracción separada del slide |
| Flechas | Motion en handoff de **Icon Button** — no duplicar acá |
| `infinite` | Animar slides adyacentes; reset de clone sin transición |
| `finite` | Misma animación de slide; flechas disabled en extremos (Icon Button) |
| Peek | track `translateX`; slides adyacentes visibles parcialmente |
| Autoplay | no aplica |
| Haptics | no aplica en Web |
| A11y | `prefers-reduced-motion` → salto instantáneo al slide activo |
