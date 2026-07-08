# Motion Handoff — Toast (Web)

| | |
|---|---|
| **Componente** | Toast |
| **Plataforma** | Web |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token entrada** | `motion-curve-lg` |
| **Token salida** | `motion-exit` |
| **Categoría** | Comunicación |
| **Fecha** | 2026-06-24 |

---

## Motion Specification

El Toast es feedback pasivo que informa sin bloquear la interacción con el contenido subyacente. Aparece en la **parte inferior derecha** de la pantalla, con ancho fijo de 320px, margen derecho de 24px y margen inferior de 24px respecto al safe area (o por encima del Bottom Nav si existe).

Se animan dos propiedades de forma simultánea en entrada y salida: `opacity` y `transform: translateY`. El desplazamiento vertical desde abajo (`100%` → `0%`) refuerza el anclaje espacial del componente; el fade suaviza la aparición y evita un corte visual abrupto. Este patrón es el estándar en Material 3 e iOS para snackbars y toasts flotantes.

La categoría **Comunicación** justifica `motion-curve-lg` (`easing-emphasis` · 500ms): presencia amable sin el peso de un modal. La salida usa `motion-exit` (`easing-accelerate` · 300ms), ~40% más rápida que la entrada, porque la persona usuaria ya procesó el mensaje o decidió cerrarlo.

Las variantes semánticas (Information, Success, Warning, Error) y la presencia de Action Link no cambian el motion de entrada/salida. `actionLink` es opcional (`false` por defecto). El Action Link afecta solo el ciclo de vida: sin auto-dismiss de 6s; el toast solo cierra con tap en X o swipe (App). El timer de 6 segundos comienza cuando **termina** la animación de entrada. Long press pausa el timer; al soltar, retoma. Solo se muestra un toast a la vez; el siguiente entra cuando el actual sale.

| Variante | Borde | Badge (Icon Badge) |
|---|---|---|
| Information | `#004799` | `#1d85fc` |
| Success | `#146542` | `#16a366` |
| Warning | `#664400` | `#ea9e06` |
| Error | `#d3315f` | `#d3315f` |

> **Delta Figma:** el handoff visual de componente menciona fade-in 300ms. Este documento alinea al Motion System GGDS (`motion-curve-lg` + slide), patrón más típico en DS de referencia y coherente con Snackbar.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Sistema encola y muestra toast (programmatic) | — | — | — | — | — | — | 0 | — |
| 2 | Response | Toast entra desde abajo | `.toast` | `opacity` | 0 | 1 | `motion-curve-lg` | 500ms | 0 | 500 |
| 3 | Response | Toast entra desde abajo | `.toast` | `translateY` | 100% | 0% | `motion-curve-lg` | 500ms | 0 | 500 |
| 4 | Trigger | Animación de entrada finaliza → inicia timer 6s (sin Action Link) | — | — | — | — | — | — | 500 | — |
| 5 | Trigger | Auto-dismiss tras 6000ms desde fin de entrada | — | — | — | — | — | — | 6500 | — |
| 6 | Response | Toast sale hacia abajo | `.toast` | `opacity` | 1 | 0 | `motion-exit` | 300ms | 6500 | 6800 |
| 7 | Response | Toast sale hacia abajo | `.toast` | `translateY` | 0% | 100% | `motion-exit` | 300ms | 6500 | 6800 |
| 8 | Trigger | Tap en botón cerrar (X) | — | — | — | — | — | — | * | — |
| 9 | Response | Toast sale (mismo motion que auto-dismiss) | `.toast` | `opacity`, `translateY` | visible | oculto | `motion-exit` | 300ms | * | *+300 |
| 10 | Trigger | Swipe lateral completado | — | — | — | — | — | — | * | — |
| 11 | Response | Toast sale por el costado | `.toast` | `opacity`, `translateX` | visible | fuera viewport lateral | `motion-exit` | 300ms | * | *+300 |
| 12 | Trigger | Long press sobre toast → pausa timer auto-dismiss | — | — | — | — | — | — | * | — |
| 13 | Trigger | Suelta long press → retoma timer | — | — | — | — | — | — | * | — |
| 14 | Trigger | Toast con Action Link visible → sin timer 6s | — | — | — | — | — | — | 500 | — |
| 15 | Trigger | Toast en cola espera cierre del toast activo | — | — | — | — | — | — | — | — |

> `*` = momento variable según interacción de la persona usuaria.
> Durante swipe, el toast sigue el dedo en `translateX`; al soltar por encima del umbral, dispara filas 10–11 con `motion-exit` hacia el costado (sin `translateY`).

---

## Token Mapping

| Dirección | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor ms |
|---|---|---|---|---|---|
| Entrada | `motion-curve-lg` | `easing-emphasis` | `cubic-bezier(0.2, 0, 0, 1)` | `duration-500` | 500ms |
| Salida | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | `duration-300` | 300ms |

---

## Implementación CSS

```css
.toast {
  transition-property: opacity, transform;
  transition-duration: var(--motion-duration-from-motion-curve-lg);
  transition-timing-function: var(--motion-curve-lg);
  opacity: 0;
  transform: translateY(100%);
}

.toast.is-entering {
  opacity: 1;
  transform: translateY(0);
}

.toast.is-exiting {
  transition-duration: var(--motion-duration-from-motion-exit);
  transition-timing-function: var(--motion-exit);
  opacity: 0;
  transform: translateY(100%);
}

@media (prefers-reduced-motion: reduce) {
  .toast,
  .toast.is-entering,
  .toast.is-exiting {
    transition: none;
  }
}
```

**Posicionamiento viewport (preview y producción):**

```css
.toast-viewport {
  position: fixed;
  right: 24px;
  bottom: 24px;
  width: 320px;
  max-width: calc(100vw - 48px);
  pointer-events: none;
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Patrón elegido | Slide desde abajo + fade (`motion-curve-lg`). Más típico que fade-only 300ms del handoff visual Figma |
| Posición | Abajo a la derecha; `right: 24px`, `bottom: 24px`; ancho 320px |
| Salida | Siempre `motion-exit` · 300ms, más corta que entrada |
| Triggers de salida | Auto-dismiss 6s post-entrada, tap X → `translateY` · swipe lateral → `translateX` — ambos con `motion-exit` |
| Action Link | Sin auto-dismiss; solo X o swipe |
| Long press | Pausa timer; no es transición animada |
| Cola | Un toast visible; el siguiente entra al cerrar el actual |
| Evitar | `scale`, `easing-back`, blur, scrim, stagger entre ícono/texto/link |
| Accesibilidad | `prefers-reduced-motion`: estado final sin transición |
| Haptics | No aplica en Web. En App: mapping por variante semántica (ver handoff App) |
