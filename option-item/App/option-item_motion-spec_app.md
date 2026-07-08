---
component: Option Item
component_slug: option-item
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: motion-spring-sm
owner: Teresa Mendez
date: 2026-06-23
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-sm
companion_web_handoff: option-item_motion-handoff_web.html
web_parity_token: motion-curve-sm
haptic_semantic_default: haptic-selection-change
haptic_override_allowed: false
haptic_trigger: onOptionSelected
optional_children: [Badge, Thumbnail, Checkbox]
disabled_state: false
---

## Descripcion

Fila atomica de seleccion en App. Estados internos de la fila con motion-spring-sm. Sin hover. Sin disabled. Hijos opcionales con motion documentado por separado.

## Tokens

### App
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

## Propiedades animadas

### OptionItemRow
| propiedad | de | a | token |
|---|---|---|---|
| background-color | enabled | pressed | motion-spring-sm |
| background-color | default | selected | motion-spring-sm |
| background-color | selected | selected+focus | motion-spring-sm |
| opacity (focus ring) | 0 | 1 | motion-spring-sm |

## Triggers

### Pressed
- evento: onTapDown / onTapUp
- tipo: user-action
- delay_ms: 0

### Focus
- evento: focus-visible
- tipo: user-action
- delay_ms: 0

### Selected
- evento: tap que cambia seleccion
- tipo: user-action
- delay_ms: 0

## Estados intermedios

### Selected focused
- trigger: fila selected + focus
- propiedades: background-color, focus ring opacity
- token: motion-spring-sm

## Haptics

- semantic_default: haptic-selection-change
- primitive: haptic-selection-click
- flutter_api: HapticFeedback.selectionClick()
- trigger: onOptionSelected
- override_allowed: false

## Coherencia sistemica

- componentes_relacionados: [Checkbox, Bottom Sheet, Field Phone Number, Search]
- token_mismo_que: [Checkbox, Radio Button, Box Selector]
- razon: seleccion atomica; misma categoria default que otros controles de seleccion.

## Accesibilidad

- flutter_disable_animations: MediaQuery.of(context).disableAnimations

## Referencias

- tokens_file: tokens-motion.md
- related_handoffs: [Checkbox, Bottom Sheet, Field Phone Number]
