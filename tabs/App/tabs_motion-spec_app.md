---
component: Tabs
component_slug: tabs
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: motion-spring-sm
semantic_token_indicator: motion-spring-md
owner: Ezequiel Arguello
date: 2026-07-02
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-sm
companion_web_handoff: tabs_motion-handoff_web.html
web_parity_token: motion-curve-sm
haptic_semantic_default: haptic-selection-change
haptic_override_allowed: false
haptic_trigger: onChanged
---

## Descripción

Control de navegación/selección entre grupos de contenido del mismo nivel jerárquico en App, con variantes Segmented Item y Line Item. Usa motion de estados internos con springs, sin enter/exit de viewport.

## Tokens

### Estados internos (Pressed, Focused, fade de color)
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

### Indicador de selección (movimiento espacial)
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: Mismo token en entrada y salida

## Propiedades animadas

### Tab Item (Segmented / Line) — Pressed
| propiedad | de | a | token |
|---|---|---|---|
| scale | 1.0 | 0.98 | motion-spring-sm |

### Focus ring
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0.0 | 1.0 | motion-spring-sm |

### Indicador de selección — Segmented / Line
| propiedad | de | a | token |
|---|---|---|---|
| position (offset) | posición anterior | posición del tab activo | motion-spring-md |
| size | ancho anterior | ancho del tab activo | motion-spring-md |

### Label / ícono — cambio de selección
| propiedad | de | a | token |
|---|---|---|---|
| color | control | control-selected | motion-spring-sm |
| fontWeight | Regular | Semi Bold | motion-spring-sm |

## Triggers

### pressed
- evento: onTapDown / onTapUp
- tipo: user-action

### focus
- evento: focus-visible
- tipo: user-action

### selected
- evento: onChanged (tap en tab o swipe en TabBarView)
- tipo: user-action
- auto_delay_ms: null

### disabled
- evento: disabled true
- tipo: programmatic
- note: instantáneo

## Estados intermedios

Ninguno.

## Haptics

- semantic_default: haptic-selection-change
- primitive: haptic-selection-click
- flutter_api: HapticFeedback.selectionClick()
- trigger: onChanged
- override_allowed: false

## Coherencia sistémica

- componentes_relacionados: [Button, Box Selector]
- token_mismo_que: [Button (estados internos)]
- razon: Pressed/Focused usan el mismo spring de microinteracción que Button; el indicador usa un spring de categoría distinta (guía) por ser desplazamiento espacial. Haptics comparte token con Switch/Checkbox/Box Selector.

## Accesibilidad

- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Referencias

- material_design: https://m3.material.io/components/tabs
- apple_hig: no aplica
- tokens_file: references/tokens-motion.md
