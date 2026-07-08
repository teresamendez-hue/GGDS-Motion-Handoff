# Motion Handoff — Modal (Web)

| | |
|---|---|
| **Componente** | Modal |
| **Plataforma** | Web |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token entrada** | `motion-curve-xl` |
| **Token salida** | `motion-exit` |
| **Categoría** | Interactivos |
| **Fecha** | 2026-07-01 |
| **Figma** | [Core — Web Components · Modal](https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=9341-3731) |

---

## Motion Specification

El Modal es un overlay bloqueante que exige atención inmediata: confirmar decisiones, mostrar información crítica o completar tareas breves. A diferencia del Toast, interrumpe el flujo hasta que la persona usuaria resuelve la interacción. La categoría **Interactivos** justifica `motion-curve-xl` (700ms): ritmo pausado y confiable.

Se animan el **scrim** y el **panel** en paralelo. El scrim hace fade de opacidad 0 → 0.7 (`rgba(37, 37, 41, 0.7)` según Figma). El panel hace fade 0 → 1 y `translateY` 8% → 0. **No se usa scale** en Web — la librería de referencia usa solo desplazamiento vertical para anclar el diálogo al centro sin competir con Bottom Sheet.

La salida usa `motion-exit` (`easing-accelerate` · 400ms), ~40% más rápida que la entrada. Scrim y panel salen en paralelo con el mismo token.

Las variantes **Size** (S, M, L), **Type** (Default, Illustration), presencia de title/description/slot/close y combinaciones de **_Modal/Actions** no alteran el motion de entrada/salida. **Illustration** comparte el mismo motion que **Default**; la ilustración es contenido estático.

**Skeleton:** estado visual dentro del modal ya abierto. **Sin animación de swap** al cargar contenido real. **Scroll:** scroll nativo del cuerpo; sin tokens de motion adicionales.

**Cierre manual:** botón X, tap en scrim, tecla `Esc`, o botones de acción que cierran el flujo. Sin auto-dismiss por delay.

---

## Timeline de interacción

### Entrada

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Usuario o sistema abre el modal | — | — | — | — | — | — | 0 | — |
| 2 | Response | Scrim aparece | `.modal-scrim` | `opacity` | 0 | 0.7 | `motion-curve-xl` | 700ms | 0 | 700 |
| 3 | Response | Panel sube a posición | `.modal-panel` | `translateY` | 8% | 0% | `motion-curve-xl` | 700ms | 0 | 700 |
| 4 | Response | Panel aparece | `.modal-panel` | `opacity` | 0 | 1 | `motion-curve-xl` | 700ms | 0 | 700 |

### Salida

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Cierre: X, scrim, Esc, o acción que cierra | — | — | — | — | — | — | 0 | — |
| 2 | Response | Scrim desaparece | `.modal-scrim` | `opacity` | 0.7 | 0 | `motion-exit` | 400ms | 0 | 400 |
| 3 | Response | Panel baja | `.modal-panel` | `translateY` | 0% | 8% | `motion-exit` | 400ms | 0 | 400 |
| 4 | Response | Panel desaparece | `.modal-panel` | `opacity` | 1 | 0 | `motion-exit` | 400ms | 0 | 400 |

> Filas con mismo Inicio y Fin se ejecutan en paralelo.

---

## Token Mapping

| Dirección | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor ms |
|---|---|---|---|---|---|
| Entrada | `motion-curve-xl` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-700` | 700ms |
| Salida | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | `duration-400` | 400ms |

---

## Implementación CSS

```css
/* --- Tokens de motion --- */
--motion-curve-xl: cubic-bezier(0.4, 0, 0.2, 1);
--motion-exit: cubic-bezier(0.4, 0, 1, 1);
--duration-700: 700ms;
--duration-400: 400ms;

/* --- Estado oculto --- */
.modal-panel--hidden {
  opacity: 0;
  transform: translateY(8%);
  pointer-events: none;
}

.modal-scrim--hidden {
  opacity: 0;
  pointer-events: none;
}

/* --- Entrada --- */
.modal-panel--entering {
  transition:
    opacity var(--duration-700) var(--motion-curve-xl),
    transform var(--duration-700) var(--motion-curve-xl);
  opacity: 1;
  transform: translateY(0);
}

.modal-scrim--entering {
  transition: opacity var(--duration-700) var(--motion-curve-xl);
  opacity: 0.7;
}

/* --- Salida --- */
.modal-panel--exiting {
  transition:
    opacity var(--duration-400) var(--motion-exit),
    transform var(--duration-400) var(--motion-exit);
  opacity: 0;
  transform: translateY(8%);
}

.modal-scrim--exiting {
  transition: opacity var(--duration-400) var(--motion-exit);
  opacity: 0;
}

/* --- Accesibilidad --- */
@media (prefers-reduced-motion: reduce) {
  .modal-panel,
  .modal-scrim {
    transition: none;
  }
}
```

---

## Notas para Figma (prototipado)

1. Usar **Smart Animate** entre frames cerrado y abierto.
2. **Frame cerrado:** scrim `Opacity: 0%`; panel `Opacity: 0%`, desplazado `8%` hacia abajo en Y.
3. **Frame abierto:** scrim `Fill rgba(37,37,41,0.7)`; panel `Opacity: 100%`, `Y: 0`.
4. **Entrada:** Custom Bezier `0.4, 0, 0.2, 1` · 700ms.
5. **Salida:** Custom Bezier `0.4, 0, 1, 1` · 400ms.
6. **Skeleton:** mostrar placeholders en el frame abierto; al cambiar a contenido real, **sin** Smart Animate de swap.
7. **Scroll:** fijar header/footer; solo el slot central hace scroll (sin motion adicional).
8. Constraints del panel: `Center` en ambos ejes para Default e Illustration.

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Sin scale en Web | Coherencia con librería GGDS Web; translateY 8% es suficiente para anclaje espacial |
| No `easing-back` | Overshoot compite con contenido bloqueante |
| Size S/M/L | Solo afecta dimensiones; mismo motion |
| Acciones | Primary, Primary+Secondary, Destructive+Secondary, Primary+Action Link — motion de cierre idéntico |
| Accesibilidad | `prefers-reduced-motion: reduce` → estado final instantáneo |
| Paridad App | App usa scale en Default/Illustration; Web usa translateY — excepción documentada |

### Referencias externas

- [Material Design Motion](https://m3.material.io/styles/motion/overview)
- [Apple HIG — Animation](https://developer.apple.com/design/human-interface-guidelines/animation)
