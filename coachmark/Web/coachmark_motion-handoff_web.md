# Motion Handoff — Coachmark (Web)

| | |
|---|---|
| **Componente** | Coachmark |
| **Plataforma** | Web |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token entrada** | `motion-curve-md` |
| **Token salida** | `motion-exit` |
| **Token cambio de step** | `motion-curve-sm` |
| **Categoría** | Guía |
| **Fecha** | 2026-06-25 |
| **Figma** | [Hand-Off Core Web](https://www.figma.com/design/KXyagGEUmiS08WO7TV8vVM/Hand-Off---Core-Web?node-id=5003-63630) |

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token entrada | `motion-curve-md` · 300ms | `motion-spring-md` |
| Token salida suave | `motion-exit` · 200ms | `motion-spring-md` (invertido) |
| Cambio de step | `motion-curve-sm` · 150ms × 2 | `motion-spring-sm` |
| Salida forzada | instantánea | instantánea |
| Propiedades | opacity + translate 8px eje puntero | opacity + translate 8px eje puntero |

**Principio:** el motion debe sentirse lo más similar posible entre plataformas. Misma intención, mismos triggers, mismas propiedades — tokens distintos (curva vs spring). Ver [handoff App](../App/coachmark_motion-handoff_app.md) para implementación Flutter.

---

## Motion Specification

El Coachmark guía a la persona usuaria en un tour contextual sobre un elemento target de la interfaz. Se ancla al target con un puntero triangular; ancho fijo 320px, fondo `accent-1` (#d9e1b8), radio 8px y sombra según Figma. **No hay scrim ni overlay animado** — el contenido subyacente permanece visible e interactivo fuera del panel.

La entrada usa `motion-curve-md` (300ms): `opacity` 0→1 y `translate` 8px en el **eje del puntero** (desde la dirección opuesta al target). La salida suave — X, Omitir, Finalizar en último paso — invierte el motion con `motion-exit` (200ms). Cierre forzado (Escape, target fuera del viewport por scroll) es **instantáneo** (`transition: none`).

El avance entre pasos anima solo el **contenido interno** (título, descripción, contador) con crossfade `motion-curve-sm` (150ms). Si el salto de posición al siguiente target es grande, el panel completo puede re-entrar con `motion-curve-md`; en saltos pequeños el panel permanece y solo crossfadea el contenido.

Variantes de puntero — **Top, Bottom, Left, Right** con auto-flip en borde de viewport — afectan layout y dirección del translate, **no** los tokens. Variantes de contenido (título, steps "1 de 4", secondary) tampoco cambian motion.

Funcional (fuera del contrato motion): scroll sigue al target vía layout; dismiss X/Omitir persiste permanentemente; focus trap y a11y en Web.

> **Delta Figma:** sin delta de motion reportado. Tokens alineados a categoría Guía del Motion System GGDS.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Tour inicia / step se muestra (programmatic) | — | — | — | — | — | — | 0 | — |
| 2 | Response | Coachmark entra | `.coachmark` | `opacity` | 0 | 1 | `motion-curve-md` | 300ms | 0 | 300 |
| 3 | Response | Coachmark entra | `.coachmark` | `translate` (eje puntero) | 8px | 0 | `motion-curve-md` | 300ms | 0 | 300 |
| 4 | Trigger | Tap en X | — | — | — | — | — | — | * | — |
| 5 | Response | Coachmark sale (suave) | `.coachmark` | `opacity` | 1 | 0 | `motion-exit` | 200ms | * | *+200 |
| 6 | Response | Coachmark sale (suave) | `.coachmark` | `translate` (eje puntero) | 0 | 8px | `motion-exit` | 200ms | * | *+200 |
| 7 | Trigger | Tap en Secondary (Omitir) | — | — | — | — | — | — | * | — |
| 8 | Response | Mismo motion que filas 5–6 | `.coachmark` | `opacity`, `translate` | visible | oculto | `motion-exit` | 200ms | * | *+200 |
| 9 | Trigger | Tap en Primary en último step (Finalizar) | — | — | — | — | — | — | * | — |
| 10 | Response | Mismo motion que filas 5–6 | `.coachmark` | `opacity`, `translate` | visible | oculto | `motion-exit` | 200ms | * | *+200 |
| 11 | Trigger | Tap en Primary (no último step) | — | — | — | — | — | — | * | — |
| 12 | Response | Crossfade contenido | `.coachmark-body` | `opacity` | 1 | 0 → 1 | `motion-curve-sm` | 150ms | * | *+150 |
| 13 | Response | Reposicionar panel (salto grande) | `.coachmark` | `opacity`, `translate` | oculto | visible | `motion-curve-md` | 300ms | * | *+300 |
| 14 | Trigger | `Escape` | — | — | — | — | — | — | * | — |
| 15 | Response | Cierre forzado | `.coachmark` | `opacity`, `translate` | visible | oculto | none | 0ms | * | * |
| 16 | Trigger | Target sale del viewport (scroll) | — | — | — | — | — | — | * | — |
| 17 | Response | Cierre forzado | `.coachmark` | `opacity`, `translate` | visible | oculto | none | 0ms | * | * |

> `*` = momento variable según interacción o avance del tour.
> Filas 13: solo si el salto de posición al siguiente target supera umbral de producto; si no, solo fila 12.

---

## Token Mapping

| Momento | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor ms |
|---|---|---|---|---|---|
| Entrada | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0, 0, 0.2, 1)` | `duration-300` | 300ms |
| Salida suave | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | `duration-200` | 200ms |
| Cambio de step | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |
| Salida forzada | — | — | — | — | 0ms (instantáneo) |

---

## Implementación CSS

```css
:root {
  --motion-curve-md: cubic-bezier(0, 0, 0.2, 1);
  --motion-curve-sm: cubic-bezier(0.4, 0, 0.2, 1);
  --motion-exit: cubic-bezier(0.4, 0, 1, 1);
  --duration-300: 300ms;
  --duration-200: 200ms;
  --duration-150: 150ms;
}

.coachmark {
  opacity: 0;
  transition: none;
}

.coachmark.is-entering {
  opacity: 1;
  transition:
    opacity var(--duration-300) var(--motion-curve-md),
    transform var(--duration-300) var(--motion-curve-md);
}

.coachmark.is-exiting {
  opacity: 0;
  transition:
    opacity var(--duration-200) var(--motion-exit),
    transform var(--duration-200) var(--motion-exit);
}

.coachmark-body {
  transition: opacity var(--duration-150) var(--motion-curve-sm);
}

.coachmark-body.is-fading {
  opacity: 0;
}

.coachmark.is-forced-hidden,
.coachmark.is-hidden {
  opacity: 0;
  transition: none;
}

@media (prefers-reduced-motion: reduce) {
  .coachmark,
  .coachmark.is-entering,
  .coachmark.is-exiting,
  .coachmark-body {
    transition: none;
  }
}
```

**Dirección del translate por puntero (entrada desde opuesto al target):**

| Puntero | `transform` inicial (hidden/exiting) | `transform` final (entering) |
|---|---|---|
| Top | `translateY(8px)` | `translateY(0)` |
| Bottom | `translateY(-8px)` | `translateY(0)` |
| Left | `translateX(8px)` | `translateX(0)` |
| Right | `translateX(-8px)` | `translateX(0)` |

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Categoría | Guía → `motion-curve-md` entrada, coherente con Drawer/Sidesheet |
| Scrim | **Sin** animación de overlay; fondo visible |
| Entrada | Fade + 8px en eje del puntero — refuerza origen espacial |
| Salida suave | `motion-exit` · 200ms (~40% menor que entrada) |
| Salida forzada | Esc, target off-screen → instantáneo |
| Cambio de step | Solo crossfade contenido con `motion-curve-sm` · 150ms |
| Reposicionamiento | `motion-curve-md` solo en saltos grandes entre targets |
| Variantes puntero | Top/Bottom/Left/Right + auto-flip — no cambian tokens |
| Persistencia | X/Omitir dismiss permanente (lógica de negocio, no motion) |
| Scroll | Coachmark sigue target vía layout; auto-close off-screen |
| Evitar | `easing-back`, scrim animado, stagger excesivo, scale |
| Accesibilidad | Focus trap en panel; `prefers-reduced-motion`: estado final sin transición |
| Haptics | No aplica en Web |
