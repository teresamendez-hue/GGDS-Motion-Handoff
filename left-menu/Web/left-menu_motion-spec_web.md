---
component: Left Menu
component_slug: left-menu
platform: web
category: guide
semantic_token_enter: motion-curve-md
semantic_token_exit: motion-exit
owner: Nehuén Benitez
date: 2026-06-29
design_system: GGDS
status: draft
---

## Descripción

Navegación lateral **persistente** (no overlay en desktop/tablet). Colapsar/expandir panel (~68px solo icono), submenús L1/L2 con expand vertical (chevron ↕), subnivel L3 con flyout hacia la derecha (chevron ›), estados de ítem. Sin stagger ni slide+scrim salvo variante **mobile drawer** explícita.

## Tokens

### Panel colapsar / expandir (Trigger)
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Submenú expand / collapse (niveles 1 y 2)
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150
- nota: chevron ↕ con rotación 180°

### Subnivel 3 — flyout hacia la derecha
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150
- nota: chevron › sin rotación; translateX + opacity del panel

### Estados de ítem (hover / pressed / focus / selected)
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

### Mobile drawer — entrada
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Mobile drawer — salida
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-200
- duration_ms: 200

## Propiedades animadas

### Panel — Colapsar / expandir
| propiedad | de | a | token |
|---|---|---|---|
| width | 320px (desktop) / 288px (tablet) | 68px | motion-curve-md |
| opacity (labels, footer text) | 1 | 0 | motion-curve-md |
| opacity (labels, footer text) | 0 | 1 | motion-curve-md |

### Submenú — Niveles 1 y 2 (vertical)
| propiedad | de | a | token |
|---|---|---|---|
| grid-template-rows (slot) | 0fr | 1fr | motion-curve-sm |
| opacity (inner) | 0 | 1 | motion-curve-sm |
| transform (chevron ↕) | 0deg | 180deg | motion-curve-sm |

### Subnivel 3 — Flyout (derecha)
| propiedad | de | a | token |
|---|---|---|---|
| transform (panel) | translateX(-6px) | translateX(0) | motion-curve-sm |
| opacity (panel) | 0 | 1 | motion-curve-sm |

### Ítem — Estados
| propiedad | de | a | token |
|---|---|---|---|
| background-color | default | hovered / pressed / selected | motion-curve-sm |
| outline / box-shadow (focus-visible) | 0 | ring visible | motion-curve-sm |

### Mobile drawer
| propiedad | de | a | token |
|---|---|---|---|
| transform (panel) | translateX(-100%) | translateX(0) | motion-curve-md |
| opacity (scrim) | 0 | 1 | motion-curve-md |
| transform (panel) | translateX(0) | translateX(-100%) | motion-exit |
| opacity (scrim) | 1 | 0 | motion-exit |

## Triggers

### Colapsar / expandir panel
- evento: tap en Trigger (chevron lateral)
- tipo: user-action
- persistencia: estado collapsed se guarda entre sesiones

### Submenú L1/L2
- evento: tap en ítem con hijos o chevron ↕
- tipo: user-action

### Subnivel L3 (flyout)
- evento: tap en ítem nivel 2 con hijos o chevron ›
- tipo: user-action

### Navegación
- evento: tap en ítem hoja (sin hijos)
- tipo: user-action

### Mobile drawer
- evento: abrir menú (hamburger / gesture) · cerrar (scrim, swipe, back)
- tipo: user-action

## Restricciones

- Sin stagger de ítems al expandir panel o submenú
- Desktop/tablet: menú persistente en layout — **no** usar patrón Sidesheet/Drawer (slide + scrim)
- Mobile overlay: solo cuando la variante de producto lo define explícitamente
- Chevron L1/L2: ↕ (expand vertical); L3: › (flyout derecha)
- `prefers-reduced-motion`: transiciones a 0ms; estados finales inmediatos
