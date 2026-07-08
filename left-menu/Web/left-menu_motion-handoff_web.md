# Motion Handoff — Left Menu (Web)

| | |
|---|---|
| **Componente** | Left Menu |
| **Plataforma** | Web |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token panel** | `motion-curve-md` |
| **Token submenú / ítem** | `motion-curve-sm` |
| **Token salida drawer** | `motion-exit` |
| **Categoría** | Guía |
| **Fecha** | 2026-06-29 |
| **Figma** | [Core — Web Components](https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=10251-8907) |

---

## Motion Specification

El Left Menu es la navegación principal **persistente** en el lateral del layout (desktop/tablet). Variantes: **Collapsed** (~68px, solo iconos), **Single Level**, **Navigation** (submenús de 2º y 3º nivel), scroll interno, Trigger para colapsar, estados de ítem (hover, pressed, focus, selected, skeleton).

A diferencia del Sidesheet/Drawer, en desktop/tablet el menú **no** entra con slide ni scrim: ocupa su columna y el contenido principal se adapta. La motion principal es el **ancho del panel** al colapsar/expandir (`motion-curve-md`, 300ms) junto con fade de labels y footer.

Los **submenús de nivel 1 y 2** usan expand vertical con chevron ↕: `grid-template-rows` 0fr↔1fr + `opacity` del inner + rotación del chevron (`motion-curve-sm`, 150ms). El **nivel 3** no expande hacia abajo: se abre un **flyout hacia la derecha** del ítem padre (chevron ›) con `translateX` + `opacity` (`motion-curve-sm`, 150ms). Sin stagger entre ítems hijos.

Los **estados de ítem** (superficie, focus ring) transicionan con `motion-curve-sm` (150ms), alineado con Button.

En **mobile**, cuando el producto usa variante **drawer overlay**, el panel entra desde la izquierda con `translateX` + scrim (`motion-curve-md` entrada, `motion-exit` salida 200ms). Esta variante es excepcional respecto al menú persistente.

El estado **collapsed** persiste entre sesiones (localStorage o equivalente).

> **Nota Figma:** este handoff documenta solo motion para implementación en código. No se modifican archivos ni prototipos en Figma.

---

## Timeline de interacción

### A — Panel colapsar / expandir (Trigger)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| A1 | Trigger | Tap en Trigger | — | — | — | — | — | — | 0 | — |
| A2 | Response | Panel colapsa o expande | `.left-menu` | `width` | 320px / 68px | 68px / 320px | `motion-curve-md` | 300ms | 0 | 300 |
| A3 | Response | Labels y footer | `.menu-label`, `.menu-footer` | `opacity` | 1 / 0 | 0 / 1 | `motion-curve-md` | 300ms | 0 | 300 |

### B — Submenú nivel 2

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| B1 | Trigger | Tap en ítem con hijos (nivel 1) | — | — | — | — | — | — | * | — |
| B2 | Response | Slot submenú | `.submenu-slot` | `grid-template-rows` | 0fr | 1fr | `motion-curve-sm` | 150ms | * | *+150 |
| B3 | Response | Contenido submenú | `.submenu-inner` | `opacity` | 0 | 1 | `motion-curve-sm` | 150ms | * | *+150 |
| B4 | Response | Chevron | `.item-chevron` | `transform` | 0deg | 180deg | `motion-curve-sm` | 150ms | * | *+150 |
| B5 | Trigger | Tap colapsar (nivel 1) | — | — | — | — | — | — | * | — |
| B6 | Response | Reverse B2–B4 | — | — | expandido | colapsado | `motion-curve-sm` | 150ms | * | *+150 |

### C — Subnivel 3 (flyout hacia la derecha)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| C1 | Trigger | Tap en ítem con hijos (nivel 2) o chevron › | — | — | — | — | — | — | * | — |
| C2 | Response | Panel flyout | `.submenu-flyout` | `transform` + `opacity` | translateX(-6px), 0 | translateX(0), 1 | `motion-curve-sm` | 150ms | * | *+150 |
| C3 | Trigger | Tap fuera / cerrar | — | — | — | — | — | — | * | — |
| C4 | Response | Reverse C2 | — | — | abierto | cerrado | `motion-curve-sm` | 150ms | * | *+150 |

> Chevron nivel 3: **solo derecha** (›). Sin rotación vertical.

### D — Estados de ítem

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| D1 | Response | Hover | `.menu-item` | `background-color` | default | hovered | `motion-curve-sm` | 150ms |
| D2 | Response | Pressed | `.menu-item` | `background-color` | hovered | pressed | `motion-curve-sm` | 150ms |
| D3 | Response | Focus visible | `.menu-item` | `box-shadow` (ring) | none | ring | `motion-curve-sm` | 150ms |
| D4 | Response | Selected | `.menu-item` | `background-color` | default | selected | `motion-curve-sm` | 150ms |

### E — Mobile drawer (solo variante overlay)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración |
|---|---|---|---|---|---|---|---|---|
| E1 | Trigger | Abrir menú mobile | — | — | — | — | — | — |
| E2 | Response | Panel entra | `.left-menu--drawer` | `transform` | translateX(-100%) | translateX(0) | `motion-curve-md` | 300ms |
| E3 | Response | Scrim | `.menu-scrim` | `opacity` | 0 | 1 | `motion-curve-md` | 300ms |
| E4 | Trigger | Cerrar (scrim / back) | — | — | — | — | — | — |
| E5 | Response | Panel sale | `.left-menu--drawer` | `transform` | translateX(0) | translateX(-100%) | `motion-exit` | 200ms |
| E6 | Response | Scrim sale | `.menu-scrim` | `opacity` | 1 | 0 | `motion-exit` | 200ms |

> `*` = momento variable según interacción.

---

## Token Mapping

| Momento | Token semántico | Curva primitiva | Valor CSS | Duración primitiva | Valor ms |
|---|---|---|---|---|---|
| Panel colapsar/expandir | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0, 0, 0.2, 1)` | `duration-300` | 300ms |
| Submenú L1/L2 (↕) | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |
| Flyout L3 (→) | `motion-curve-sm` | `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | `duration-150` | 150ms |
| Drawer entrada | `motion-curve-md` | `easing-decelerate` | `cubic-bezier(0, 0, 0.2, 1)` | `duration-300` | 300ms |
| Drawer salida | `motion-exit` | `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` | `duration-200` | 200ms |

---

## Decisiones de diseño

| Decisión | Elección | Motivo |
|---|---|---|
| Patrón desktop | Width + fade labels | Menú persistente; el layout respira sin overlay |
| Submenú L1/L2 | Grid 0fr↔1fr + chevron ↕ | Consistencia con Inline Message |
| Subnivel L3 | Flyout → con translateX + opacity | Figma Web: tercer nivel hacia la derecha, no indent vertical |
| Stagger ítems | No | Navegación utilitaria; evita ruido visual |
| Slide + scrim desktop | No | Reservado a Sidesheet; menú no es modal |
| Mobile | Drawer solo si variante overlay | Persistente por defecto; drawer documentado aparte |
| Collapsed | Persiste en sesión | Expectativa de usuario en apps enterprise |

---

## Implementación

### Panel

```css
.left-menu {
  width: var(--menu-width-expanded, 280px);
  transition: width 300ms cubic-bezier(0, 0, 0.2, 1);
}

.left-menu.is-collapsed {
  width: 68px;
}

.menu-label,
.menu-brand {
  opacity: 1;
  transition: opacity 300ms cubic-bezier(0, 0, 0.2, 1),
              max-width 300ms cubic-bezier(0, 0, 0.2, 1);
}

.left-menu.is-collapsed .menu-label,
.left-menu.is-collapsed .menu-brand {
  opacity: 0;
  max-width: 0;
  pointer-events: none;
}
```

### Submenú L1/L2

```css
.submenu-slot {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows 150ms cubic-bezier(0.4, 0, 0.2, 1);
}

.nav-group.is-expanded > .submenu-slot {
  grid-template-rows: 1fr;
}

.submenu-inner {
  overflow: hidden;
  min-height: 0;
  opacity: 0;
  transition: opacity 150ms cubic-bezier(0.4, 0, 0.2, 1);
}

.nav-group.is-expanded > .submenu-slot > .submenu-inner {
  opacity: 1;
}

.nav-group.is-expanded > .menu-item--parent .item-chevron--down {
  transform: rotate(180deg);
  transition: transform 150ms cubic-bezier(0.4, 0, 0.2, 1);
}
```

### Flyout L3

```css
.submenu-flyout {
  opacity: 0;
  transform: translateX(-6px);
  transition: opacity 150ms cubic-bezier(0.4, 0, 0.2, 1),
              transform 150ms cubic-bezier(0.4, 0, 0.2, 1);
}

.submenu-flyout.is-open {
  opacity: 1;
  transform: translateX(0);
}
```

### Estados de ítem

```css
.menu-item {
  transition: background-color 150ms cubic-bezier(0.4, 0, 0.2, 1);
}
```

### Mobile drawer

```css
.left-menu--drawer {
  transform: translateX(-100%);
  transition: transform 300ms cubic-bezier(0, 0, 0.2, 1);
}

.left-menu--drawer[data-open="true"] {
  transform: translateX(0);
}

.left-menu--drawer[data-open="false"] {
  transition-duration: 200ms;
  transition-timing-function: cubic-bezier(0.4, 0, 1, 1);
  transform: translateX(-100%);
}
```

### Accesibilidad

```css
@media (prefers-reduced-motion: reduce) {
  .left-menu,
  .menu-label,
  .menu-brand,
  .submenu-slot,
  .submenu-inner,
  .submenu-flyout,
  .menu-item,
  .item-chevron {
    transition: none !important;
  }
}
```

---

## Referencias GGDS

- **Button** — estados de ítem con `motion-curve-sm`
- **Inline Message** — expand submenú con grid + opacity
- **Sidesheet** — contraste: slide+scrim **no** aplica al menú persistente desktop

---

## Preview

Archivo: `left-menu_motion-handoff_web.html`

Interactuá con Trigger o controles Colapsar/Expandir, submenús L1/L2 (chevron ↕), flyout L3 en Categoría B (chevron ›) y selección de ítem. Checkbox `prefers-reduced-motion` desactiva transiciones. (Drawer mobile documentado en timeline E; preview en App.)
