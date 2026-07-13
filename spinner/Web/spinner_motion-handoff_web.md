# Motion Handoff — Spinner (Web)

| | |
|---|---|
| **Componente** | Spinner |
| **Plataforma** | Web |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token rotación** | *(sin semántico)* → `easing-linear` · `duration-800` |
| **Categoría** | Default |
| **Fecha** | 2026-06-29 |
| **Figma** | [Core — Web Components](https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=6055-995) |

---

## Motion Specification

Spinner **indeterminado** de carga pasiva. Variantes de **Color** (accent, neutral, white) y **Size** (S → 5XL). Sin interacción hover/pressed/focus.

**Única animación documentada:** rotación continua del arco SVG mientras el componente está visible.

- **Rotación:** `transform: rotate(360deg)` en loop infinito.
- **Curva:** `easing-linear` · `cubic-bezier(0, 0, 1, 1)`.
- **Período:** `duration-800` · **800ms** por vuelta completa.
- **Sin token semántico** en el motion system actual — la cadena primitiva es la fuente de verdad. Misma convención que el spinner embebido en Button / Icon Button (0.8s linear).

**Sin animación** en: montaje/desmontaje (aparición instantánea), cambio de `color` o `size` (swap instantáneo), variante skeleton (excluida del handoff).

Sin viewport enter/exit. Sin stagger. Sin haptics (solo Web).

> **Nota Figma:** este handoff documenta solo motion para implementación en código.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Montaje / visible en DOM | — | — | — | — | — | — | 0 | — |
| 2 | Response | Aparición | `.spinner` | — | — | — | **sin animación** | — | 0 | 0 |
| 3 | Response | Rotación continua | `.spinner-svg` | `transform` (rotate) | 0° | 360° (loop) | `easing-linear` + `duration-800` | 800ms / vuelta | 0 | ∞ |
| 4 | Trigger | Cambio `color` o `size` | — | — | — | — | — | — | * | — |
| 5 | Response | Swap de variante | `.spinner` | color / dimensiones | anterior | nuevo | **sin animación** | — | — | — |
| 6 | Trigger | Desmontaje / oculto | — | — | — | — | — | — | * | — |
| 7 | Response | Salida | `.spinner` | — | — | — | **sin animación** | — | — | — |
| 8 | Trigger | `prefers-reduced-motion: reduce` | — | — | — | — | — | — | * | — |
| 9 | Response | Pausa rotación | `.spinner-svg` | `animation` | running | none | — | — | * | — |

> `*` = momento variable según props o sistema.

---

## Token Mapping

| Momento | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor ms |
|---|---|---|---|---|---|
| Rotación continua | *(ninguno)* | `easing-linear` | `cubic-bezier(0, 0, 1, 1)` | `duration-800` | 800ms / revolución |
| Montaje / desmontaje | — | — | — | — | instantáneo |
| Cambio color / size | — | — | — | — | instantáneo |
| `prefers-reduced-motion` | — | — | `animation: none` | — | rotación pausada |

---

## Implementación CSS

```css
:root {
  --easing-linear: cubic-bezier(0, 0, 1, 1);
  --duration-800: 800ms;
}

@keyframes spinner-rotate {
  to {
    transform: rotate(360deg);
  }
}

.spinner-svg {
  animation: spinner-rotate var(--duration-800) var(--easing-linear) infinite;
}

@media (prefers-reduced-motion: reduce) {
  .spinner-svg {
    animation: none !important;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token semántico | No existe `motion-spinner-*`; documentar primitivos. Evaluar token futuro si el patrón se repite fuera de Button |
| Coherencia Button / Icon Button | El spinner inline de esos componentes debe usar la misma cadena (`easing-linear` · `duration-800`) |
| Cambio de variante | Color y size: reemplazo visual instantáneo, sin `transition` |
| Skeleton | Excluido del handoff y del preview |
| Accesibilidad | `role="status"` o contenedor con `aria-busy="true"`; con `prefers-reduced-motion`: pausar rotación, mantener indicador visible |
| Figma | Referencia visual; implementación en código según este handoff |
