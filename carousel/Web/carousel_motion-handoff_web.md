# Motion Handoff — Carousel (Web)

| | |
|---|---|
| **Componente** | Carousel |
| **Plataforma** | Web |
| **Owner** | Ezequiel Arguello |
| **Design system** | GGDS |
| **Token slide** | `motion-curve-md` |
| **Token microinteracciones** | `motion-curve-sm` |
| **Categoría** | Guía (slide) · Default (nav reveal / dot) |
| **Behavior** | Normal · Infinite Scroll |
| **Fecha** | 2026-07-02 |

---

## Motion Specification

Carousel permite navegar contenido relacionado en un espacio horizontal limitado mediante un track de slides que se desplaza lateralmente. A diferencia de un Modal o un Sidesheet, no es un componente que entra o sale del viewport: vive embebido en la página, por lo que todo su motion son transiciones de estado internas.

El cambio de slide anima `transform: translateX()` sobre el track completo usando **motion-curve-md**, con el mismo token en ambas direcciones — no aplica `motion-exit`, ya que no hay una jerarquía de entrada/salida como en un overlay, sino un reposicionamiento continuo dentro de una pista.

Las flechas de navegación permanecen ocultas por default (`Enabled`) y se revelan con opacity al pasar el mouse sobre el carousel (`Active`), usando **motion-curve-sm**. El dot indicator activo cambia de color con el mismo token, manteniendo coherencia entre todas las microinteracciones del componente.

En **Normal**, la flecha llega a estado `Disabled` de forma instantánea (sin transición) al alcanzar el primer o último slide. En **Infinite Scroll**, el índice hace loop continuo y la flecha nunca se deshabilita; ambos comportamientos comparten el mismo token de transición de slide.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Click flecha derecha / dot siguiente | `.carousel` | — | — | — | — | — |
| 2 | Response | Slide siguiente | `.carousel-track` | `transform` | `translateX(-n·100%)` | `translateX(-(n+1)·100%)` | `motion-curve-md` | 300ms |
| 3 | Trigger | Click flecha izquierda / dot anterior | `.carousel` | — | — | — | — | — |
| 4 | Response | Slide anterior | `.carousel-track` | `transform` | `translateX(-n·100%)` | `translateX(-(n-1)·100%)` | `motion-curve-md` | 300ms |
| 5 | Trigger | Pointer enter sobre el carousel | `.carousel` | — | — | — | — | — |
| 6 | Response | Reveal navegación | `.carousel-nav` | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms |
| 7 | Trigger | Pointer leave | `.carousel` | — | — | — | — | — |
| 8 | Response | Ocultar navegación | `.carousel-nav` | `opacity` | 1 | 0 | `motion-curve-sm` | 150ms |
| 9 | Trigger | Cambio de slide activo | `.carousel` | — | — | — | — | — |
| 10 | Response | Dot activo | `.dot` | `background-color` | `indicator` | `indicator-active` | `motion-curve-sm` | 150ms |
| 11 | Trigger | Llegada al límite (Normal) | `.carousel-nav` | — | — | — | — | — |
| 12 | Response | Disabled | `.carousel-nav` | — | — | final | — | 0ms |

---

## Token Mapping

| Interacción | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor |
|---|---|---|---|---|---|
| Slide transition | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` | `duration-300` | 300ms |
| Nav reveal / dot indicator | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |

---

## Implementación CSS

```css
/* Consumir tokens semánticos, no primitivos hardcodeados */
.carousel-track {
  transition-property: transform;
  transition-duration: var(--motion-duration-from-motion-curve-md);
  transition-timing-function: var(--motion-curve-md);
}

.carousel-nav {
  transition-property: opacity, background-color;
  transition-duration: var(--motion-duration-from-motion-curve-sm);
  transition-timing-function: var(--motion-curve-sm);
}

.carousel-nav:disabled {
  transition: none;
}

.carousel-dot {
  transition-property: background-color;
  transition-duration: var(--motion-duration-from-motion-curve-sm);
  transition-timing-function: var(--motion-curve-sm);
}

@media (prefers-reduced-motion: reduce) {
  .carousel-track,
  .carousel-nav,
  .carousel-dot {
    transition: none;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token único por dirección | El slide usa `motion-curve-md` en ambos sentidos; no aplicar `motion-exit` — el Carousel no abandona el viewport, solo reposiciona su track |
| Nav reveal | Exclusivo de Web. En touch no existe hover, por eso en App las flechas nunca se muestran (ver handoff App) |
| Rebote | Nunca `easing-back`: es un componente de contenido, no expresivo |
| Límite (Normal) | El estado `Disabled` de la flecha es instantáneo, sin transición |
| Accesibilidad | `prefers-reduced-motion` aplica el estado final sin transición. Flechas y dots navegables por teclado con foco visible |
