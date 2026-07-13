# Motion Handoff — Dropdown (Web)

| | |
|---|---|
| **Componente** | Dropdown |
| **Plataforma** | Web |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token entrada panel** | `motion-curve-sm` |
| **Token salida suave** | `motion-exit` |
| **Token estados ítem** | `motion-curve-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-29 |
| **Figma** | [Core — Web Components](https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=10025-9193) |

---

## Motion Specification

Dropdown en Web despliega un panel flotante (`List` o `Slot`) anclado a un **trigger** (`Button` o `Icon Button`). El motion del trigger reutiliza los handoffs de Button / Icon Button — este documento cubre **panel** e **ítems**.

**Panel (`List` / `Slot`):** entrada con `opacity` 0→1 y `translateY` −4px→0 (desliza desde arriba del anclaje). Token: `motion-curve-sm` (150ms). Salida suave — click fuera, `Escape`, segundo click en trigger — con `motion-exit` (100ms) en las mismas propiedades. **Cierre al seleccionar ítem:** instantáneo (`transition: none`). Sin scrim. **Ancho:** el panel **crece horizontalmente** según el contenido (`width: max-content`); **sin scroll horizontal**.

**Ítems (`_Dropdown/Dropdown Item`):** hover, pressed y focus en `background-color` y focus ring (`opacity`). Token: `motion-curve-sm` (150ms). **Header:** sin estados interactivos — sin motion. **Disabled:** sin transición. **Submenú / Second Level / ítem con Action:** excluidos de este handoff.

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
| 7 | Trigger | Pointer enter ítem | `.dropdown-item` | — | — | — | — | — | * | — |
| 8 | Response | Hovered | `.dropdown-item` | `background-color` | enabled | hover | `motion-curve-sm` | 150ms | * | *+150 |
| 9 | Trigger | Pointer down ítem | — | — | — | — | — | — | * | — |
| 10 | Response | Pressed | `.dropdown-item` | `background-color` | hover/default | pressed | `motion-curve-sm` | 150ms | * | *+150 |
| 11 | Trigger | Focus visible ítem | — | — | — | — | — | — | * | — |
| 12 | Response | Focus ring | `.dropdown-item` | `opacity` (ring) | 0 | 1 | `motion-curve-sm` | 150ms | * | *+150 |
| 13 | Trigger | Disabled ítem | — | — | — | — | — | — | * | — |
| 14 | Response | Disabled | `.dropdown-item` | — | — | — | **sin animación** | — | — | — |

> `*` = momento variable según interacción.

---

## Token Mapping

| Momento | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor ms |
|---|---|---|---|---|---|
| Entrada panel | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |
| Salida suave panel | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | `duration-100` | 100ms |
| Ítem hover / pressed / focus | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |
| Cierre al seleccionar ítem | — | — | — | — | instantáneo |
| Header / disabled | — | — | — | — | instantáneo |

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

.dropdown-item {
  white-space: nowrap;
  transition: background-color var(--duration-150) var(--motion-curve-sm);
}

.dropdown-item::after {
  opacity: 0;
  transition: opacity var(--duration-150) var(--motion-curve-sm);
}

.dropdown-item:focus-visible::after {
  opacity: 1;
}

.dropdown-item:disabled {
  transition: none;
}

@media (prefers-reduced-motion: reduce) {
  .dropdown-panel,
  .dropdown-item {
    transition: none !important;
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Trigger | Reutilizar motion de Button / Icon Button — no duplicar tokens |
| Panel | `motion-curve-sm` entrada · `motion-exit` salida suave |
| Selección | Cierre instantáneo al elegir ítem |
| Ancho panel | Crece con el contenido — sin scroll horizontal |
| Header | Sin transición |
| Scroll vertical | Sin animación en lista larga |
| Submenú / Second Level | Excluidos del handoff |
| Skeleton | Excluido del handoff |
| Coherencia | Patrón cercano a Tooltip (fade + salida forzada), con `translateY` por anclaje inferior |
| Accesibilidad | `prefers-reduced-motion`: estados finales sin transición |
