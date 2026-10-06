---
component: Toggle Button
component_slug: toggle-button
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: motion-spring-sm
owner: Nehuen Benitez
date: 2026-10-06
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-sm
companion_web_handoff: null
web_parity_token: motion-curve-sm
haptic_semantic_default: haptic-selection-change
haptic_override_allowed: false
haptic_trigger: onChanged
---

## Descripción

Control atómico de App que alterna entre Selected False y True. Refleja el estado reemplazando el ícono outline por el filled de la misma casuística.

## Tokens

### App
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35
- note: "Mismo token en ambos sentidos del toggle Selected"

## Propiedades animadas

### Toggle Button — Selected
| propiedad | de | a | token |
|---|---|---|---|
| icon opacity / glyph | outline (False) | filled (True) | motion-spring-sm |

## Triggers

### Toggle Selected
- evento: tap / onChanged
- tipo: user-action
- delay_ms: 0

### Cierre
- evento: no aplica
- tipo: no aplica
- auto_delay_ms: null

## Estados intermedios

### Selected
- trigger: onChanged
- propiedades: icon opacity / glyph (outline → filled)
- token: motion-spring-sm
- valores: False, True

## Haptics

- semantic_default: haptic-selection-change
- primitive: haptic-selection-click
- flutter_api: HapticFeedback.selectionClick()
- trigger: onChanged
- override_allowed: false

## Coherencia sistémica

- componentes_relacionados: [Switch, Checkbox, Icon Button, Box Selector]
- token_mismo_que: [Switch, Checkbox, Button]
- razon: microinteracción de selección/estado interno; mismo spring default del sistema

## Accesibilidad

- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Referencias

- material_design: https://m3.material.io/components/icon-buttons/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/toggles
- tokens_file: "references/tokens-motion.md"
