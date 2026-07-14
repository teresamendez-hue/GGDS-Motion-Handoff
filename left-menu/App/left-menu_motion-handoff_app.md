# Motion Handoff — Left Menu (App)

| | |
|---|---|
| **Componente** | Left Menu |
| **Plataforma** | App (Flutter) |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token drawer / columna** | `motion-spring-md` |
| **Token ítem** | `motion-spring-sm` |
| **Categoría** | Guía |
| **Fecha** | 2026-06-29 |
| **Figma** | [Core — App Components](https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=9932-3221) |

---

## Motion Specification

En **mobile**, el Side Menu es un **drawer** de 272dp. Los ítems del mismo nivel comparten alineación (sin indentación progresiva). Cuando un ítem tiene hijos, el tap **navega a la siguiente columna/página** del menú con slide horizontal (`motion-spring-md`); el chevron indica drill con **›** (solo derecha). No hay expand vertical, `AnimatedSize` ni chevrons arriba/abajo.

**Tablet/desktop** (patrón distinto al preview mobile): panel persistente que colapsa a ~68dp con iconos; width + opacity de labels con `motion-spring-md`.

Estados de ítem (pressed, selected) usan `motion-spring-sm` sobre `Color` / `Decoration`. Sin stagger.

**Haptics:** `haptic-action-press` en abrir/cerrar drawer, drill a siguiente columna, atrás y tap de ítem hoja. Sin haptic en slide de columna ni en scroll.

**Mobile drawer:** panel con `Offset` + scrim opacity vía `motion-spring-md`; salida con `Curves.easeIn` ~180ms (criterio GGDS para salidas más rápidas que spring de entrada).

Estado collapsed en tablet/desktop **persiste** entre sesiones (`SharedPreferences` o equivalente). No aplica al drawer mobile.

> **Nota Figma:** este handoff documenta solo motion para implementación en código.

---

## Timeline de interacción

### A — Panel colapsar / expandir (tablet/desktop)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Inicio |
|---|---|---|---|---|---|---|---|---|
| A1 | Trigger | Tap Trigger | — | — | — | — | — | 0 |
| A2 | Haptic | Confirmación tap | — | `haptic-action-press` | — | — | lightImpact | 0 |
| A3 | Response | Panel | `AnimatedContainer` width | collapsed ↔ expanded | `motion-spring-md` | 0 |
| A4 | Response | Labels | `Opacity` / `FadeTransition` | 0 ↔ 1 | `motion-spring-md` | 0 |

### B — Navegación columna nivel 2

| # | Tipo | Evento | Elemento | Propiedad | Token |
|---|---|---|---|---|---|
| B1 | Trigger | Tap ítem con hijos (›) | — | — | — |
| B2 | Haptic | Drill | — | `haptic-action-press` | — |
| B3 | Response | Columnas track | `translateX` / `PageView` | L1 → L2 | `motion-spring-md` |
| B4 | Response | Header columna | título + botón atrás | visible | — |

### C — Navegación columna nivel 3

Misma estructura que B: tap ítem con hijos en L2 desliza a columna L3 con `motion-spring-md`.

### D — Atrás (←)

| # | Tipo | Evento | Elemento | Propiedad | Token |
|---|---|---|---|---|---|
| D1 | Trigger | Tap «Atrás» | — | — | — |
| D2 | Haptic | Confirmación | — | `haptic-action-press` | — |
| D3 | Response | Columnas track | `translateX` | Ln → Ln−1 | `motion-spring-md` |

### E — Navegación (ítem hoja)

| # | Tipo | Evento | Token motion | Token haptic |
|---|---|---|---|---|
| E1 | Trigger | Tap ítem sin hijos | — | `haptic-action-press` |
| E2 | Response | Selected state | `motion-spring-sm` | — |

### F — Mobile drawer

| # | Momento | Token entrada | Token salida |
|---|---|---|---|
| F1 | Panel slide + scrim | `motion-spring-md` | `Curves.easeIn` ~180ms |
| F2 | Cerrar scrim / back | — | idem salida |

---

## Token Mapping

| Momento | Token semántico | mass | stiffness | damping |
|---|---|---|---|---|
| Drawer / columna | `motion-spring-md` | 1.0 | 300 | 28 |
| Panel collapse (tablet/desktop) | `motion-spring-md` | 1.0 | 300 | 28 |
| Ítem (pressed / selected) | `motion-spring-sm` | 1.0 | 400 | 35 |
| Drawer salida | `Curves.easeIn` | — | — | ~180ms |

---

## Haptics

| Momento | Token semántico | Primitivo | API |
|---|---|---|---|
| Abrir / cerrar drawer | `haptic-action-press` | `haptic-impact-light` | `HapticFeedback.lightImpact()` |
| Tap ítem hoja | `haptic-action-press` | `haptic-impact-light` | `HapticFeedback.lightImpact()` |
| Tap ítem con hijos (drill →) | `haptic-action-press` | `haptic-impact-light` | `HapticFeedback.lightImpact()` |
| Tap atrás (←) | `haptic-action-press` | `haptic-impact-light` | `HapticFeedback.lightImpact()` |
| Trigger colapsar/expandir (tablet/desktop) | `haptic-action-press` | `haptic-impact-light` | `HapticFeedback.lightImpact()` |
| Slide columna / scroll | `none` | — | — |

---

## Decisiones de diseño

| Decisión | Elección | Motivo |
|---|---|---|
| Submenú mobile | Columna siguiente (slide horizontal) | Figma App: sin indentación; misma alineación por nivel |
| Chevron | Solo › (derecha) | Indica drill, no expand/collapse vertical |
| Spring columna / drawer | `motion-spring-md` | Cambio de vista notable pero no modal |
| Spring ítem | `motion-spring-sm` | Microinteracciones (Button, Inline Message) |
| Stagger | No | Claridad en navegación densa |
| Drawer desktop | No | Menú persistente en tablet/desktop |
| Salida drawer | easeIn 180ms | Criterio GGDS salida ~40% más rápida |

---

## Implementación (flutter_animate)

```dart
// Drawer — motion-spring-md
.animate(target: open ? 1 : 0)
.slideX(begin: -1, end: 0, curve: GgdsMotion.springMd);

// Navegación por columna — motion-spring-md
// PageController o AnimatedSlide según profundidad (0, 1, 2)
.animate(target: depth.toDouble())
.custom(
  builder: (context, value, child) => Transform.translate(
    offset: Offset(-columnWidth * value, 0),
    child: child,
  ),
);

// Ítem selected — motion-spring-sm
.animate(target: selected ? 1 : 0)
.custom(/* Color.lerp / decoration */);

// Chevron: icono Navigation (›) estático; sin RotationTransition

// Haptic en drill
onTap: () {
  HapticFeedback.lightImpact();
  navigateToColumn(nextDepth);
}
```

Usar los helpers del design system (`GgdsMotion.springMd`, etc.) cuando estén disponibles en lugar de valores sueltos.

---

## Referencias GGDS

- **Button** — `motion-spring-sm` en estados
- **Sidesheet** — patrón drawer modal (slide + scrim)

---

## Preview

Archivo: `left-menu_motion-handoff_app.html`

Preview en **viewport mobile** (390×640): pantalla de app + Side Menu como **drawer** de 272px (Figma `Device=Mobile`). Navegación por **columnas** (L1 → L2 → L3), botón atrás, chevron › en ítems con hijos, drawer entrada/salida, selección y footer con thumbnail + icono genérico. Controles: Entrada / Salida / Reset, skeleton y `disableAnimations`.
