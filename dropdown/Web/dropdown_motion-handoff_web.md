# Motion Handoff — Dropdown (Web)

| | |
|---|---|
| **Componente** | Dropdown |
| **Plataforma** | Web |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token entrada panel** | `motion-curve-sm` |
| **Token salida suave** | `motion-exit` |
| **Categoría** | Default |
| **Fecha** | 2026-06-29 |
| **Figma** | [Core — Web Components](https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=10025-9193) |

---

## Motion Specification

Dropdown en Web despliega un panel flotante (`List` o `Slot`) anclado a un **trigger** (`Button` o `Icon Button`). El motion del trigger vive en los handoffs de [Button](../../button/Web/button_motion-handoff_web.html) e [Icon Button](../../icon-button/Web/icon-button_motion-handoff_web.html) — **este documento cubre solo entrada y salida del panel**.

**Panel (`List` / `Slot`):** entrada con `opacity` 0→1 y `translateY` −4px→0 (desliza desde arriba del anclaje). Token: `motion-curve-sm` (150ms). Salida suave — click fuera, `Escape`, segundo click en trigger — con `motion-exit` (100ms) en las mismas propiedades. **Cierre al seleccionar ítem:** instantáneo (`transition: none`). Sin scrim. **Ancho:** el panel **crece horizontalmente** según el contenido (`width: max-content`); **sin scroll horizontal**.

**Ítems (`_Dropdown/Dropdown Item`):** fuera de scope de este handoff — hover, pressed y focus no se documentan acá. **Header:** sin estados interactivos. **Submenú / Second Level / ítem con Action:** excluidos de este handoff.

**Variantes** `Alignment` (Left / Center / Right), `List` ↔ `Slot`, cambio de trigger: **sin animación** — swap instantáneo. **Scroll vertical** en lista larga: funcional, sin motion. **Skeleton:** excluido del handoff.

> **Nota Figma:** este handoff documenta solo motion para implementación en código.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Click / toggle abre panel | — | — | — | — | — | — | * | — |
| 2 | Response | Entrada panel | `.dropdown-panel` | `opacity`, `translateY` | 0, −4px | 1, 0 | `motion-curve-sm` | 150ms | * | *+150 |
| 3 | Trigger | Click fuera / Escape / toggle cierra | — | — | — | — | — | — | * | — |
| 4 | Response | Salida suave panel | `.dropdown-panel` | `opacity`, `translateY` | 1, 0 | 0, −4px | `motion-exit` | 100ms | * | *+100 |
| 5 | Trigger | Click en ítem (selección) | — | — | — | — | — | — | * | — |
| 6 | Response | Cierre forzado panel | `.dropdown-panel` | `opacity`, `translateY` | visible | oculto | **sin animación** | — | — | — |

> `*` = momento variable según interacción. Motion de ítems y trigger: ver handoffs enlazados.

---

## Token Mapping

| Momento | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor ms |
|---|---|---|---|---|---|
| Entrada panel | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |
| Salida suave panel | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | `duration-100` | 100ms |
| Cierre al seleccionar ítem | — | — | — | — | instantáneo |

---

## Implementación CSS

```css
:root {
  --motion-curve-sm: cubic-bezier(0.4, 0, 0.2, 1);
  --duration-150: 150ms;
  --motion-exit: cubic-bezier(0.4, 0, 1, 1);
  --duration-100: 100ms;
}

.dropdown-panel {
  opacity: 0;
  transform: translateY(-4px);
  transition: none;
  pointer-events: none;
  width: max-content;
  min-width: 160px;
  overflow-x: visible;
}

.dropdown-panel.is-entering {
  opacity: 1;
  transform: translateY(0);
  pointer-events: auto;
  transition:
    opacity var(--duration-150) var(--motion-curve-sm),
    transform var(--duration-150) var(--motion-curve-sm);
}

.dropdown-panel.is-exiting {
  opacity: 0;
  transform: translateY(-4px);
  transition:
    opacity var(--duration-100) var(--motion-exit),
    transform var(--duration-100) var(--motion-exit);
}

.dropdown-panel.is-forced-hidden {
  opacity: 0;
  transform: translateY(-4px);
  transition: none;
  pointer-events: none;
}

@media (prefers-reduced-motion: reduce) {
  .dropdown-panel {
    transition: none !important;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Trigger | Handoffs de [Button](../../button/Web/button_motion-handoff_web.html) e [Icon Button](../../icon-button/Web/icon-button_motion-handoff_web.html) — no duplicar tokens |
| Panel | `motion-curve-sm` entrada · `motion-exit` salida suave |
| Ítems | Fuera de scope — no documentar hover/pressed/focus en este handoff |
| Selección | Cierre instantáneo al elegir ítem |
| Ancho panel | Crece con el contenido — sin scroll horizontal |
| Header | Sin transición |
| Scroll vertical | Sin animación en lista larga |
| Submenú / Second Level | Excluidos del handoff |
| Skeleton | Excluido del handoff |
| Coherencia | Patrón cercano a Tooltip (fade + salida forzada), con `translateY` por anclaje inferior |
| Accesibilidad | `prefers-reduced-motion`: estados finales sin transición |
