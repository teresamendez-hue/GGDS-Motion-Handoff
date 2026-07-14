---
component: Left Menu
component_slug: left-menu
platform: app
category: guide
semantic_token_enter: motion-spring-md
owner: Nehuén Benitez
date: 2026-06-29
design_system: GGDS
status: draft
preview_type: viewport
---

## Descripción

Navegación lateral en **app mobile** como drawer (272dp). Los niveles de profundidad no usan indentación vertical: cada nivel es una **columna/página** dentro del menú, con slide horizontal (`motion-spring-md`). Chevron **solo hacia la derecha** (›) en ítems con hijos; sin chevrons arriba/abajo ni rotación.

**Tablet/desktop** (fuera del preview mobile): panel persistente con collapse a ~68dp (`motion-spring-md`).

Estados de ítem (pressed, selected) usan `motion-spring-sm`. Haptics en acciones de usuario, no en animaciones pasivas.

## Tokens

### Panel colapsar / expandir
- semantic: motion-spring-md
- mass: mass-1 → 1.0
- stiffness: stiffness-300 → 300
- damping: damping-28 → 28

### Navegación por columna (niveles 2 y 3)
- semantic: motion-spring-md
- mass: mass-1 → 1.0
- stiffness: stiffness-300 → 300
- damping: damping-28 → 28
- nota: slide horizontal entre columnas; **no** height expand ni chevron ↑/↓

### Estados de ítem
- semantic: motion-spring-sm
- mass: mass-1 → 1.0
- stiffness: stiffness-400 → 400
- damping: damping-35 → 35

### Mobile drawer — entrada
- semantic: motion-spring-md
- mass: mass-1 → 1.0
- stiffness: stiffness-300 → 300
- damping: damping-28 → 28

### Mobile drawer — salida
- semantic: Curves.easeIn (documentado en handoff)
- duration_ms: ~180 (≈40% menor que entrada estimada del spring-md)
- nota: springs no tienen token de salida; usar curva + duración explícita o spring más rígido documentado

## Propiedades animadas

### Panel
| propiedad | de | a | token |
|---|---|---|---|
| width | expandido | 68dp collapsed | motion-spring-md |
| opacity (labels) | 1 | 0 | motion-spring-md (paralelo) |

### Navegación por columna
| propiedad | de | a | token |
|---|---|---|---|
| offset columna | nivel N | nivel N±1 | motion-spring-md |

### Ítem
| propiedad | de | a | token |
|---|---|---|---|
| color / decoration | default | pressed / selected | motion-spring-sm |

### Mobile drawer
| propiedad | de | a | token |
|---|---|---|---|
| offset (panel) | -width | 0 | motion-spring-md |
| opacity (scrim) | 0 | 1 | motion-spring-md |
| offset / opacity salida | abierto | cerrado | easeIn ~180ms |

## Haptics

| Momento | Token semántico | Token primitivo | Flutter API |
|---|---|---|---|
| Tap Trigger colapsar/expandir | haptic-action-press | haptic-impact-light | HapticFeedback.lightImpact() |
| Tap ítem navegación (hoja) | haptic-action-press | haptic-impact-light | HapticFeedback.lightImpact() |
| Tap ítem con hijos (drill →) | haptic-action-press | haptic-impact-light | HapticFeedback.lightImpact() |
| Tap atrás (←) | haptic-action-press | haptic-impact-light | HapticFeedback.lightImpact() |
| Animación width / height | none | — | — |
| Hover (no aplica en touch) | none | — | — |

## Triggers

- Trigger panel (tablet/desktop): user-action, persiste collapsed
- Drill a columna siguiente / atrás: user-action; niveles 2 y 3 son páginas, no expand vertical
- Mobile drawer: user-action; cerrar con scrim o back

## Restricciones

- Sin stagger
- Sin slide+scrim en desktop/tablet persistente
- Submenú mobile: columna siguiente, no expand indentado
- Chevron App: solo derecha (Navigation); sin ↑/↓
